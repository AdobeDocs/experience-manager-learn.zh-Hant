---
title: 測試解決方案
description: 執行Main.java以測試解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# 匯入Eclipse專案

下載並解壓縮 [zip檔案](./assets/aem-forms-cs-doc-gen.zip)

啟動Eclipse並將專案匯入Eclipse專案資源資料夾中包含下列檔案：

* DataFile1、DataFile2和DataFile3 — 要與模板合併的示例xml資料檔案，以生成最終PDF檔案
* custom_fonts.xdp - XDP範本。
* service_token.json — 您必須將此檔案的內容取代為您的帳戶特定憑證
* options.json — 此檔案中指定的選項用於設定API產生的PDF檔案的屬性

![resources-file](./assets/resource-files.png)

## 測試解決方案

* 複製服務憑證並貼到專案的service_token.json資源檔案中。
* 開啟DocumentGeneration.java檔案，並指定要保存生成的PDF檔案的資料夾
* 開啟Main.java。 設定postURL變數的值以指向您的例項。
* 以java應用程式的形式運行Main.java

>[!NOTE]
> 當您第一次執行java程式時，就會收到HTTP 403錯誤。 若要通過，請務必將 [AEM中技術帳戶使用者的適當權限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms使用者** 是我在本課程中所扮演的角色。

