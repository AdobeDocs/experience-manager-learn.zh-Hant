---
title: 使用監控資料夾機制產生適用於列印頻道的互動通訊檔案
seo-title: 使用監控資料夾機制產生適用於列印頻道的互動通訊檔案
description: 使用監看資料夾產生列印頻道檔案
seo-description: 使用監看資料夾產生列印頻道檔案
feature: 交互通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# 使用監控資料夾機制產生適用於列印頻道的互動通訊檔案

在您設計並測試了列印渠道檔案後，通常需要進行REST呼叫或使用監控資料夾機制產生列印檔案，以產生檔案。

本文說明使用監控資料夾機制產生列印渠道檔案的使用案例。

將檔案拖放至監視資料夾時，會執行與監視資料夾關聯的指令碼。 此指令碼會在以下文章中說明。

拖放到監視資料夾的檔案具有下列結構。 代碼將為XML文檔中列出的所有帳戶號生成語句。

&lt;accountnumbers>

&lt;accountnumber>郵編：509840&lt;/accountnumber>

&lt;accountnumber>郵編：948576&lt;/accountnumber>

&lt;accountnumber>郵編：398762&lt;/accountnumber>

&lt;accountnumber>郵編：291723&lt;/accountnumber>

&lt;/accountnumbers>

下列程式碼清單執行下列動作：

第1行- InteractiveCommunicationsDocument的路徑

第15-20行：從XML檔案取得帳戶號碼清單，並放入受監視的資料夾

第24 - 25行：取得與檔案相關的PrintChannelService和Print Channel。

第30行：將帳戶編號作為關鍵元素傳遞至表單資料模型。

第32-36行：為要生成的文檔設定資料選項。

第38行：演算檔案。

第39-40行——將生成的文檔保存到檔案系統。

表單資料模型的REST端點期望ID作為輸入參數。 此ID會映射至稱為accountnumber的「請求屬性」，如以下螢幕擷取所示。

![請求屬性](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**若要在本機系統上測試此功能，請依照下列指示進行：**

* 如本[文章所述，設定Tomcat。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat具有生成樣例資料的war檔案。
* 按照本[文章](/help/forms/adaptive-forms/service-user-tutorial-develop.md)中所述，設定服務即系統用戶。
請確定此系統用戶對以下節點具有讀取權限。 要將權限登錄給[用戶admin](https://localhost:4502/useradmin)並搜索系統用戶「data」，並通過跳至權限頁籤來授予對以下節點的讀取權限
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* 使用包管理器將以下包AEM導入。 此套件包含下列項目：


* [互動式通訊檔案範例](assets/retirementstatementprint.zip)
* [Watched資料夾指令碼](assets/printchanneldocumentusingwatchedfolder.zip)
* [資料來源組態](assets/datasource.zip)

* 開啟/etc/fd/watchfolder/scripts/PrintPDF.ecma檔案。 請確定第1行中的interactiveCommunicationsDocument路徑指向您要列印的正確檔案

* 根據第2行上的首選項修改saveLocation

* 使用下列內容建立accountnumbers.xml檔案

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* 將accountnumbers.xml拖放至C:\RenderPrintChannel\input folder目錄。

* 產生的PDF檔案會如ecma指令碼中所指定，寫入saveLocation。

>[!NOTE]
>
>如果您打算在非Windows作業系統上使用此功能，請導覽至
>
>/etc/fd/watchfolder /config/PrintChannelDocument，並依您的偏好設定變更folderPath

