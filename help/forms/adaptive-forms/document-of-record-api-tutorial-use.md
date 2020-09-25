---
title: 使用API產生AEM Forms的記錄檔案
seo-title: 使用API產生AEM Forms的記錄檔案
description: 以程式設計方式產生記錄檔案(DOR)
seo-description: 使用API產生AEM Forms的記錄檔案
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# 使用API在AEM Forms中產生記錄檔案 {#using-api-to-generate-document-of-record-with-aem-forms}

以程式設計方式產生記錄檔案(DOR)

本文說明如何使用程 `com.adobe.aemds.guide.addon.dor.DoRService API` 式化方 **式產生記錄** 。 [記錄檔案](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) (Document of Record)是PDF版本的資料，以最適化表單擷取。

1. 以下是程式碼片段。 第一行是DOR服務。
1. 設定DoROptions。
1. 調用DoRService的演算方法，並將DoROptions物件傳遞至演算方法

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

若要在您的本機系統上試用此功能，請依照下列步驟進行

1. [使用封裝管理員下載及安裝文章資產](assets/dor-with-api.zip)
1. 請確定您已安裝並啟動DevelopingWithServiceUser套件，此套件是「建立服務使用者」文章 [的一部分](service-user-tutorial-develop.md)
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling Service User Mapper Service
1. 請確定您在「服務映 _射」部分中有下列項目DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_
1. [開啟表格](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表格，然後按一下「檢視PDF」
1. 您應該會在瀏覽器的新標籤中看到DOR


**疑難排解提示**

PDF不會顯示在新的瀏覽器標籤中：

1. 請確定您未在瀏覽器中封鎖彈出式視窗
1. 讓您遵循本文章所述的步 [驟](service-user-tutorial-develop.md)
1. 請確定「DevelopingWithServiceUser」套件處於作用 *中狀態*
1. 確保系統用戶「 data 」對以下節點具有讀取、修改和建立權限 `/content/usergenerated/content/aemformsenablement`

