---
title: 與外部應AEM用程式as a Cloud Service驗證
description: 瀏覽外部應用程式如何使用本地開發訪問令牌和服AEM務憑據通過HTTP以寫程式方式驗證as a Cloud Service並與其交互。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# 基於令牌的對AEMas a Cloud Service的驗證

公AEM開了各種HTTP端點，這些端點可以無頭方式交互，從GraphQL、內容服AEM務到資產HTTP API。 通常，這些無頭用戶可能需要驗證AEM才能訪問受保護的內容或操作。 為了方便這一點，AEM支援對來自外部應用程式、服務或系統的HTTP請求進行基於令牌的驗證。

在本教程中，請詳細瞭解外部應用程式如何通過訪問令牌通過HTTP以寫程式方式驗證AEM並與as a Cloud Service交互。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 先決條件

在繼續學習本教程之前，請確保已執行以下操作：

1. 對as a Cloud ServiceAEM環境（最好是開發環境或沙盒程式）的訪問
1. as a Cloud Service環境AEM的作者服務管理員產品配AEM置檔案中的成員
1. Adobe IMS組織管理員的成員身份或訪問權限(他們必須執行一次性初始化 [服務憑據](./service-credentials.md))
1. 最新 [WKND站點](https://github.com/adobe/aem-guides-wknd) 部署到您的Cloud Service環境

## 外部應用程式概述

本教程使用 [簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip) 從命令行運行，以在AEMas a Cloud Service上 [資產HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)。

Node.js應用程式的執行流如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令行調用Node.js應用程式
1. 命令行參數定義：
   + 要連AEM接到的as a Cloud Service作者服務主機(`aem`)
   + 更新AEM其資產的資產資料夾(`folder`)
   + 要更新的元資料屬性和值(`propertyName` 和 `propertyValue`)
   + 提供訪問as a Cloud Service所需憑據的檔案的本地路AEM徑(`file`)
1. 用於驗證的訪問令牌AEM是從通過命令行參數提供的JSON檔案派生的 `file`

   a.如果JSON檔案中提供了用於非本地開發的服務憑據(`file`)，訪問令牌從Adobe IMS API中檢索
1. 應用程式使用訪問令牌訪問AEM並列出命令行參數中指定的資料夾中的所有資產 `folder`
1. 對於資料夾中的每個資產，應用程式會根據命令行參數中指定的屬性名稱和值更新其元資料 `propertyName` 和 `propertyValue`

雖然此示例應用程式是Node.js，但這些交互可以使用不同的寫程式語言開發，並從其他外部系統執行。

## 本地開發訪問令牌

為特定as a Cloud Service環境生成本地開發訪AEM問令牌，並提供對作者和發佈服務的訪問。  這些訪問令牌是臨時的，僅用於開發通過HTTP與之交互的外部應用程式或AEM系統。 與開發人員不必獲得和管理惡意服務憑據相比，他們可以快速而輕鬆地自行生成臨時訪問令牌，以便開發其整合。

+ [如何使用本地開發訪問令牌](./local-development-access-token.md)

## 服務憑據

服務憑據是任何非開發方案（最明顯是生產方案）中使用的合適憑據，它有助於外部應用程式或系統通過HTTP驗證到as a Cloud Service並與其交互AEM的能力。 服務憑據本身不會發送AEM到進行身份驗證，而是外部應用程式使用這些憑據生成JWT，該JWT與Adobe IMS的API交換 _為_ 訪問令牌，然後可用於驗證HTTP請求到AEMas a Cloud Service。

+ [如何使用服務憑據](./service-credentials.md)

## 其他資源

+ [下載示例應用程式](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT建立和交換的其他代碼樣例
   + [Node.js、Java、Python、C#.NET和PHP代碼示例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [基於JavaScript/Axios的代碼示例](https://github.com/adobe/aemcs-api-client-lib)
