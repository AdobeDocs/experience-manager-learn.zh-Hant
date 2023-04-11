---
title: 伺服器對伺服器Node.js應用程式 — AEM無頭範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此伺服器端的Node.js應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 5%

---

# 伺服器對伺服器Node.js應用程式

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此伺服器對伺服器應用程式示範如何使用持續查詢使用AEM GraphQL API來查詢內容，並在終端機上列印。

![具有AEM Headless的伺服器對伺服器Node.js應用程式](./assets/server-to-server-app/server-to-server-app.png)

檢視 [GitHub原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

Node.js應用程式可搭配下列AEM部署選項使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ （可選） [服務憑證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) 如果授權請求（例如連線至AEM作者服務）。

此Node.js應用程式可以根據命令列參數連線至AEM Author或AEM Publish。

## 如何使用

1. 複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 可使用命令執行應用程式：

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例如，若要在未取得授權的情況下對AEM Publish執行應用程式：

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   若要使用授權對AEM作者執行應用程式：

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. 來自WKND參考網站的冒險JSON清單應列印在終端機中。

## 程式碼

以下是如何建立伺服器對伺服器Node.js應用程式、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及如何呈現該資料的摘要。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

伺服器對伺服器AEM無頭應用程式的常見使用案例是將內容片段資料從AEM同步至其他系統，不過此應用程式刻意簡單，並會列印持續查詢的JSON結果。

### 持續查詢

遵循AEM無頭式最佳實務，應用程式使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續的查詢：

+ `wknd/adventures-all` 持續查詢，會傳回AEM中具有一組縮略屬性的所有歷險。 這個持續的查詢會驅動初始檢視的探險清單。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

### 建立AEM Headless用戶端

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


### 執行GraphQL持續查詢

AEM持續查詢會透過HTTPGET執行，因此， [AEM Node.js的無頭式用戶端](https://github.com/adobe/aem-headless-client-nodejs) 用於 [執行持續的GraphQL查詢](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) 比對AEM，並擷取冒險內容。

通過調用 `aemHeadlessClient.runPersistedQuery(...)`，並傳遞持續存在的GraphQL查詢名稱。 GraphQL傳回資料後，請將其傳遞至簡化 `doSomethingWithDataFromAEM(..)` 函式，可列印結果 — 但通常會將資料傳送至其他系統，或根據擷取的資料產生一些輸出。

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
