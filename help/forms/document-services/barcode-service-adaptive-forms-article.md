---
title: 使用適用性Forms的條碼服務
description: 使用條碼服務來解碼條碼，並從擷取的資料填入表單欄位。
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 使用適用性Forms的條碼服務{#barcode-service-with-adaptive-forms}

本文將示範如何使用條碼服務填入最適化表單。 使用案例如下：

1. 使用者新增含有條碼作為最適化表單附件的PDF
1. 附件的路徑會傳送至servlet
1. Servlet解碼了條形碼並以JSON格式返回資料
1. 然後使用解碼的資料填入最適化表單

下列程式碼會解碼條碼，並使用解碼值填入JSON物件。 然後，Servlet會在回應呼叫應用程式時傳回JSON物件。



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

以下是Servlet程式碼。 當使用者新增附件至適用性表單時，會呼叫此Servlet。 Servlet會將JSON物件傳回給呼叫應用程式。 接著，呼叫應用程式會以從JSON物件擷取的值填入最適化表單。

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

下列程式碼是「適用性表單」參考的用戶端程式庫的一部分。 當使用者將附件新增至適用性表單時，就會觸發此程式碼。 程式碼會對servlet進行GET呼叫，且要求參數中會傳入附件的路徑。 接著，從servlet呼叫接收的資料將用來填入最適化表單。

```
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
>此套件隨附的適用性表單是使用AEM Forms 6.4建置的。如果您想在AEM Forms 6.3環境中使用此套件，請在AEM表單6.3中建立適用性表單

第12行 — 取得服務解析程式的自訂程式碼。 此套件組合包含在本文資產中。

第23行 — 呼叫DocumentServices extractBarCode方法，以取得填入已解碼資料的JSON物件

要在您的系統上運行此程式，請執行以下步驟

1. [下載BarcodeService.zip](assets/barcodeservice.zip) 並使用套件管理器匯入至AEM
1. [下載和安裝自訂DocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下載並安裝DevelopingWithServiceUser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載範例PDF表單](assets/barcode.pdf)
1. 將瀏覽器指向 [範例適用性表單](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上傳提供的範例PDF
1. 您應該會看到填入了資料的表單
