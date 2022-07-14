---
title: 反應應用 — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此React應用程式演示了如何使用GraphQL APIAEM使用永續查詢來查詢內容。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 4b8f7d9c6def8c121f3c975218b7214d271fa77f
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 1%

---

# 反應應用

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此React應用程式演示了如何使用GraphQL APIAEM使用永續查詢來查詢內容。 JavaScriptAEM的無頭客戶端用於執行為應用程式提供動力的GraphQL永續查詢。

![使用無頭應用程式AEM反應](./assets/react-app/react-app.png)

查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [全步逐步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html) 描述此React應用如何生成可用。

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [蠢貨](https://git-scm.com/)

## AEM要求

React應用程式可以與以下部署AEM選項配合使用。 所有部署都需要 [WKND站點v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本地設定使用 [AEM Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM6.5 SP13+快速啟動](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

React應用程式旨在連接到 __AEM發佈__ 但是，如果在React應用程式的配置中提供了身份驗證，則它可以從AEM Author中源出內容。

## 如何使用

1. 克隆 `adobbe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/react-app/.env.development` 檔案和設定 `REACT_APP_HOST_URI` 瞄準目標AEM。

   如果連接到作者實例，請更新驗證方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/_cq_graphql/wknd-shared/endpoint.json
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. 開啟終端並運行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新瀏覽器窗口應載入 [http://localhost:3000](http://localhost:3000)
1. 應用程式上應顯示來自WKND參考站點的冒險清單。

## 代碼

以下是React應用程式構建方式的摘要，它如何連接到AEMHeadless以使用GraphQL持久查詢檢索內容，以及如何顯示該資料。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)。


### 永續查詢

遵循AEM無頭最佳實踐， React應用程式使用AEMGraphQL持久查詢來查詢冒險資料。 應用程式使用兩個永續查詢：

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

+ `wknd/adventure-by-slug` 永續查詢，返回單個冒險 `slug` （具有完整屬性集的唯一標識冒險的自定義屬性）。 此持久查詢可支援冒險詳細資訊視圖。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
  	}) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### 執行GraphQL永續查詢

通AEM過HTTPGET執行永續查詢， [用AEM於JavaScript的無頭客戶端](https://github.com/adobe/aem-headless-client-js) 是 [執行永續GraphQL查詢](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 並AEM將冒險內容載入到應用中。

每個永續查詢在 `src/api/persistedQueries.js`，它非同步調AEM用HTTPGET端點並返回冒險資料。

每個函式依次調用 `aemHeadlessClient.runPersistedQuery(...)`，執行永續的GraphQL查詢。

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### 檢視

React應用程式使用兩個視圖來呈現Web體驗中的冒險資料。

+ `src/components/Adventures.js`

   調用 `getAllAdventures()` 從 `src/api/persistedQueries.js`  並在清單中顯示返回的冒險。

+ `src/components/AdventureDetail.js`

   調用 `getAdventureBySlug(..)` 使用 `slug` 參數通過 `Adventures` 元件，並顯示單個冒險的詳細資訊。


### 環境變數

幾個 [環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables) 用於連接到環AEM境。 預設連接到運行於的AEM發佈 `http://localhost:4503`。 更改連AEM接更新 `.env.development` 檔案：

+ `REACT_APP_HOST_URI=http://localhost:4502`:設定為AEM目標主機
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`:設定GraphQL終結點路徑。 此React應用未使用此功能，因為此應用僅使用永續查詢。
+ `REACT_APP_AUTH_METHOD=`:首選的身份驗證方法。 可選，預設情況下不使用身份驗證。
   + `service-token`:使用服務憑據在AEMas a Cloud Service上獲取訪問令牌
   + `dev-token`:使用dev令牌在AEMas a Cloud Service上進行本地開發
   + `basic`:與本地AEM作者一起使用用戶/通行證進行本地開發
   + 留空以在不進行身份驗證AEM的情況下連接
+ `REACT_APP_AUTHORIZATION=admin:admin`:將基本身份驗證憑據設定為在連接到AEM作者環境時使用（僅用於開發）。 如果連接到「發佈」環境，則不需要此設定。
+ `REACT_APP_DEV_TOKEN`:開發令牌字串。 要連接到遠程實例，除基本身份驗證（用戶：pass）外，您可以在雲控制台中將持有者身份驗證與DEV令牌一起使用
+ `REACT_APP_SERVICE_TOKEN`:服務憑據檔案的路徑。 要連接到遠程實例，還可以使用服務令牌（從開發人員控制台下載檔案）進行身份驗證。

### 代理請AEM求

使用WebPack開發伺服器時(`npm start`)項目依賴於 [代理設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`。 檔案配置在 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 並依賴於 `.env` 和 `.env.development`。

如果連接到作AEM者環境，則 [驗證方法需要配置](#environment-variables)。

### 跨源資源共用(CORS)

此React應用程式依賴於在AEM目標環境上運行的基於CORS的配AEM置，並假定React應用程式在 `http://localhost:3000` 的下界。 的 [CORS配置](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND站點](https://github.com/adobe/aem-guides-wknd)。

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
