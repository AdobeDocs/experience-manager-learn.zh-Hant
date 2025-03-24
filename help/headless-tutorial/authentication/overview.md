---
title: 從外部應用程式向AEM as a Cloud Service進行驗證
description: 探索外部應用程式如何使用本機開發存取權杖和服務認證，以程式設計方式透過HTTP驗證並與AEM as a Cloud Service互動。
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# 對AEM as a Cloud Service的權杖型驗證

AEM公開各種能以無周邊方式互動的HTTP端點，從GraphQL、AEM Content Services到Assets HTTP API。 通常，這些Headless消費者可能需要向AEM驗證以存取受保護的內容或操作。 為方便起見，AEM支援對來自外部應用程式、服務或系統的HTTP請求進行權杖式驗證。

在本教學課程中，請深入探索外部應用程式可如何以程式設計方式使用存取權杖並透過HTTP向AEM as a Cloud Service驗證及互動。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 必要條件

在參加本教學課程之前，請確定已具備下列條件：

1. 存取AEM as a Cloud Service環境（最好是開發環境或沙箱計畫）
1. AEM as a Cloud Service環境作者服務的成員資格AEM管理員產品設定檔
1. 您的Adobe IMS組織管理員的成員資格或存取權（他們將必須執行[服務認證](./service-credentials.md)的一次性初始化）
1. 最新[WKND網站](https://github.com/adobe/aem-guides-wknd)已部署至您的Cloud Service環境

## 外部應用程式概述

此教學課程使用從命令列執行的[簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip)，以使用[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)更新AEM as a Cloud Service上的資產中繼資料。

Node.js應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令列叫用Node.js應用程式
1. 命令列引數定義：
   + 要連線的AEM as a Cloud Service Author服務主機(`aem`)
   + 資產已更新的AEM資產資料夾(`folder`)
   + 要更新的中繼資料屬性和值（`propertyName`和`propertyValue`）
   + 檔案的本機路徑，提供存取AEM as a Cloud Service所需的認證(`file`)
1. 用來驗證AEM的存取權杖衍生自透過命令列引數`file`提供的JSON檔案

   a.如果在JSON檔案(`file`)中提供用於非本機開發的服務認證，則會從Adobe IMS API擷取存取權杖
1. 應用程式使用存取權杖來存取AEM，並列出在命令列引數`folder`中指定的資料夾中的所有資產
1. 針對資料夾中的每個資產，應用程式會根據命令列引數`propertyName`和`propertyValue`中指定的屬性名稱和值更新其中繼資料

雖然此範例應用程式為Node.js，但這些互動可以使用不同的程式設計語言開發，並從其他外部系統執行。

## 本機開發存取權杖

本機開發存取權杖是為特定的AEM as a Cloud Service環境產生，可讓您存取製作和發佈服務。  這些存取權杖為暫時性，僅供在開發透過HTTP與AEM互動的外部應用程式或系統時使用。 開發人員不必取得及管理好記的服務認證，而是可以快速輕鬆地自行產生暫時存取權杖，好讓他們開發整合。

+ [如何使用本機開發存取權杖](./local-development-access-token.md)

## 服務認證

服務憑證是任何非開發情況（最明顯的是生產情況）中使用的真實憑證，可促進外部應用程式或系統透過HTTP向AEM as a Cloud Service驗證及與互動。 服務憑證本身不會傳送給AEM進行驗證，而是由外部應用程式使用這些憑證來產生JWT，再與Adobe IMS的API _交換_&#x200B;存取權杖，然後使用後者來向AEM as a Cloud Service驗證HTTP請求。

+ [如何使用服務認證](./service-credentials.md)

## 其他資源

+ [下載應用程式範例](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT建立與交換的其他程式碼範例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [JavaScript/Axios型程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
