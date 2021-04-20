---
title: 使用API與AEM Forms一起產生記錄檔案
seo-title: 使用API與AEM Forms一起產生記錄檔案
description: 以程式設計方式產生記錄檔案(DOR)
seo-description: 使用API與AEM Forms一起產生記錄檔案
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 使用API在AEM Forms生成記錄文檔{#using-api-to-generate-document-of-record-with-aem-forms}

以程式設計方式產生記錄檔案(DOR)

本文說明使用`com.adobe.aemds.guide.addon.dor.DoRService API`以程式設計方式產生&#x200B;**記錄檔案**。 [檔案記](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 錄在最適化表單中擷取的PDF版本資料。

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
1. 請確定您已安裝並啟動作為[「建立服務使用者」文章](service-user-tutorial-develop.md)一部分提供的DevelopingWithServiceUser套件
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling Service User Mapper Service
1. 請確定「服務映射」部分中的以下條目&#x200B;_DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_
1. [開啟表格](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表格，然後按一下「檢視PDF」
1. 您應該會在瀏覽器的新標籤中看到DOR


**疑難排解提示**

PDF不會顯示在新的瀏覽器標籤中：

1. 請確定您未在瀏覽器中封鎖彈出式視窗
1. 讓您遵循本[文章](service-user-tutorial-develop.md)中概述的步驟
1. 確定「DevelopingWithServiceUser」捆綁包處於&#x200B;*活動狀態*
1. 確保系統用戶「 data 」對以下節點`/content/usergenerated/content/aemformsenablement`具有讀取、修改和建立權限

