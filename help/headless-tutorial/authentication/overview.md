---
title: 從外部應用程式驗證AEM作為Cloud Service
description: 探索外部應用程式如何使用本機開發存取權杖和服務憑證，以程式設計方式驗證AEM，並透過HTTP與其互動，作為Cloud Service。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: 無頭式整合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# 以代號為基礎的驗證以AEM作為Cloud Service

在本教學課程中，請進一步了解外部應用程式如何使用存取權杖，以程式設計方式驗證AEM，並與作為HTTPCloud Service互動。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先決條件

遵循本教學課程之前，請確定已具備下列條件：

1. 以Cloud Service環境（最好是開發環境或沙箱方案）存取am AEM
1. AEM as aCloud Service環境的作者服務AEM管理員產品設定檔的成員資格
1. 加入或存取您的AdobeIMS組織管理員（他們必須執行一次性初始化[服務憑證](./service-credentials.md)）
1. 部署到您的Cloud Service環境的最新[WKND站點](https://github.com/adobe/aem-guides-wknd)

## 外部應用程式概述

本教學課程使用從命令列執行的[簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip)，以使用[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)更新AEM上的資產中繼資料作為Cloud Service。

Node.js應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令行調用Node.js應用程式
1. 命令行參數定義：
   + 要連線至(`aem`)的AEM作為Cloud Service製作服務主機
   + 將更新其資產的AEM資產資料夾(`folder`)
   + 要更新的元資料屬性和值（`propertyName`和`propertyValue`）
   + 檔案的本機路徑，提供以Cloud Service形式存取AEM所需的憑證(`file`)
1. 用於驗證AEM的存取權杖衍生自透過命令列參數`file`提供的JSON檔案

   a.若JSON檔案(`file`)中提供用於非本機開發的服務憑證，則會從AdobeIMS API擷取存取權杖
1. 應用程式使用存取權杖來存取AEM，並列出命令列參數`folder`中指定之資料夾中的所有資產
1. 對於資料夾中的每個資產，應用程式會根據命令行參數`propertyName`和`propertyValue`中指定的屬性名稱和值更新其元資料

雖然此示例應用程式是Node.js，但這些交互可以使用不同的寫程式語言開發，並從其他外部系統執行。

## 本機開發存取權杖

本機開發存取權杖會針對特定AEM產生，作為Cloud Service環境，並提供製作和發佈服務的存取權。  這些存取權杖為暫時性的，僅用於開發透過HTTP與AEM互動的外部應用程式或系統。 與其讓開發人員必須取得及管理妥善的服務憑證，他們可以快速且輕鬆地自行產生暫時存取權杖，以便開發其整合。

+ [如何使用本機開發存取權杖](./local-development-access-token.md)

## 服務憑據

服務認證是用於任何非開發案例（最明顯的是生產案例）的合用認證，可促進外部應用程式或系統驗證AEM並透過HTTP與其互動的Cloud Service。 服務憑證本身不會傳送至AEM進行驗證，而是外部應用程式會使用這些憑證來產生JWT，並與AdobeIMS的API _交換_&#x200B;的存取權杖，之後存取權杖便可用來驗證AEM作為Cloud Service的HTTP要求。

+ [如何使用服務憑證](./service-credentials.md)

## 其他資源

+ [下載範例應用程式](./assets/aem-guides_token-authentication-external-application.zip)
+ 建立和交換JWT的其他代碼示例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axios型程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
