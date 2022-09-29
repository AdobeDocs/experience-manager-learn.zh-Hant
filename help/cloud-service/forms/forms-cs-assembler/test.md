---
title: 測試Forms組合器解決方案
description: 運行ExecuteAssemblerService.java以測試解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 匯入Eclipse專案

* 下載並解壓縮 [zip檔案](./assets/pdf-manipulation.zip)
* 啟動Eclipse並將專案匯入Eclipse
* 該項目在資源資料夾中包括以下資料夾：
   * ddxFiles — 此資料夾包含描述要生成的輸出的ddx檔案
   * pdffiles — 此資料夾包含要組合的pdf檔案和用於測試PDFA應用程式的pdf檔案
   * 憑證 — 此資料夾包含pdfa-options.json檔案

![resources-file](./assets/resources.png)

## 測試組裝PDF檔案

* 複製服務憑證並貼到專案的service_token.json資源檔案中。
* 開啟AssemblePDFFiles.java檔案，並指定要保存生成的PDF檔案的資料夾
* 開啟ExecuteAssemblerService.java。 設定變數的值 _AEM_FORMS._CS_ 指向您的例項。
* 取消註解適當的行以測試裝配兩個或更多PDF檔案
* 以java應用程式的形式運行ExecuteAssemblerService.java

### 測試PDFA實用程式

* 複製服務憑證並貼到專案的service_token.json資源檔案中。
* 開啟PDFAUtilities.java檔案，並指定您要儲存所產生PDF檔案的資料夾。
* 開啟ExecuteAssemblerService.java。 設定變數的值 _AEM_FORMS._CS_ 指向您的例項。
* 取消註解適當的行以測試PDFA操作。
* 以java應用程式的形式運行ExecuteAssemblerService.java。



>[!NOTE]
> 當您第一次執行java程式時，就會收到HTTP 403錯誤。 若要通過，請務必將 [AEM中技術帳戶使用者的適當權限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms使用者** 是我在本課程中所扮演的角色。
