---
title: Next.js - AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此Next.js應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Next.js應用程式

範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此Next.js應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。 適用於JavaScript的AEM Headless Client是用來執行GraphQL持續性查詢，以支援此應用程式。

使用AEM Headless的![Next.js應用程式](./assets/next-js/next-js.png)

在GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)上檢視[原始程式碼

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM需求

Next.js應用程式可與下列AEM部署選項搭配使用。 所有部署都需要在AEM as a Cloud Service環境中安裝[WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)或[WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

此範例Next.js應用程式設計來連線至&#x200B;__AEM Publish__&#x200B;服務。

### AEM作者需求

Next.js的設計可連線至&#x200B;__AEM Publish__&#x200B;服務，並存取未受保護的內容。 Next.js可設定為透過下述的`.env`屬性連線至AEM Author。 由AEM Author提供的影像需要驗證，因此存取Next.js應用程式的使用者也必須登入AEM Author。

## 使用方式

1. 複製`adobe/aem-guides-wknd-graphql`存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯`aem-guides-wknd-graphql/next-js/.env.local`檔案並將`NEXT_PUBLIC_AEM_HOST`設定為AEM服務。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   如果連線到AEM Author服務，則必須提供驗證，因為根據預設，AEM Author服務是安全的。

   若要使用本機AEM帳戶集`AEM_AUTH_METHOD=basic`，並在`AEM_AUTH_USER`和`AEM_AUTH_PASSWORD`屬性中提供使用者名稱和密碼。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   若要使用[AEM as a Cloud Service本機開發權杖](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token)，請設定`AEM_AUTH_METHOD=dev-token`，並在`AEM_AUTH_DEV_TOKEN`屬性中提供完整的開發權杖值。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 編輯`aem-guides-wknd-graphql/next-js/.env.local`檔案並驗證`NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT`已設定為適當的AEM GraphQL端點。

   使用[WKND共用](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)或[WKND網站](https://github.com/adobe/aem-guides-wknd/releases/latest)時，請使用`wknd-shared` GraphQL API端點。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. 開啟命令提示字元，並使用下列命令啟動Next.js應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新的瀏覽器視窗會在[http://localhost:3000](http://localhost:3000)開啟Next.js應用程式
1. Next.js應用程式會顯示冒險清單。 選取冒險會在新頁面中開啟其詳細資訊。

## 程式碼

以下摘要說明如何建立Next.js應用程式、其如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現。 您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)上找到完整程式碼。

### 持久查詢

依照AEM Headless最佳實務，Next.js應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all`持續查詢，此查詢會傳回AEM中所有冒險的摘要。 此持續查詢會驅動初始檢視的冒險清單。

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

+ `wknd/adventure-by-slug`持續查詢，會傳回`slug`的單一冒險（唯一識別冒險的自訂屬性）和完整屬性集。 此持續性查詢可為冒險詳細資料檢視提供支援。

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

### 執行GraphQL持久查詢

AEM的持久查詢會透過HTTP GET執行，因此，JavaScript的[AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js)是用來對AEM [執行持久的GraphQL查詢](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)，並將冒險內容載入應用程式。

每個持續查詢在`src/lib//aem-headless-client.js`中都有對應的函式，它會呼叫AEM GraphQL端點，並傳回冒險資料。

每個函式又會叫用`aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

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

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### 頁面

Next.js應用程式使用兩個頁面來呈現冒險資料。

+ `src/pages/index.js`

  使用[Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)來呼叫`getAllAdventures()`，並將每個冒險活動顯示為卡片。

  使用`getServerSiteProps()`可讓伺服器端轉譯此Next.js頁面。

+ `src/pages/adventures/[...slug].js`

  顯示單一冒險詳細資料的[Next.js動態路由](https://nextjs.org/docs/routing/dynamic-routes)。 此動態路由會使用[Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props)，透過`getAdventureBySlug(slug, queryVariables)`的呼叫，使用透過`adventures/index.js`頁面上的冒險選項傳入的`slug`引數預先擷取每個冒險的資料，並使用`queryVariables`來控制影像格式、寬度和品質。

  動態路由可以使用[Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)，並根據GraphQL查詢`getAdventurePaths()`傳回的完整冒險清單填入所有可能的路由排列，預先擷取所有冒險的詳細資料

  使用`getStaticPaths()`和`getStaticProps(..)`允許靜態網站產生這些Next.js頁面。

## 部署設定

Next.js應用程式，尤其是在伺服器端轉譯(SSR)和伺服器端產生(SSG)的內容中，不需要進階安全性設定，例如跨原始資源共用(CORS)。

不過，如果Next.js確實從使用者端內容向AEM發出HTTP請求，則可能需要AEM中的安全性設定。 如需詳細資訊，請檢閱[AEM Headless單頁應用程式部署教學課程](../deployment/spa.md)。
