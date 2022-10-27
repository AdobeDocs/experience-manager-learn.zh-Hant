---
title: 使用最適化表單資料產生互動式DoR
description: 將最適化表單資料與XDP範本合併，以產生可下載的pdf
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

常見的使用案例是能夠下載具有最適化表單資料的互動式DoR。 下載的DoR隨後將使用Adobe Acrobat或Adobe Reader完成。

## 適用性表單不以XSD結構為基礎

如果您的XDP和適用性表單並非以任何結構為基礎，請依照下列步驟產生互動式記錄檔案。

### 建立最適化表單

建立最適化表單，並確認最適化表單欄位名稱與xdp範本中的欄位名稱相同。
記下xdp範本的根元素名稱。
![根元素](assets/xfa-root-element.png)

### 客戶端庫

觸發下載PDF按鈕時，會觸發下列程式碼

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

## 以XSD結構為基礎的適用性表單

如果您的xdp不是以XSD為基礎，請依照下列步驟建立XSD（結構），以便建立最適化表單

### 產生XDP的範例資料

* 在AEM Forms設計工具中開啟XDP。
* 按一下檔案 |表單屬性 |預覽
* 按一下「產生預覽資料」
* 按一下產生
* 提供有意義的檔案名稱，例如&quot;form-data.xml&quot;

### 從xml資料生成XSD

您可以使用任何免費線上工具 [生成XSD](https://www.freeformatter.com/xsd-generator.html) 從上一步驟中產生的xml資料。

### 建立最適化表單

根據上一步的XSD建立最適化表單。 將表單關聯以使用客戶端庫「irs」。 此客戶端庫具有對Servlet進行POST調用的代碼，該Servlet將PDF返回給調用應用程式。當 _下載PDF_ 已點按

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



## 建立自訂Servlet

建立自訂Servlet，將資料與XDP範本合併並傳回pdf。 完成此作業的程式碼列於下方。 自訂Servlet是 [AEMFormsDocumentServices.core-1.0-SNAPSHOT套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar))。

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

在范常式式碼中，我們會從要求物件中擷取xdp名稱和其他參數。 如果表單不是基於XSD，則會建立要與xdp合併的xml文檔。如果表單是基於XSD，我們只需從適用性表單提交資料中提取相應的節點，以生成要與xdp模板合併的xml文檔。

## 在伺服器上部署示例

若要在本機伺服器上測試，請執行下列步驟：

1. [下載並安裝DevelopingWithServiceUser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 在Apache Sling Service使用者對應程式服務DevelopingWithServiceUser.core:getformsresourceresolver=fd-service中新增下列項目
1. [下載和安裝自定義DocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). 這個Servlet可將資料與XDP範本合併，並將pdf串流回
1. [匯入用戶端程式庫](assets/generate-interactive-dor-client-lib.zip)
1. [匯入文章資產（適用性表單、XDP範本和XSD）](assets/generate-interactive-dor-sample-assets.zip)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 填寫幾個表單欄位。
1. 按一下「下載PDF」以取得PDF。 您可能需要等待幾秒，PDF才能下載。

>[!NOTE]
>
>您可以嘗試使用相同的使用案例 [非xsd型適用性表單](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). 請務必將適當的參數傳遞至位於irs clientlib的streampdf.js中的post端點。
