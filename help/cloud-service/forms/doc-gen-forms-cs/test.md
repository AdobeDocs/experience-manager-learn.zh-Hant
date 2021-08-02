---
title: 測試解決方案
description: 執行Main.java以測試解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: 適用性表單
topic: 開發
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 1%

---


# 匯入Eclipse專案

下載並解壓縮[zip檔案](./assets/aem-forms-doc-gen.zip)

啟動Eclipse並將專案匯入Eclipse
該項目在資源資料夾中包含以下檔案：

* DataFile1和DataFile2 — 要與模板合併的示例xml資料檔案，以生成最終PDF檔案
* address.xdp - XDP模板
* service_token.json — 您必須將此檔案的內容取代為您的帳戶特定憑證
* options.json — 此檔案中指定的選項用於設定API產生的PDF檔案的屬性

![resources-file](./assets/resource-files.JPG)

## 測試解決方案

* 複製服務憑證並貼到專案的service_token.json資源檔案中。
* 開啟DocumentGeneration.java檔案，並指定要保存生成的PDF檔案的資料夾
* 開啟Main.java。 設定postURL變數的值以指向您的例項。
* 以java應用程式的形式運行Main.java

>[!NOTE]
> 當您第一次執行java程式時，就會收到HTTP 403錯誤。 若要通過此設定，請務必為AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)中的技術帳戶使用者提供[適當的權限。

**AEM Forms** Users是我在本課程中使用的角色。

