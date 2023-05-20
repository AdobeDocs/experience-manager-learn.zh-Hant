---
title: 使用自適應表單資料生成互動式DoR
description: 將自適應表單資料與XDP模板合併以生成可下載的pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# 下載互動式DoR

通用的使用案例是能夠下載具有自適應表單資料的互動式DoR。 下載的DoR將使用Adobe Acrobat或Adobe Reader完成。

## 自適應表單不基於XSD架構

如果您的XDP和自適應表單不是基於任何架構的，請按照以下步驟生成互動式記錄文檔。

### 建立自適應窗體

建立自適應表單，並確保自適應表單欄位名稱與xdp模板中的欄位名稱相同。
記下xdp模板的根元素名稱。
![根元素](assets/xfa-root-element.png)

### 客戶端庫

觸發「下載PDF」按鈕時觸發以下代碼

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```

## 基於XSD架構的自適應表單

如果您的xdp不基於XSD，請按照以下步驟建立XSD（架構），您將在該XSD（架構）上建立自適應表單

### 為XDP生成示例資料

* 開啟AEM Forms設計師的XDP。
* 按一下「檔案」 |窗體屬性 |預覽
* 按一下「生成預覽資料」
* 按一下「生成」
* 提供有意義的檔案名，如&quot;form-data.xml&quot;

### 從xml資料生成XSD

可以使用任何免費線上工具 [生成XSD](https://www.freeformatter.com/xsd-generator.html) 上一步中生成的xml資料。

### 建立自適應窗體

根據上一步中的XSD建立自適應表單。 關聯表單以使用客戶端庫「irs」。 此客戶端庫具有用於對Servlet進行POST調用的代碼，該Servlet將PDF返回給調用的應用程式當 _下載PDF_ 按一下

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```



## 建立自定義Servlet

建立將資料與XDP模板合併並返回pdf的自定義Servlet。 下面列出了完成此操作的代碼。 自定義Servlet是 [AEMFormsDocumentServices.core-1.0-SNAPSHOT捆綁包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar))。

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
            response.setContentLength((int) fileInputStream.available());
            OutputStream responseOutputStream = response.getOutputStream();
            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }
            responseOutputStream.flush();
            responseOutputStream.close();

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

在示例代碼中，我們從請求對象中提取了xdp Name和其他參數。 如果表單不是基於XSD的，則建立要與xdp合併的xml文檔。如果表單是基於XSD的，我們只需從自適應表單提交資料中提取相應的節點，以生成要與xdp模板合併的xml文檔。

## 在伺服器上部署示例

要在本地伺服器上test此功能，請執行以下步驟：

1. [下載並安裝DevelopingWithServiceUser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 在Apache Sling服務用戶映射器服務DevelopingWithServiceUser.core:getformsresourceresolver=fd-service中添加以下項
1. [下載並安裝自定義DocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。 這使Servlet能夠將資料與XDP模板合併，並將pdf流返回
1. [導入客戶端庫](assets/generate-interactive-dor-client-lib.zip)
1. [導入文章資產（自適應表單、XDP模板和XSD）](assets/generate-interactive-dor-sample-assets.zip)
1. [預覽自適應窗體](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 填寫幾個表單域。
1. 按一下「下載PDF」獲取PDF。 您可能需要等待幾秒鐘才能下載PDF。

>[!NOTE]
>
>您可以嘗試使用 [基於xsd的自適應窗體](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled)。 確保將相應參數傳遞到位於irs客戶端庫中的streampdf.js中的後端點。
