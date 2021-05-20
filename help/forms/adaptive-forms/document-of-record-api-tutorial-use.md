---
title: 使用API產生含有AEM Forms的記錄檔案
seo-title: 使用API產生含有AEM Forms的記錄檔案
description: 以寫程式方式生成記錄文檔(DOR)
seo-description: 使用API產生含有AEM Forms的記錄檔案
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

---


# 使用API在AEM Forms中產生記錄檔案{#using-api-to-generate-document-of-record-with-aem-forms}

以寫程式方式生成記錄文檔(DOR)

本文說明使用`com.adobe.aemds.guide.addon.dor.DoRService API`以寫程式方式生成&#x200B;**記錄文檔**。 [記錄](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 以最適化表單擷取之資料的PDF版本。

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
1. 請確保已安裝並啟動作為[建立服務用戶文章](service-user-tutorial-develop.md)一部分提供的DevelopingWithServiceUser套件組合
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling Service使用者對應程式服務
1. 請務必在「服務對應」區段中輸入下列項目&#x200B;_DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_
1. [開啟表單](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表單，然後按一下「 View PDF 」
1. 您應會在瀏覽器的新索引標籤中看到DOR


**疑難排解提示**

PDF不會顯示在新的瀏覽器頁簽中：

1. 請確定您未封鎖瀏覽器中的快顯視窗
1. 讓您遵循本[文章](service-user-tutorial-develop.md)中概述的步驟
1. 確保「DevelopingWithServiceUser」套件組合處於&#x200B;*活動狀態*
1. 確保系統用戶「 data 」具有以下節點`/content/usergenerated/content/aemformsenablement`的讀取、修改和建立權限

