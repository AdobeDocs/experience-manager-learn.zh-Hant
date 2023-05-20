---
title: Next.js — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Next.js應用程式演示了如何使用永續查詢AEM使用GraphQLAPI查詢內容。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 1%

---

# Next.js應用

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Next.js應用程式演示了如何使用永續查詢AEM使用GraphQLAPI查詢內容。 用於AEMJavaScript的無頭客戶端用於執行為應用程式提供動力的GraphQL永續查詢。

![Next.js應用，帶無AEM頭](./assets/next-js/next-js.png)

查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [節點.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM要求

Next.js應用可與以下部署選AEM項配合使用。 所有部署都需要 [WKND共用v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站點v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝到AEMas a Cloud Service。

此示例Next.js應用設計為連接到 __AEM發佈__ 服務。

### AEM作者要求

Next.js設計為連接到 __AEM發佈__ 訪問未受保護的內容。 Next.js可配置為通過 `.env` 屬性。 從AEM Author提供的映像需要身份驗證，因此訪問Next.js應用的用戶還必須登錄到AEM Author。

## 如何使用

1. 克隆 `adobe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案和設定 `NEXT_PUBLIC_AEM_HOST` 服AEM務。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   如果連接到AEM作者服務，則必須提供身份驗證，因為預設情況下AEM作者服務是安全的。

   使用本地帳AEM戶集 `AEM_AUTH_METHOD=basic` 並在 `AEM_AUTH_USER` 和 `AEM_AUTH_PASSWORD` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   使用 [AEMas a Cloud Service本地開發令牌](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) 集 `AEM_AUTH_METHOD=dev-token` 並在中提供完整的dev令牌值 `AEM_AUTH_DEV_TOKEN` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案和驗證  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 設定為相應的AEMGraphQL終結點。

   使用時 [WKND共用](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站點](https://github.com/adobe/aem-guides-wknd/releases/latest)，使用 `wknd-shared` GraphQLAPI終結點。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. 開啟命令提示符，然後使用以下命令啟動Next.js應用：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新瀏覽器窗口將在以下位置開啟Next.js應用 [http://localhost:3000](http://localhost:3000)
1. Next.js應用顯示冒險清單。 選擇冒險將在新頁面中開啟其詳細資訊。

## 代碼

下面是Next.js應用的構建方式、它如何連接到AEMHeadless以使用GraphQL永續查詢檢索內容以及如何顯示該資料的摘要。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)。

### 永續查詢

遵循AEM無頭最佳實踐，Next.js應用使AEM用GraphQL持久查詢來查詢冒險資料。 應用使用兩個永續查詢：

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

每個永續查詢在 `src/lib//aem-headless-client.js`這叫做AEMGraphQL終點，並返回冒險資料。

每個函式依次調用 `aemHeadlessClient.runPersistedQuery(...)`，執行永續的GraphQL查詢。

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

Next.js應用使用兩頁來顯示冒險資料。

+ `src/pages/index.js`

   使用 [Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 呼叫 `getAllAdventures()` 把每次冒險都顯示成一張卡片。

   使用 `getServerSiteProps()` 允許此Next.js頁的伺服器端呈現。

+ `src/pages/adventures/[...slug].js`

   A [Next.js動態路由](https://nextjs.org/docs/routing/dynamic-routes) 顯示一次冒險的細節。 此動態路由使用 [Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 通過呼叫 `getAdventureBySlug(..)` 使用 `slug` 參數通過 `adventures/index.js` 的子菜單。

   動態路由能夠通過使用 [Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) 並根據GraphQL查詢返回的歷險清單填充所有可能的路由排列  `getAdventurePaths()`

   使用 `getStaticPaths()` 和 `getStaticProps(..)` 允許生成這些Next.js頁的靜態站點。

## 部署配置

Next.js應用，特別是在伺服器端呈現(SSR)和伺服器端生成(SSG)的上下文中，不需要高級安全配置，如跨源資源共用(CORS)。

但是，如果Next.js確實從客戶端上AEM下文向發出HTTP請求，則可能需要中的安AEM全配置。 查看 [無AEM頭單頁應用程式部署教程](../deployment/spa.md) 的子菜單。
