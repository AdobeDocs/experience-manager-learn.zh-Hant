---
title: TestForms匯編器解決方案
description: 運行ExecuteAssemblerService.java以test解決方案
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

# 導入Eclipse項目

* 下載並解壓縮 [zip檔案](./assets/pdf-manipulation.zip)
* 啟動Eclipse並將項目導入Eclipse
* 項目在資源資料夾中包含以下資料夾：
   * ddxFiles — 此資料夾包含描述要生成的輸出的ddx檔案
   * pdffiles — 此資料夾包含要匯編的pdf檔案和pdf檔案以testPDFA實用程式
   * 憑據 — 此資料夾包含pdfa-options.json檔案

![資源檔案](./assets/resources.png)

## Test裝配PDF檔案

* 在項目的service_token.json資源檔案中複製並貼上您的服務憑據。
* 開啟AssemblePDFFiles.java檔案，並指定要在其中保存生成的PDF檔案的資料夾
* 開啟ExecuteAssemblerService.java。 設定變數的值 __AEMFORMS._CS_ 來指向你的實例。
* 取消注釋相應行以test裝配兩個或多個PDF檔案
* 以Java應用程式形式運行ExecuteAssemblerService.java

### TestPDFA實用程式

* 在項目的service_token.json資源檔案中複製並貼上您的服務憑據。
* 開啟PDFAUtilities.java檔案，並指定要在其中保存生成的PDF檔案的資料夾。
* 開啟ExecuteAssemblerService.java。 設定變數的值 __AEMFORMS._CS_ 來指向你的實例。
* 取消對相應行的注釋以testPDFA操作。
* 將ExecuteAssemblerService.java作為java應用程式運行。



>[!NOTE]
> 第一次運行java程式時，將會出現HTTP 403錯誤。 為了過去，確保 [對技術帳戶用戶的適當權AEM限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)。

**AEM Forms用戶** 就是我在本課程中所扮演的角色。
