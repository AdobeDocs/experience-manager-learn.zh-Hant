---
title: Test解決方案
description: 運行Main.java以test解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
source-git-commit: 47d36e472719049de1346c5f0bba010c9af4e039
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 導入Eclipse項目

下載並解壓縮 [zip檔案](./assets/aem-forms-cs-doc-gen.zip)

啟動Eclipse並將項目導入Eclipse項目資源資料夾中包含以下檔案：

* DataFile1、DataFile2和DataFile3 — 要與模板合併的示例xml資料檔案，以生成最終PDF檔案
* custom_fonts.xdp - XDP模板。
* service_token.json — 您必須用帳戶特定憑據替換此檔案的內容
* options.json — 此檔案中指定的選項用於設定API生成的PDF檔案的屬性

![資源檔案](./assets/resource-files.png)

## Test解決方案

* 在項目的service_token.json資源檔案中複製並貼上您的服務憑據。
* 開啟DocumentGeneration.java檔案，並指定要在其中保存生成的PDF檔案的資料夾
* 開啟Main.java。 設定變數postURL的值以指向實例。
* 將Main.java作為Java應用程式運行

>[!NOTE]
> 第一次運行java程式時，將會出現HTTP 403錯誤。 為了過去，確保 [對技術帳戶用戶的適當權AEM限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)。

**AEM Forms用戶** 就是我在本課程中所扮演的角色。
