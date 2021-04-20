---
title: 從外部應AEM用程式驗證為Cloud Service
description: 探索外部應用程式如何使用本機開發存取Token和服務AEM認證，以程式設計方式驗證並與HTTPCloud Service互動。
version: cloud-service
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---


# 以代號為基礎的驗AEM證，做為Cloud Service

在本教學課程中，請進一步瞭解外部應用程式如何使用存取Token，以程式設計方式驗證並與之AEM互動，成為透過HTTP進行Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 先決條件

請先確定下列項目已就緒，然後再隨附本教學課程：

1. 以Cloud Service環AEM境（最好是開發環境或沙盒程式）存取
1. 以Cloud Service環AEM境的作者服務管理員產AEM品配置檔案的身份
1. 您的AdobeIMS組織管理員的會籍或存取權（他們必須執行一次性初始化[服務認證](./service-credentials.md)）
1. 部署到Cloud Service環境的最新[WKND站點](https://github.com/adobe/aem-guides-wknd)

## 外部應用程式概觀

本教學課程使用從命令列執行的[簡單的Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip)，以使用[Assets HTTP APIAEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)將資產中繼資料更新為Cloud Service。

Node.js應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令行調用Node.js應用程式
1. 命令行參數定義：
   + 作為AEM連接到(`aem`)的Cloud Service作者服務主機
   + 資產資AEM料夾，其資產將會更新(`folder`)
   + 要更新的中繼資料屬性和值（`propertyName`和`propertyValue`）
   + 檔案的本地路徑，提供作為Cloud Service訪AEM問所需憑據(`file`)
1. 用於驗證的存取TokenAEM衍生自透過命令列參數`file`提供的JSON檔案

   a.如果JSON檔案(`file`)中提供用於非本機開發的服務認證，則會從AdobeIMS API擷取存取Token
1. 應用程式會使用存取Token來存取AEM並列出命令列參數`folder`中所指定資料夾中的所有資產
1. 對於資料夾中的每個資產，應用程式會根據命令行參數`propertyName`和`propertyValue`中指定的屬性名稱和值更新其元資料

雖然此範例應用程式是Node.js，但這些互動可使用不同的程式設計語言開發，並從其他外部系統執行。

## 本機開發存取Token

本機開發存取Token會針對特定環境產AEM生，並提供作者和發佈服務的存取權。  這些存取Token是暫時性的，僅用於開發透過HTTP與外部應用程式或系統互AEM動時。 開發人員不需要取得和管理固定服務認證，而是可快速輕鬆地自行產生暫時存取Token，讓他們開發整合。

+ [如何使用本機開發存取Token](./local-development-access-token.md)

## 服務憑據

服務認證是用於任何非開發案例（最明顯是生產案例）中的合併認證，可協助外部應用程式或系統以透過HTTP進行Cloud Service的身分驗證並與之互動AEM。 服務憑證本身不會傳送AEM給驗證，而是外部應用程式使用這些來產生JWT,JWT會與AdobeIMS的API _交換，以取得存取Token，接著可用來驗證HTTP要求AEM以做為Cloud Service。_

+ [如何使用服務憑據](./service-credentials.md)

## 其他資源

+ [下載範例應用程式](./assets/aem-guides_token-authentication-external-application.zip)
+ 建立和交換JWT的其他代碼示例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [以JavaScript/Axios為基礎的程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
