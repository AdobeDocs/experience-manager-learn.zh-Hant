---
title: 從外部應用程式驗證AEM為雲端服務
description: 探索外部應用程式如何使用本機開發存取Token和服務認證，以程式設計方式透過HTTP驗證AEM作為雲端服務並與之互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: 733382dc0e0ca14d4bd6e49174ba33f8d7fc517d
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# AEM雲端服務的Token型驗證

在本教學課程中，請進一步瞭解外部應用程式如何使用存取Token，以程式設計方式將AEM驗證為透過HTTP的雲端服務並與之互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

## 先決條件

請先確定下列項目已就緒，然後再隨附本教學課程：

1. 以雲端服務環境存取AEM（最好是開發環境或沙盒程式）
1. AEM的Cloud Service環境「作者服務」會籍AEM Administrator產品設定檔
1. 您Adobe IMS組織管理員的會籍或存取權（他們必須執行一次性初始化[服務認證](./service-credentials.md)）
1. 部署到雲服務環境的最新[WKND站點](https://github.com/adobe/aem-guides-wknd)

## 外部應用程式概觀

本教學課程使用從命令列執行的[簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip)，使用[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)將AEM上的資產中繼資料更新為雲端服務。

Node.js應用程式的執行流程如下：

![外部應用程式](./assets/overview/external-application.png)

1. 從命令行調用Node.js應用程式
1. 命令行參數定義：
   + AEM做為雲端服務作者服務主機，可連接至(`aem`)
   + 資產將會更新的AEM資產資料夾(`folder`)
   + 要更新的中繼資料屬性和值（`propertyName`和`propertyValue`）
   + 提供以雲端服務形式存取AEM所需憑證之檔案的本機路徑(`file`)
1. 用於驗證AEM的存取Token衍生自透過命令列參數`file`提供的JSON檔案

   a.如果JSON檔案(`file`)中提供用於非本機開發的服務認證，則會從Adobe IMS API擷取存取Token
1. 應用程式會使用存取Token來存取AEM，並列出命令列參數`folder`中所指定之資料夾中的所有資產
1. 對於資料夾中的每個資產，應用程式會根據命令行參數`propertyName`和`propertyValue`中指定的屬性名稱和值更新其元資料

雖然此範例應用程式是Node.js，但這些互動可使用不同的程式設計語言開發，並從其他外部系統執行。

## 本機開發存取Token

本機開發存取Token會以雲端服務環境的形式產生給特定AEM，並提供作者和發佈服務的存取權。  這些存取Token是暫時性的，僅用於開發透過HTTP與AEM互動的外部應用程式或系統。 開發人員不需要取得和管理固定服務認證，而是可快速輕鬆地自行產生暫時存取Token，讓他們開發整合。

+ [如何使用本機開發存取Token](./local-development-access-token.md)

## 服務憑據

「服務認證」是用於任何非開發案例（最明顯是生產案例）的合併認證，可協助外部應用程式或系統以HTTP雲端服務身分驗證AEM並與之互動。 服務憑證本身不會傳送至AEM進行驗證，而是外部應用程式會使用這些憑證來產生JWT,JWT會與Adobe IMS的API _交換，以取代_&#x200B;存取Token，然後存取Token可用來將HTTP要求驗證為雲端服務的AEM。

+ [如何使用服務憑據](./service-credentials.md)

## 其他資源

+ [下載範例應用程式](./assets/aem-guides_token-authentication-external-application.zip)
+ 建立和交換JWT的其他代碼示例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [以JavaScript/Axios為基礎的程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
