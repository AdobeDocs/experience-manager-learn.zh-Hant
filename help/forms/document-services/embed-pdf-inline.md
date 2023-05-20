---
title: 內聯顯示記錄文檔
description: 將自適應表單資料與XDP模板合併，並使用文檔雲嵌入pdf API以內嵌方式顯示PDF。
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 內聯顯示DoR

常見的使用情形是顯示帶有表單填充輸入的資料的pdf文檔。

要完成此使用案例，我們利用 [Adobe PDF嵌入式API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)。

執行了以下步驟以完成整合

## 建立自定義元件以內聯顯示PDF

已建立自定義元件(embed-pdf)以嵌入POST調用返回的pdf。

## 用戶端資源庫

在 `viewPDF` 複選框。 我們將自適應表單資料、模板名稱傳遞給端點以生成pdf。 然後，使用嵌入的pdf JavaScript庫將生成的pdf顯示到表單填充符。

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## 為XDP生成示例資料

* 開啟AEM Forms設計師的XDP。
* 按一下「檔案」 |窗體屬性 |預覽
* 按一下「生成預覽資料」
* 按一下「生成」
* 提供有意義的檔案名，如&quot;form-data.xml&quot;

## 從xml資料生成XSD

可以使用任何免費線上工具 [生成XSD](https://www.freeformatter.com/xsd-generator.html) 上一步中生成的xml資料。

## 上載模板

確保將xdp模板上載到 [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) 使用「建立」按鈕


## 建立自適應窗體

根據上一步中的XSD建立自適應表單。
將新頁籤添加到自適應頁籤。 將複選框元件和embed-pdf元件添加到此頁籤確保將複選框視圖命名為PDF。
配置embed-pdf元件，如下面螢幕快照所示
![嵌入 — pdf](assets/embed-pdf-configuration.png)

**嵌入PDFAPI密鑰**  — 這是可用於嵌入pdf的鍵。 此密鑰僅與localhost一起使用。 您可以建立 [你自己的鑰匙](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 並與其他域關聯。

**終結點返回pdf**  — 這是自定義Servlet，它將資料與xdp模板合併並返回pdf。

**模板名稱**  — 這是通往xdp的路徑。 通常，它儲存在formsanddocuments資料夾下。

**PDF檔案名**  — 這是將出現在embed pdf元件中的字串。

## 建立自定義Servlet

已建立自定義Servlet，以將資料與XDP模板合併並返回pdf。 下面列出了完成此操作的代碼。 自定義Servlet是 [嵌入式pdf包](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
         response.setContentType("application/pdf");
         response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
         response.setContentLength((int) fileInputStream.available());
         OutputStream responseOutputStream = response.getOutputStream();
         int bytes;
         while ((bytes = fileInputStream.read()) != -1) {
            responseOutputStream.write(bytes);
         }
         responseOutputStream.flush();
         responseOutputStream.close();

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## 在伺服器上部署示例

要在本地伺服器上test此功能，請執行以下步驟：

1. [下載並安裝嵌入式pdf包](assets/embedpdf.core-1.0-SNAPSHOT.jar)。
這使Servlet能夠將資料與XDP模板合併，並將pdf流返回。
1. 使用將路徑/bin/getPDFToEmbed添加到Adobe花崗岩CSRF過濾器的排除路徑部分 [ConfigMgrAEM](http://localhost:4502/system/console/configMgr)。 在您的生產環境中，建議使用 [CSRF保護框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [導入客戶端庫和自定義元件](assets/embed-pdf.zip)
1. [導入自適應表單和模板](assets/embed-pdf-form-and-xdp.zip)
1. [預覽自適應窗體](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. 填寫幾個表單域
1. 頁籤。 選中「查看pdf」複選框。 您應看到表單中顯示的pdf，其中填入了自適應表單資料
