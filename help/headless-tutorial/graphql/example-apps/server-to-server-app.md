---
title: 伺服器到伺服器Node.js應用 — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此伺服器端Node.js應用程式演示了如何使用永續查詢AEM使用GraphQLAPI查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 5%

---

# 伺服器到伺服器Node.js應用

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此伺服器到伺服器應用程式演示了如何使用永續查詢AEM使用GraphQLAPI查詢內容，並在終端上打印內容。

![帶無頭功能的伺服器到伺服器Node.js應AEM用](./assets/server-to-server-app/server-to-server-app.png)

查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [節點.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

Node.js應用程式可以使用以下部AEM署選項。 所有部署都需要 [WKND站點v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ （可選） [服務憑據](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) 如果授權請求（例如，連接到AEM作者服務）。

此Node.js應用程式可以基於命令行參數連接到AEM Author或AEM Publish。

## 如何使用

1. 克隆 `adobe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟終端並運行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 可以使用以下命令運行應用：

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例如，要針對AEM Publish運行應用程式，請執行以下操作：

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   要使用授權對AEM Author運行應用程式：

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. 來自WKND參考站點的JSON冒險清單應打印在終端中。

## 代碼

下面是如何構建伺服器到伺服器Node.js應用程式、如何連接到AEMHeadless以使用GraphQL永續查詢檢索內容以及如何呈現該資料的摘要。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)。

伺服器到伺服器無頭應用的常見使用情形AEM是將內容碎片資料從其AEM他系統同步到其他系統，但此應用程式有意簡單，並打印永續查詢的JSON結果。

### 永續查詢

遵循AEM無頭最佳實踐，應用程AEM序使用GraphQL持久查詢來查詢冒險資料。 應用程式使用兩個永續查詢：

+ `wknd/adventures-all` 永續查詢，返回帶有AEM一組刪除屬性的所有冒險。 此永續查詢驅動初始視圖的冒險清單。

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

### 建立無AEM頭客戶端

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


### 執行GraphQL永續查詢

通AEM過HTTPGET執行永續查詢， [NodeAEM.js的無頭客戶端](https://github.com/adobe/aem-headless-client-nodejs) 是 [執行永續GraphQL查詢](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) 並檢AEM索冒險內容。

通過調用調用永續查詢 `aemHeadlessClient.runPersistedQuery(...)`，並傳遞永續的GraphQL查詢名稱。 一旦GraphQL返回資料，將其傳遞給簡化 `doSomethingWithDataFromAEM(..)` 函式，該函式打印結果，但通常會將資料發送到另一個系統，或根據檢索到的資料生成一些輸出。

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
