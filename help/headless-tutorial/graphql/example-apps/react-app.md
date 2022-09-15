---
title: React應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此React應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: b20a29e67da0bcbf53ae8089a7cde0dfde800214
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 1%

---

# React應用程式{#react-app}

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此React應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。 適用於JavaScript的AEM無周邊用戶端可用來執行為應用程式提供動力的GraphQL持續查詢。

![使用AEM Headless反應應用程式](./assets/react-app/react-app.png)

檢視 [GitHub原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [逐步完整教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html) 說明此React應用程式的建置可用性。

>[!CONTEXTUALHELP]
>id="aemcloud_sites_trial_admin_content_fragments_react_app"
>title="自訂範例React應用程式中的內容"
>abstract="我們已設定現代化的React應用程式，您可借此了解如何使用無頭功能集來自訂內容。"

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM需求

React應用程式可搭配下列AEM部署選項使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

React應用程式設計為連線至 __AEM發佈__ 環境，但若React應用程式設定中提供驗證，則可從AEM Author取得內容。

## 如何使用

1. 複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/react-app/.env.development` 檔案和設定 `REACT_APP_HOST_URI` 指向您的target AEM。

   如果連線至製作例項，請更新驗證方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
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

1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新瀏覽器視窗應載入 [http://localhost:3000](http://localhost:3000)
1. 應用程式中應顯示來自WKND參考網站的歷險清單。

## 程式碼

以下是如何建置React應用程式、如何連接至AEM無周邊以使用GraphQL持續查詢擷取內容，以及如何呈現該資料的摘要。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 持續查詢

遵循AEM無周邊最佳實務，React應用程式使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續的查詢：

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

+ `wknd/adventure-by-slug` 持續查詢，其返回單個歷程，方法為 `slug` （可唯一識別冒險的自訂屬性），包含完整的屬性集。 這個持續的查詢可支援探險詳細資訊的檢視。

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

### 執行GraphQL持續查詢

AEM持續查詢會透過HTTPGET執行，因此， [AEM JavaScript適用的無頭式用戶端](https://github.com/adobe/aem-headless-client-js) 用於 [執行持續的GraphQL查詢](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 來抵御AEM，並將冒險內容載入應用程式中。

每個保存的查詢在 `src/api/persistedQueries.js`，會非同步呼叫AEM HTTPGET端點，並傳回冒險資料。

每個函式依次調用 `aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

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

React應用程式使用兩種檢視來呈現網頁體驗中的冒險資料。

+ `src/components/Adventures.js`

   調用 `getAllAdventures()` 從 `src/api/persistedQueries.js`  並在清單中顯示傳回的歷險。

+ `src/components/AdventureDetail.js`

   調用 `getAdventureBySlug(..)` 使用 `slug` 參數是透過 `Adventures` 元件，並顯示單一歷險的詳細資訊。


### 環境變數

數個 [環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables) 用於連線至AEM環境。 預設連線至執行中的AEM發佈 `http://localhost:4503`. 若要變更AEM連線，請更新 `.env.development` 檔案：

+ `REACT_APP_HOST_URI=http://localhost:4502`:設為AEM目標主機
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`:設定GraphQL端點路徑。 此React應用程式不會使用此值，因為此應用程式僅使用持續的查詢。
+ `REACT_APP_AUTH_METHOD=`:慣用的驗證方法。 選用，依預設不使用驗證。
   + `service-token`:使用服務憑證在AEM as a Cloud Service上取得存取權杖
   + `dev-token`:在AEMas a Cloud Service上使用開發代號進行本機開發
   + `basic`:透過本機AEM作者，將使用者/傳遞用於本機開發
   + 保留為空白，即可不進行驗證而連線至AEM
+ `REACT_APP_AUTHORIZATION=admin:admin`:設定連線至AEM製作環境時使用的基本驗證憑證（僅限開發）。 如果連線至發佈環境，則不需要此設定。
+ `REACT_APP_DEV_TOKEN`:開發代號字串。 若要連線至遠端執行個體，除了基本驗證(user:pass)之外，您還可以透過雲端主控台，將承載驗證與DEV代號搭配使用
+ `REACT_APP_SERVICE_TOKEN`:服務憑據檔案的路徑。 若要連線至遠端執行個體，您也可以使用服務代號（從開發人員控制台下載檔案）來完成驗證。

### 代理AEM請求

使用Webpack開發伺服器時(`npm start`)專案需仰賴 [代理設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`. 檔案的設定位置為 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 並需仰賴設定於的數個自訂環境變數 `.env` 和 `.env.development`.

若連線至AEM製作環境，則對應的 [驗證方法需要配置](#environment-variables).

### 跨原始資源共用(CORS)

此React應用程式需仰賴在目標AEM環境上執行的AEM型CORS設定，並假設React應用程式執行於 `http://localhost:3000` 在開發模式中。 此 [CORS設定](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND站點](https://github.com/adobe/aem-guides-wknd).

![CORS設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
