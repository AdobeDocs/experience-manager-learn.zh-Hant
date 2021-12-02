---
title: '內聯顯示記錄文檔 '
description: 將最適化表單資料與XDP範本合併，並使用document cloud embed pdf API內嵌顯示PDF。
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9411
source-git-commit: 7f9a7951b2d9bb780d5374f17bb289c38b2e2ae7
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---


# 內嵌顯示DoR

常見的使用案例是顯示PDF檔案，其中包含表單填入器輸入的資料。

為了完成此使用案例，我們已使用 [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

執行下列步驟以完成整合

## 建立自訂元件以內嵌顯示PDF

已建立自訂元件(embed-pdf)，以內嵌POST呼叫傳回的pdf。

## 用戶端資源庫

下列程式碼會在 `viewPDF` 按一下核取方塊按鈕。 我們會將最適化表單資料、範本名稱傳遞至端點，以產生pdf。 然後，使用內嵌的pdf JavaScript程式庫，將產生的pdf顯示在表單填入器中。

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
* 按一下檔案 |表單屬性 |預覽
* 按一下「產生預覽資料」
* 按一下產生
* 提供有意義的檔案名稱，例如&quot;form-data.xml&quot;

## 從xml資料生成XSD

您可以使用任何免費線上工具 [生成XSD](https://www.freeformatter.com/xsd-generator.html) 從上一步驟中產生的xml資料。

## 上傳範本

請務必將xdp範本上傳至 [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) 使用「建立」按鈕


## 建立最適化表單

根據上一步的XSD建立最適化表單。
將新索引標籤新增至適用性。 將核取方塊元件和embed-pdf元件新增至此索引標籤請務必為核取方塊命名viewPDF。
如下方螢幕擷取所示，設定embed-pdf元件
![embed-pdf](assets/embed-pdf-configuration.png)

**內嵌PDFAPI金鑰**  — 這是您用來內嵌pdf的索引鍵。 此鍵只能與localhost一起使用。 您可以建立 [您自己的密鑰](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 並將其與其他網域建立關聯。

**端點傳回pdf**  — 這是自訂Servlet，將合併資料與xdp範本並傳回pdf。

**範本名稱**  — 此為xdp的路徑。 通常會儲存在formsanddocuments資料夾下。

**PDF檔案名**  — 這是將顯示在內嵌pdf元件中的字串。

## 建立自訂Servlet

已建立自訂Servlet，以將資料與XDP範本合併並傳回pdf。 完成此作業的程式碼列於下方。 自訂Servlet是 [內嵌pdf套件](assets/embedpdf.core-1.0-SNAPSHOT.jar)

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

若要在本機伺服器上測試，請執行下列步驟：

1. [下載及安裝內嵌pdf套件組合](assets/embedpdf.core-1.0-SNAPSHOT.jar).
此Servlet可將資料與XDP範本合併，並將PDF串流回流。
1. 使用將路徑新增至「AdobeGranite CSRF篩選器」的「排除路徑」區段中， [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). 在您的生產環境中，建議使用 [CSRF保護框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [匯入用戶端程式庫和自訂元件](assets/embed-pdf.zip)
1. [匯入最適化表單和範本](assets/embed-pdf-form-and-xdp.zip)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. 填寫幾個表單欄位
1. 頁簽到「查看PDF」頁簽。 選取檢視pdf核取方塊。 您應該會看到表單中顯示已填入最適化表單資料的pdf
