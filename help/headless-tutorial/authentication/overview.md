---
title: 從外部應用程式向 AEM as a Cloud Service 進行驗證
description: 探索外部應用程式如何使用本機開發存取權杖和服務認證，以程式設計方式透過 HTTP 對 AEM as a Cloud Service 進行驗證並與之互動。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# 對 AEM as a Cloud Service 進行權杖型驗證

AEM 公開各種可以透過無周邊方式進行互動的 HTTP 端點，包括 GraphQL、AEM 內容服務以及 Assets HTTP API。通常，這些無周邊使用者需要向 AEM 進行驗證後，才能存取受保護的內容或動作。為了更方便驗證，AEM 支援對外部應用程式、服務或系統的 HTTP 要求進行權杖型驗證。

在本教學課程中，我們會探索外部應用程式如何以程式設計方式使用存取權杖，並透過 HTTP 向 AEM as a Cloud Service 進行驗證並與其互動。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 必要條件

在繼續本教學課程之前，請確保以下事項已完備：

1. AEM as a Cloud Service 環境的存取權 (最好為開發環境或沙箱方案)
1. AEM as a Cloud Service 環境中 Author 服務 AEM 管理員產品設定檔的會籍
1. 您的 Adobe IMS 組織管理員的會籍或存取權 (必須執行[服務認證](./service-credentials.md)的一次性初始化)
1. 最新的 [WKND 網站](https://github.com/adobe/aem-guides-wknd)已部署至您的雲端服務環境

## 外部應用程式概觀

本教學課程使用從命令列執行的[簡單 Node.js 應用程式](./assets/aem-guides_token-authentication-external-application.zip)，透過 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=zh-Hant) 更新 AEM as a Cloud Service 上的資產中繼資料。

Node.js 應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. Node.js 應用程式是從命令列叫用的
1. 命令列參數定義：
   + 要連線的 AEM as a Cloud Service Author 服務主機 (`aem`)
   + 所含資產已更新的 AEM 資產資料夾 (`folder`)
   + 要更新的中繼資料屬性和值 (`propertyName` 和 `propertyValue`)
   + 提供存取 AEM as a Cloud Service 所需認證之檔案的本機路徑 (`file`)
1. 用於向 AEM 進行驗證的存取權杖取自透過命令列參數 `file` 提供的 JSON 檔案

   a. 若 JSON 檔案 (`file`) 中提供用於非本機開發的服務認證，則會從 Adobe IMS API 中擷取存取權杖
1. 應用程式會使用存取權杖存取 AEM，並列出命令列參數 `folder` 中指定之資料夾內所有資產
1. 針對資料夾中的每項資產，應用程式會根據命令列參數 `propertyName` 和 `propertyValue` 中所指定的屬性名稱和值更新其中繼資料

雖然這個應用程式範例使用 Node.js，但這些互動可以使用不同的程式語言進行開發，及從其他外部系統執行。

## 本機開發存取權杖

本機開發存取權杖是為特定 AEM as a Cloud Service 環境所產生的，並提供對 Author 與 Publish 服務的存取權。這些存取權杖是臨時的，僅在開發透過 HTTP 與 AEM 互動之外部應用程式或系統期間使用。開發人員不需要取得和管理真正的服務認證，而是可以快速輕鬆地自行產生臨時存取權杖，方便他們開發整合功能。

+ [本機開發存取權杖的使用方法](./local-development-access-token.md)

## 服務認證

服務認證是在任何非開發情境 (最明顯的是生產情境) 中所使用的真實認證，可協助外部應用程式或系統透過 HTTP 與 AEM as a Cloud Service 進行驗證和互動。服務認證本身不會發送到 AEM 進行驗證，反而是外部應用程式會使用這些認證產生 JWT，並與 Adobe IMS 的 API 交換&#x200B;_取得_&#x200B;存取權杖，然後使用該權杖來驗證對 AEM as a Cloud Service 發出的 HTTP 要求。

+ [服務認證的使用方法](./service-credentials.md)

## 其他資源

+ [下載應用程式範例](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT 建立和交換的其他程式碼範例
   + [Node.js、Java、Python、C#.NET 和 PHP 程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [以 JavaScript/Axios 為基礎的程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
