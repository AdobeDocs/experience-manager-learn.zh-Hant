---
title: 處理HTML5表單提交
description: 建立HTML5表單提交處理常式
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 2%

---


# 處理HTML5表單提交

HTML5表單可提交至AEM中托管的servlet。 提交的資料可在servlet中作為輸入流訪問。 若要提交HTML5表單，您需使用AEM Forms Designer在表單範本上新增「HTTP提交按鈕」

## 建立提交處理常式

可建立簡單的servlet以處理HTML5表單提交。 然後，可使用下列程式碼擷取提交的資料。 本[servlet](assets/html5-submit-handler.zip)已在本教學課程中提供給您。 請使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝[servlet](assets/html5-submit-handler.zip)

第9行的代碼可用於調用J2EE進程。 如果要使用代碼調用J2EE進程，請確保已配置[AdobeLiveCycle客戶端SDK配置](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)。

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


## 配置HTML5表單的提交URL

![submit-url](assets/submit-url.PNG)

* 點選xdp，然後按一下「屬性&#x200B;_->_&#x200B;進階&#x200B;_」_
* 複製http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html並貼到「提交URL」文字欄位中
* 按一下&#x200B;_SaveAndClose_&#x200B;按鈕。

### 在排除路徑中新增登入項目

* 導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
* 搜尋&#x200B;_AdobeGranite CSRF篩選器_
* 在「排除路徑」區段中新增下列項目
* _/content/AemFormsSamples/handlehml5formsubmission_
* 儲存您的變更

### 測試表單

* 點選xdp範本。
* 按一下&#x200B;_預覽_->預覽為HTML
* 在表單中輸入一些資料，然後按一下提交
* 您應該會看到已提交的資料寫入伺服器的stdout.log檔案中

### 其他閱讀

此外，建議您參閱[文章](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)，以從HTML5表單提交產生PDF。




