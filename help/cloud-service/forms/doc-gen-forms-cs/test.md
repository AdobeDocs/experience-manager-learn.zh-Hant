---
title: 測試解決方案
description: 執行Main.java以測試解決方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 54
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# 匯入Eclipse專案

下載並解壓縮 [zip檔案](./assets/aem-forms-cs-doc-gen.zip)

啟動Eclipse並將專案匯入Eclipse專案包含resources資料夾中的下列檔案：

* DataFile1、DataFile2和DataFile3 — 要與範本合併以產生最終PDF檔案的範例xml資料檔案
* custom_fonts.xdp - XDP範本
* service_token.json — 您必須使用您的帳戶特定認證來取代此檔案的內容
* options.json — 此檔案中指定的選項用於設定API產生的PDF檔案的屬性

![resources-file](./assets/resource-files.png)

## 測試解決方案

* 將您的服務認證複製並貼到專案的service_token.json資源檔案中。
* 開啟DocumentGeneration.java檔案，並指定要儲存產生的PDF檔案的資料夾
* 開啟Main.java。 設定變數postURL的值以指向您的執行個體。
* 以Java應用程式執行Main.java

>[!NOTE]
> 第一次執行Java程式時，您會收到HTTP 403錯誤。 若要完成此程式，請務必提供 [AEM中技術帳戶使用者的適當許可權](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms使用者** 是我用於此課程的角色。
