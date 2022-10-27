---
title: 使用API產生含有AEM Forms的記錄檔案
description: 以寫程式方式生成記錄文檔(DOR)
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---

# 使用API在AEM Forms中產生記錄檔案 {#using-api-to-generate-document-of-record-with-aem-forms}

以寫程式方式生成記錄文檔(DOR)

本文說明的使用 `com.adobe.aemds.guide.addon.dor.DoRService API` 產生 **記錄檔案** 寫程式。 [記錄檔案](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 是在適用性表單中擷取的資料的PDF版本。

1. 以下是程式碼片段。 第一行取得DOR服務。
1. 設定DoROptions。
1. 調用DoRService的呈現方法，並將DoROptions對象傳遞到呈現方法

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

要在本地系統上嘗試，請執行以下步驟

1. [使用封裝管理程式下載及安裝文章資產](assets/dor-with-api.zip)
1. 請確定您已安裝並啟動作為 [建立服務用戶文章](service-user-tutorial-develop.md)
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling Service使用者對應程式服務
1. 請務必輸入下列項目 _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ （在「服務映射」部分）
1. [開啟表單](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表單，然後按一下「 ViewPDF」
1. 您應會在瀏覽器的新索引標籤中看到DOR


**疑難排解提示**

PDF未顯示在新的瀏覽器標籤中：

1. 請確定您未封鎖瀏覽器中的快顯視窗
1. 讓您遵循以下概述的步驟 [文章](service-user-tutorial-develop.md)
1. 確認「DevelopingWithServiceUser」套件組合位於 *活動狀態*
1. 確保系統用戶「 data 」具有以下節點的讀取、修改和建立權限 `/content/usergenerated/content/aemformsenablement`
