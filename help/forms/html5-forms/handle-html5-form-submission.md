---
title: 處理HTML5表單提交
description: 建立HTML5表單提交處理常式。
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
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# 處理HTML5表單提交

HTML5 forms可提交至AEM中託管的servlet。 提交資料可在servlet中作為輸入資料流存取。 若要提交HTML5表單，請使用AEM Forms Designer在表單範本上新增「HTTP提交按鈕」。

## 建立您的提交處理常式

一個簡單的servlet可以處理HTML5表單提交。 使用下列程式碼片段擷取提交的資料。 下載本教學課程中提供的[servlet](assets/html5-submit-handler.zip)。 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)安裝[servlet](assets/html5-submit-handler.zip)。

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

如果您打算使用程式碼來叫用J2EE處理序，請確定您已設定[Adobe LiveCycle Client SDK設定](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)。

## 設定HTML5表單的提交URL

![提交URL](assets/submit-url.PNG)

- 開啟xdp並導覽至&#x200B;_屬性_->_進階_。
- 複製http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html並貼到提交URL文字欄位中。
- 按一下&#x200B;_SaveAndClose_&#x200B;按鈕。

### 在排除路徑中新增專案

- 移至[configMgr](http://localhost:4502/system/console/configMgr)。
- 搜尋&#x200B;_Adobe Granite CSRF篩選器_。
- 在排除的路徑區段中新增下列專案： _/content/AemFormsSamples/handlehml5formsubmission_。
- 儲存您的變更。

### 測試表單

- 開啟xdp範本。
- 按一下「_預覽_->以HTML預覽」。
- 在表單中輸入資料，然後按一下提交。
- 請檢查伺服器的stdout.log檔案以取得提交的資料。

### 其他閱讀

如需從HTML5表單提交專案產生PDF的詳細資訊，請參閱本[文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)。

