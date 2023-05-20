---
title: 具有自適應Forms的條形碼服務
description: 使用條形碼服務對條形碼進行解碼並從提取的資料填充表單域。
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# 具有自適應Forms的條形碼服務{#barcode-service-with-adaptive-forms}

本文將演示使用條形碼服務填充自適應表單。 使用案例如下：

1. 用戶將PDF添加為條形碼作為自適應表單附件
1. 附件的路徑將發送到servlet
1. Servlet解碼條形碼並以JSON格式返回資料
1. 然後使用解碼資料填充自適應格式

以下代碼對條形碼進行解碼，並使用解碼值填充JSON對象。 然後，servlet在對調用應用程式的響應中返回JSON對象。



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

以下是servlet代碼。 當用戶向自適應表單中添加附件時，將調用此servlet。 Servlet將JSON對象返回給調用的應用程式。 然後調用應用程式使用從JSON對象提取的值填充自適應表單。

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

以下代碼是Adaptive Form引用的客戶端庫的一部分。 當用戶將附件添加到自適應表單時，將觸發此代碼。 該代碼對請求參數中傳遞的附件路徑進行GET調用。 然後使用從servlet調用接收的資料來填充自適應表單。

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>此軟體包中包含的自適應表單是使用AEM Forms6.4構建的。如果您打算在AEM Forms6.3環境中使用此包，請在表單6.3中創AEM建自適應表單

第12行 — 獲取服務解析程式的自定義代碼。 此捆綁包作為本文章資產的一部分包含。

第23行 — 調用DocumentServices extractBarCode方法以獲取填充有已解碼資料的JSON對象

要在您的系統上運行此程式，請執行以下步驟：

1. [下載BarcodeService.zip](assets/barcodeservice.zip) 並使用包AEM管理器導入
1. [下載並安裝自定義DocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下載並安裝DevelopingWithServiceUser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載示例PDF表](assets/barcode.pdf)
1. 將瀏覽器指向 [樣本自適應格式](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上載提供的示例PDF
1. 您應看到填充了資料的表單
