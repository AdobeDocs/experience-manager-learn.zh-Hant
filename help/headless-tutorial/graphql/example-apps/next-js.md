---
title: Next.js - AEM無頭範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此Next.js應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 1%

---

# Next.js應用程式

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此Next.js應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。 適用於JavaScript的AEM無周邊用戶端可用來執行為應用程式提供動力的GraphQL持續查詢。

![具有AEM Headless的Next.js應用程式](./assets/next-js/next-js.png)

檢視 [GitHub原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM需求

Next.js應用程式可搭配下列AEM部署選項使用。 所有部署都需要 [WKND共用v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [WKND Site v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)，或 [參考示範附加元件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) 安裝在AEMas a Cloud Service環境中。

此範例Next.js應用程式的設計目的為連線至 __AEM發佈__ 服務。

### AEM作者需求

Next.js的設計是連線至 __AEM發佈__ 服務，以及存取未受保護的內容。 Next.js可設定為透過 `.env` 屬性如下所述。 從AEM Author提供的影像需要驗證，因此存取Next.js應用程式的使用者也必須登入AEM Author。

## 如何使用

1. 複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案和設定 `NEXT_PUBLIC_AEM_HOST` 到AEM服務。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   如果連線至AEM製作服務，驗證必須提供，因為AEM製作服務預設是安全的。

   使用本機AEM帳戶集 `AEM_AUTH_METHOD=basic` 並在 `AEM_AUTH_USER` 和 `AEM_AUTH_PASSWORD` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   若要使用 [AEMas a Cloud Service本機開發代號](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` 並在 `AEM_AUTH_DEV_TOKEN` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案及驗證  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 設為適當的AEM GraphQL端點。

   使用時 [WKND共用](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站點](https://github.com/adobe/aem-guides-wknd/releases/latest)，請使用 `wknd-shared` GraphQL API端點。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   使用時 [參考示範附加元件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html)，請使用 `aem-demo-assets` GraphQL API端點。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. 開啟命令提示字元，然後使用下列命令啟動Next.js應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新的瀏覽器視窗會在開啟Next.js應用程式 [http://localhost:3000](http://localhost:3000)
1. Next.js應用程式會顯示歷險清單。 選取冒險項目會在新頁面中開啟其詳細資訊。

## 程式碼

以下是如何建置Next.js應用程式、如何連線至AEM無周邊以使用GraphQL持續查詢擷取內容，以及如何呈現該資料的摘要。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 持續查詢

遵循AEM無周邊最佳實務，Next.js應用程式使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續的查詢：

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

每個保存的查詢在 `src/lib//aem-headless-client.js`，會呼叫AEM GraphQL端點並傳回冒險資料。

每個函式依次調用 `aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### 頁面

Next.js應用程式使用兩個頁面來呈現冒險資料。

+ `src/pages/index.js`

   使用 [Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 呼叫 `getAllAdventures()` 把每個冒險都顯示成卡片。

   使用 `getServerSiteProps()` 允許伺服器端轉譯此Next.js頁面。

+ `src/pages/adventures/[...slug].js`

   A [Next.js動態路由](https://nextjs.org/docs/routing/dynamic-routes) 顯示單一冒險的詳細資訊。 此動態路由會使用 [Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 透過呼叫 `getAdventureBySlug(..)` 使用 `slug` 參數是透過 `adventures/index.js` 頁面。

   動態路由可透過以下項目預先擷取所有歷險的詳細資訊： [Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) 並根據GraphQL查詢返回的歷險清單填入所有可能的路由排列  `getAdventurePaths()`

   使用 `getStaticPaths()` 和 `getStaticProps(..)` 允許靜態網站產生這些Next.js頁面。

## 部署配置

Next.js應用程式(尤其是在伺服器端轉譯(SSR)和伺服器端產生(SSG)的內容中)不需要進階安全設定，例如跨原始資源共用(CORS)。

不過，如果Next.js確實從用戶端內容向AEM提出HTTP要求，則可能需要AEM中的安全設定。 檢閱 [AEM Headless單頁應用程式部署教學課程](../deployment/spa.md) 以取得更多詳細資訊。

