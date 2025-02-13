---
title: 處理HTML5表單提交
description: 建立HTML5表單提交處理常式
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# 處理HTML5表單提交

HTML5表單可提交至AEM中託管的servlet。 提交資料可在servlet中作為輸入資料流存取。 若要提交HTML5表單，您必須使用AEM Forms Designer在表單範本上新增「HTTP提交按鈕」

## 建立您的提交處理常式

可以建立簡單的servlet來處理HTML5表單提交。 然後可以使用以下程式碼擷取提交的資料。 此[servlet](assets/html5-submit-handler.zip)已在本教學課程中提供給您使用。 請使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)安裝[servlet](assets/html5-submit-handler.zip)

第9行的程式碼可用來叫用J2EE程式。 如果您打算使用程式碼來叫用J2EE處理序，請確定您已設定[AdobeLiveCycle使用者端SDK組態](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)。

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## 設定HTML5表單的提交URL

![submit-url](assets/submit-url.PNG)

* 點選xdp並按一下&#x200B;_內容_->_進階_
* 複製http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html ，並在「提交URL」文字欄位中貼上
* 按一下&#x200B;_儲存並關閉_&#x200B;按鈕。

### 在排除路徑中新增專案

* 瀏覽至[configMgr](http://localhost:4502/system/console/configMgr)。
* 搜尋&#x200B;_AdobeGranite CSRF篩選器_
* 在「排除的路徑」區段中新增下列專案
* _/content/AemFormsSamples/handlehml5formsubmission_
* 儲存您的變更

### 測試表單

* 點選xdp範本。
* 按一下&#x200B;_預覽_->預覽為HTML
* 在表單中輸入一些資料，然後按一下提交
* 您應該會看到提交的資料已寫入伺服器的stdout.log檔案

### 其他閱讀

也建議使用此[article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)，瞭解如何從HTML5表單提交產生PDF。
