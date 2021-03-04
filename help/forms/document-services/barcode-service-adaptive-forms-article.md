---
title: 使用自適應Forms的條形碼服務
seo-title: 使用自適應Forms的條形碼服務
description: 使用條形碼服務對條形碼進行解碼並從提取的資料中填充表單欄位
seo-description: 使用條形碼服務對條形碼進行解碼並從提取的資料中填充表單欄位
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# 使用Adaptive Forms的條形碼服務{#barcode-service-with-adaptive-forms}

本文將示範如何使用條形碼服務來填入最適化表單。 使用案例如下：

1. 使用者新增具有條碼的PDF做為最適化表單附件
1. 附件的路徑將發送到servlet
1. Servlet解碼條形碼並返回JSON格式的資料
1. 然後使用解碼資料填充自適應表單

下列程式碼會解碼條碼，並使用解碼值填入JSON物件。 然後，servlet會在回應呼叫應用程式時傳回JSON物件。

您可以即時檢視此功能，請造訪[samples portal](https://forms.enablementadobe.com/content/samples/samples.html?query=0)並搜尋條碼服務展示

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

以下是servlet代碼。 當用戶將附件添加到最適化表單時，將調用此servlet。 Servlet會將JSON物件傳回至呼叫的應用程式。 然後，呼叫應用程式會以從JSON物件擷取的值填入最適化表單。

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

以下代碼是Adaptive Form所引用的客戶端庫的一部分。 當使用者將附件新增至最適化表單時，就會觸發此程式碼。 代碼對servlet進行GET調用，其路徑是在請求參數中傳遞的附件。 然後，從servlet調用接收的資料被用於填充自適應表單。

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
>此軟體包中包含的最適化表單是使用AEM Forms6.4構建的。如果您打算在AEM Forms6.3環境中使用此軟體包，請在6.3表單中創AEM建最適化表單

第12行——自訂程式碼以取得服務解析程式。 此套件會隨附於本文章資產中。

第23行——呼叫DocumentServices extractBarCode方法，以取得填入解碼資料的JSON物件

若要在您的系統上執行此程式，請依照下列步驟進行

1. [下載BarcodeService.](assets/barcodeservice.zip) zipp並使AEM用套件管理器匯入
1. [下載並安裝自訂DocumentServices套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下載並安裝DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載範例PDF表格](assets/barcode.pdf)
1. 將瀏覽器指向[示例自適應表單](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上傳提供的範例PDF
1. 您應看到填入資料的表單

