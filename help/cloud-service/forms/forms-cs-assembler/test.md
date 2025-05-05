---
title: 測試Forms組合器解決方案
description: 執行ExecuteAssemblerService.java以測試解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 匯入Eclipse專案

* 下載並解壓縮[zip檔案](./assets/pdf-manipulation.zip)
* 啟動Eclipse並將專案匯入Eclipse
* 專案在資源資料夾中包含下列資料夾：
   * ddxFiles — 此資料夾包含描述您要產生的輸出的ddx檔案
   * pdf — 此資料夾包含您要組合的pdf檔案和用於測試PDFA公用程式的pdf檔案
   * 認證 — 此資料夾包含pdfa-options.json檔案

![resources-file](./assets/resources.png)

## 測試組合PDF檔案

* 將您的服務認證複製並貼到專案的service_token.json資源檔案中。
* 開啟AssemblePDFFiles.java檔案，並指定要儲存產生的PDF檔案的資料夾
* 開啟ExecuteAssemblerService.java。 設定變數&#x200B;_AEM_FORMS_CS_&#x200B;的值以指向您的執行個體。
* 取消註解適當的行，以測試組裝兩個或多個PDF檔案
* 以Java應用程式執行ExecuteAssemblerService.java

### 測試PDFA公用程式

* 將您的服務認證複製並貼到專案的service_token.json資源檔案中。
* 開啟PDFAUtilities.java檔案，並指定要儲存產生的PDF檔案的資料夾。
* 開啟ExecuteAssemblerService.java。 設定變數&#x200B;_AEM_FORMS_CS_&#x200B;的值以指向您的執行個體。
* 取消註解適當的行以測試PDFA操作。
* 以Java應用程式執行ExecuteAssemblerService.java。



>[!NOTE]
> 第一次執行Java程式時，您會收到HTTP 403錯誤。 若要完成此程式，請確定您已在AEM[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)中將適當的許可權授予技術帳戶使用者。

**AEM Forms使用者**&#x200B;是我用於此課程的角色。
