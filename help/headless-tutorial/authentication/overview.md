---
title: 從外部應用程式驗證AEMas a Cloud Service
description: 探索外部應用程式如何使用本機開發存取權杖和服務憑證，以程式設計方式驗證AEMas a Cloud Service，並透過HTTP與其互動。
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 0%

---

# 以代號為基礎的驗證以AEMas a Cloud Service

AEM會公開各種HTTP端點，可以無周邊方式互動，從GraphQL、AEM Content Services到Assets HTTP API。 這些無頭消費者通常需要向AEM驗證，才能存取受保護的內容或動作。 為方便執行此作業，AEM支援來自外部應用程式、服務或系統之HTTP要求的權杖式驗證。

在本教學課程中，請妥善探索外部應用程式如何以程式設計方式，使用存取權杖，透過HTTP驗證AEMas a Cloud Service並與其互動。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先決條件

遵循本教學課程之前，請確定已具備下列條件：

1. 存取am AEMas a Cloud Service環境（最好是開發環境或沙箱方案）
1. AEMas a Cloud Service環境之作者服務AEM管理員產品設定檔的成員資格
1. 加入或存取您的Adobe IMS組織管理員(他們必須執行一次性初始化 [服務憑據](./service-credentials.md))
1. 最新 [WKND站點](https://github.com/adobe/aem-guides-wknd) 部署至您的Cloud Service環境

## 外部應用程式概述

本教學課程使用 [簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip) 從命令列執行，在AEM as a Cloud Service上使用更新資產中繼資料 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Node.js應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令行調用Node.js應用程式
1. 命令行參數定義：
   + 要連線至的AEMas a Cloud Service製作服務主機(`aem`)
   + 資產已更新的AEM資產資料夾(`folder`)
   + 要更新的元資料屬性和值(`propertyName` 和 `propertyValue`)
   + 檔案的本機路徑，提供存取AEMas a Cloud Service所需的憑證(`file`)
1. 用來驗證AEM的存取權杖，衍生自透過命令列參數提供的JSON檔案 `file`

   a.若JSON檔案中提供非本機開發使用的服務憑證(`file`)，則會從Adobe IMS API擷取存取權杖
1. 應用程式會使用存取權杖來存取AEM，並列出命令列參數中指定之資料夾中的所有資產 `folder`
1. 對於資料夾中的每個資產，應用程式會根據命令行參數中指定的屬性名稱和值更新其元資料 `propertyName` 和 `propertyValue`

雖然此示例應用程式是Node.js，但這些交互可以使用不同的寫程式語言開發，並從其他外部系統執行。

## 本機開發存取權杖

本機開發存取權杖是針對特定AEMas a Cloud Service環境產生，並提供作者和發佈服務的存取權。  這些存取權杖為暫時性的，僅用於開發透過HTTP與AEM互動的外部應用程式或系統時。 與其讓開發人員必須取得及管理妥善的服務憑證，他們可以快速且輕鬆地自行產生暫時存取權杖，以便開發其整合。

+ [如何使用本機開發存取權杖](./local-development-access-token.md)

## 服務憑據

服務憑證是用於任何非開發案例（最明顯的是生產案例）的合用憑證，可協助外部應用程式或系統透過HTTP驗證AEMas a Cloud Service並與之互動的能力。 服務憑證本身不會傳送至AEM進行驗證，而是外部應用程式會使用這些憑證來產生JWT，並與Adobe IMS的API交換 _for_ 存取權杖，接著可用來驗證AEM as a Cloud Service的HTTP要求。

+ [如何使用服務憑證](./service-credentials.md)

## 其他資源

+ [下載範例應用程式](./assets/aem-guides_token-authentication-external-application.zip)
+ 建立和交換JWT的其他代碼示例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axios型程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
