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
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# 處理HTML5表單提交

HTML5表格可提交至中代管的ServletAEM。 提交的資料可以作為輸入流在servlet中訪問。 若要送出HTML5表格，您必須使用AEM Forms設計人員在表格範本上新增「HTTP提交按鈕」

## 建立您的提交處理常式

可以建立一個簡單的servlet來處理HTML5表單提交。 然後，可使用下列程式碼擷取提交的資料。 本教學課程提供此[servlet](assets/html5-submit-handler.zip)。 請使用[軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝[servlet](assets/html5-submit-handler.zip)

第9行的程式碼可用來叫用J2EE程式。 如果您要使用程式碼來叫用J2EE程式，請確定您已設定[AdobeLiveCycle用戶端SDK設定](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)。

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

* 點選xdp並按一下「屬性&#x200B;__->_進階_」
* 複製http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html，並將它貼在「提交URL」文字欄位中
* 按一下&#x200B;_SaveAndClose_&#x200B;按鈕。

### 在排除路徑中新增項目

* 導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
* 搜索&#x200B;_Adobe花崗岩CSRF過濾器_
* 在「排除的路徑」區段中新增下列項目
* _/content/AemFormsSamples/handlehml5formsubmission_
* 儲存變更

### 測試表單

* 點選xdp範本。
* 按一下「預覽為HTML」__
* 在表單中輸入一些資料，然後按一下「提交」
* 您應看到已提交的資料寫入伺服器的stdout.log檔案

### 其他閱讀

此外，建議使用此[文章](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)，說明如何從HTML5表單提交產生PDF。




