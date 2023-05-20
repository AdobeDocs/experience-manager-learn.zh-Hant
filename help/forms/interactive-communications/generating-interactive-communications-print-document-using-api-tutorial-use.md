---
title: 使用監視資料夾機制為打印通道生成互動式通信文檔
seo-title: Generating Interactive Communications Document for print channel using watch folder mechanism
description: 使用監視資料夾生成打印渠道文檔
seo-description: Use watched folder to generate print channel documents
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 0%

---

# 使用監視資料夾機制為打印通道生成互動式通信文檔

設計並測試打印渠道文檔後，通常需要通過進行REST調用來生成文檔，或使用監視資料夾機制生成打印文檔。

本文介紹了利用監視資料夾機制生成打印通道文檔的使用案例。

將檔案放入監視資料夾時，將執行與監視資料夾關聯的指令碼。 下面的文章對此指令碼進行了說明。

被丟棄到受監視資料夾的檔案具有以下結構。 代碼將為XML文檔中列出的所有帳號生成語句。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下代碼列出如下：

第1行 — InteractiveCommunicationsDocument的路徑

第15-20行：從XML文檔中獲取已放入監視資料夾的帳號清單

第24 -25行：獲取與文檔關聯的PrintChannelService和Print Channel。

第30行：將帳號作為關鍵元素傳遞到表單資料模型。

第32-36行：為要生成的文檔設定資料選項。

38號線：呈現文檔。

行39-40 — 將生成的文檔保存到檔案系統。

表單資料模型的REST端點需要ID作為輸入參數。 此id映射到名為accountnumber的請求屬性，如下面的螢幕快照所示。

![requestation（請求屬性）](assets/requestattributeprintchannel.gif)

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


**要在本地系統上test此功能，請按照以下說明操作：**

* 按照本中所述設定Tomcat [文章。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat具有生成示例資料的war檔案。
* 設定服務（即系統用戶），如本中所述 [文章](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。
確保此系統用戶對以下節點具有讀取權限。 將權限登錄到 [用戶管理員](https://localhost:4502/useradmin) 並搜索系統用戶「資料」，並通過在「權限」頁籤上開啟頁籤，為以下節點授予讀取權限
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /內容/表單/af
* 使用包管理器將以AEM下包導入。 此包包含以下內容：


* [互動式通信文檔示例](assets/retirementstatementprint.zip)
* [受監視資料夾指令碼](assets/printchanneldocumentusingwatchedfolder.zip)
* [資料來源組態](assets/datasource.zip)

* 開啟/etc/fd/watchfolder/scripts/PrintPDF.ecma檔案。 確保第1行中的interactiveCommunicationsDocument的路徑指向要打印的正確文檔

* 根據第2行上的首選項修改saveLocation

* 建立具有以下內容的accountnumbers.xml檔案

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


* 將accountnumbers.xml放入C:\RenderPrintChannel\input folder目錄中。

* 生成的PDF檔案將寫入ecma指令碼中指定的saveLocation。

>[!NOTE]
>
>如果您計畫在非Windows作業系統上使用此選項，請導航至
>
>/etc/fd/watchfolder /config/PrintChannelDocument並根據您的首選項更改folderPath
