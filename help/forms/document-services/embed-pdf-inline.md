---
title: 顯示內嵌的記錄檔案
description: 將最適化表單資料與XDP範本合併，並使用Document Cloud內嵌PDF API顯示PDF內嵌。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# 顯示DoR內嵌

常見的使用案例是顯示含有表單填寫者所輸入資料的pdf檔案。

為了完成此使用案例，我們已運用[Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)。

已執行下列步驟以完成整合

## 建立自訂元件以內嵌PDF

已建立自訂元件(embed-pdf)以內嵌POST呼叫傳回的pdf。

## 用戶端資源庫

按一下`viewPDF`核取方塊按鈕時，會執行以下程式碼。 我們將最適化表單資料、範本名稱傳遞至端點以產生PDF。 接著會使用內嵌pdf JavaScript資料庫將產生的pdf顯示給表單填充程式。

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

## 產生XDP的範例資料

* 在AEM Forms設計工具中開啟XDP。
* 按一下檔案 | 表單屬性 | 預覽
* 按一下產生預覽資料
* 按一下「產生」
* 提供有意義的檔案名稱，例如&quot;form-data.xml&quot;

## 從xml資料產生XSD

您可以使用任何免費的線上工具，從上個步驟產生的xml資料中[產生XSD](https://www.freeformatter.com/xsd-generator.html)。

## 上傳範本

確定您使用「建立」按鈕將xdp範本上傳到[AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)


## 建立最適化表單

根據上一步的XSD建立最適化表單。
新增索引標籤至最適化。 新增核取方塊元件和embed-pdf元件至此索引標籤
請確定您將核取方塊命名為viewPDF。
設定內嵌pdf元件，如下方熒幕擷圖所示
![embed-pdf](assets/embed-pdf-configuration.png)

**內嵌PDF API金鑰** — 此金鑰可用來內嵌PDF。 此金鑰僅適用於localhost。 您可以建立[您自己的金鑰](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)，並將其與其他網域建立關聯。

**傳回pdf**&#x200B;的端點 — 這是自訂servlet，會將資料與xdp範本合併並傳回pdf。

**範本名稱** — 這是xdp的路徑。 通常儲存在formsanddocuments資料夾下。

**PDF檔案名稱** — 這是將顯示在內嵌pdf元件中的字串。

## 建立自訂servlet

已建立自訂servlet以將資料與XDP範本合併並傳回pdf。 完成此任務的程式碼如下。 自訂servlet是[內嵌pdf套件](assets/embedpdf.core-1.0-SNAPSHOT.jar)的一部分

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


## 在您的伺服器上部署範例

若要在本機伺服器上測試此專案，請遵循下列步驟：

1. [下載並安裝內嵌pdf套件](assets/embedpdf.core-1.0-SNAPSHOT.jar)。
這有servlet可合併資料與XDP範本，並串流回pdf。
1. 使用[AEM ConfigMgr](http://localhost:4502/system/console/configMgr)，在Adobe Granite CSRF篩選器的排除路徑區段中新增路徑/bin/getPDFToEmbed。 建議在生產環境中使用[CSRF保護架構](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=zh-Hant)
1. [匯入使用者端資料庫和自訂元件](assets/embed-pdf.zip)
1. [匯入最適化表單和範本](assets/embed-pdf-form-and-xdp.zip)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. 填寫一些表單欄位
1. 按Tab鍵前往「檢視PDF 」標籤。 選取「檢視pdf」核取方塊。 您應該會看到PDF顯示在填入最適化表單資料的表單中
