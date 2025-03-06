---
title: 範例專案
description: 可在您的環境中匯入和執行的範例專案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---


# 在您的本機環境中測試它

* 匯入專案

   * 下載並解壓縮[範例專案](./assets/formsdocumentservices.zip)
   * 開啟您偏好的&#x200B;**Java開發環境**（IntelliJ IDEA、Eclipse或VS Code），並將專案匯入為Maven專案
* 設定認證

   * 找出檔案`resources/credentials/server_credentials.json`
   * 開啟它並&#x200B;**更新您環境的特定認證**。
   * 請確定其中包含下列專案的有效值：
     `clientId`、`clientSecret`、`adobeIMSV3TokenEndpointURL`和
     `scopes`

* 執行Main類別

   * 導覽至`src/main/java/Main.java`並執行主要方法

* 驗證執行
   * 在終端機視窗中驗證輸出

