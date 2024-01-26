---
title: 伺服器對伺服器Node.js應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此伺服器端Node.js應用程式示範了如何使用AEM GraphQL APIs透過持續性查詢來查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 145
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# 伺服器對伺服器Node.js應用程式

範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此伺服器對伺服器應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容，並在終端機上列印。

![使用AEM Headless的伺服器對伺服器Node.js應用程式](./assets/server-to-server-app/server-to-server-app.png)

檢視 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## AEM需求

Node.js應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即將安裝。

+ [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 或者 [服務認證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) 如果授權請求(例如，連線到AEM作者服務)。

此Node.js應用程式可以根據命令列引數連線至AEM Author或AEM Publish。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 應用程式可使用命令執行：

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例如，若要在未獲得授權的情況下對AEM Publish執行應用程式：

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   若要以授權方式對AEM Author執行應用程式：

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. 應於終端機中列印來自WKND參考網站的JSON冒險清單。

## 程式碼

以下摘要說明如何建立伺服器對伺服器Node.js應用程式、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現。 完整的程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

伺服器對伺服器AEM Headless應用程式的常見使用案例是將內容片段資料從AEM同步到其他系統，不過此應用程式刻意要簡單一點，而且會列印來自持續查詢的JSON結果。

### 持久查詢

依照AEM Headless最佳實務，應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all` 持久查詢，它會傳回AEM中所有冒險和一組刪節的屬性。 此持續查詢會驅動初始檢視的冒險清單。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### 建立AEM Headless客戶端

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### 執行GraphQL持久查詢

AEM持續查詢會透過HTTPGET執行，因此 [Node.js的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-nodejs) 用於 [執行持續的GraphQL查詢](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) AEM並擷取冒險內容。

透過呼叫來叫用持久查詢 `aemHeadlessClient.runPersistedQuery(...)`，並傳遞儲存的GraphQL查詢名稱。 GraphQL傳回資料後，請將其傳遞至簡化版 `doSomethingWithDataFromAEM(..)` 函式列印結果，但通常會傳送資料至其他系統，或根據擷取的資料產生一些輸出。

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
