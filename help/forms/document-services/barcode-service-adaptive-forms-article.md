---
title: 條碼服務搭配最適化Forms
description: 使用條碼服務，將條碼解碼並從擷取的資料填入表單欄位。
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# 條碼服務搭配最適化Forms{#barcode-service-with-adaptive-forms}

本文將示範如何使用條碼服務填入最適化表單。 使用案例如下：

1. 使用者新增帶有條碼的PDF作為最適化表單附件
1. 附件的路徑已傳送到servlet
1. 此servlet將條碼解碼並傳回JSON格式的資料
1. 最適化表單接著會使用解碼資料填入

下列程式碼會解碼條碼，並將解碼的值填入JSON物件。 然後，servlet會在回應呼叫的應用程式時傳回JSON物件。



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

以下是servlet程式碼。 當使用者將附件新增至最適化表單時，就會呼叫此servlet。 此servlet會將JSON物件傳回呼叫的應用程式。 然後，呼叫應用程式會使用從JSON物件擷取的值填入調適型表單。

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

下列程式碼為最適化表單所參考的使用者端程式庫的一部分。 當使用者將附件新增到最適化表單時會觸發此程式碼。 程式碼會對servlet進行GET呼叫，並在要求引數中傳遞附件的路徑。 接著會使用從servlet呼叫收到的資料來填入調適型表單。

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
>此套件隨附的最適化表單是使用AEM Forms 6.4建置的。如果您打算在AEM Forms 6.3環境中使用此套件，請在AEM Form 6.3中建立最適化表單

第12行 — 用於取得服務解析程式的自訂程式碼。 此套件組合已納入到本文資產中。

第23行 — 呼叫DocumentServices extractBarCode方法以取得填入已解碼資料的JSON物件

若要在系統上執行此檔案，請遵循下列步驟：

1. [下載BarcodeService.zip](assets/barcodeservice.zip)並使用封裝管理員匯入AEM
1. [下載並安裝自訂DocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下載並安裝DevelopingWithServiceUser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載範例PDF表單](assets/barcode.pdf)
1. 將瀏覽器指向[最適化表單範例](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上傳提供的PDF範例
1. 您應該會看到已填入資料的表單
