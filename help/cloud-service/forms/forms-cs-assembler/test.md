---
title: Test解決方案
description: 運行ExecuteAssemblerService.java以test解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# 導入Eclipse項目

* 下載並解壓縮 [zip檔案](./assets/pdf-manipulation.zip)
* 啟動Eclipse並將項目導入Eclipse
* 項目在資源資料夾中包含以下資料夾：
   * ddxFiles — 此資料夾包含描述要生成的輸出的ddx檔案
   * pdf檔案 — 此資料夾包含要匯編的pdf檔案

![資源檔案](./assets/resources.png)

## Test解決方案

* 在項目的service_token.json資源檔案中複製並貼上您的服務憑據。
* 開啟AssemblePDFFiles.java檔案，並指定要在其中保存生成的PDF檔案的資料夾
* 開啟ExecuteAssemblerService.java。 將變數assembleURL的值設定為指向實例。
* 以Java應用程式形式運行ExecuteAssemblerService.java

>[!NOTE]
> 第一次運行java程式時，將會出現HTTP 403錯誤。 為了過去，確保 [對技術帳戶用戶的適當權AEM限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)。

**AEM Forms用戶** 就是我在本課程中所扮演的角色。
