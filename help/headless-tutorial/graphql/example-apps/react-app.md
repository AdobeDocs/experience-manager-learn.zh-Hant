---
title: React應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此React應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。
version: Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
duration: 256
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# React應用程式{#react-app}

範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此React應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。 適用於JavaScript的AEM Headless Client是用來執行GraphQL持續性查詢，以支援此應用程式。

使用AEM Headless的![React應用程式](./assets/react-app/react-app.png)

在GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)上檢視[原始程式碼

提供[完整的逐步教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)，說明此React應用程式的建置方式。

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

React應用程式可與下列AEM部署選項搭配使用。 所有部署都需要安裝[WKND網站v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)進行本機設定
   + 需要[JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444)

React應用程式設計來連線至&#x200B;__AEM Publish__&#x200B;環境，不過，如果React應用程式的設定中有提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 複製`adobe/aem-guides-wknd-graphql`存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯`aem-guides-wknd-graphql/react-app/.env.development`檔案並將`REACT_APP_HOST_URI`設定為指向您的目標AEM。

   如果連線到作者執行個體，請更新驗證方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
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

1. 新的瀏覽器視窗應載入[http://localhost:3000](http://localhost:3000)
1. WKND參考網站中的冒險清單應顯示在應用程式上。

## 程式碼

以下是如何建立React應用程式的摘要，它如何連線到AEM Headless以使用GraphQL持續查詢來擷取內容，以及資料如何呈現。 您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)上找到完整程式碼。


### 持久查詢

依照AEM Headless最佳實務，React應用程式會使用AEM GraphQL持續性查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all`持續查詢，這會傳回AEM中所有具有刪節屬性集的冒險。 此持續查詢會驅動初始檢視的冒險清單。

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

AEM的持久查詢會透過HTTPGET執行，因此JavaScript的[AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js)是用來對AEM[執行持久的GraphQL查詢](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)，並將冒險內容載入應用程式。

每個持續查詢在`src/api/usePersistedQueries.js`中具有對應的React [useEffect](https://reactjs.org/docs/hooks-effect.html)勾點，該勾點會非同步呼叫AEM HTTPGET持續查詢端點，並傳回冒險資料。

每個函式又會叫用`aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### 檢視

React應用程式使用兩個檢視，在網頁體驗中呈現冒險資料。

+ `src/components/Adventures.js`

  從`src/api/usePersistedQueries.js`叫用`getAdventuresByActivity(..)`並在清單中顯示傳回的冒險。

+ `src/components/AdventureDetail.js`

  使用透過`Adventures`元件上的冒險選項傳入的`slug`引數叫用`getAdventureBySlug(..)`，並顯示單一冒險的詳細資料。

### 環境變數

有數個[環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables)可用來連線至AEM環境。 預設會連線到`http://localhost:4503`執行中的AEM Publish。 更新`.env.development`檔案以變更AEM連線：

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`：設定為AEM目標主機
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`：設定GraphQL端點路徑。 此React應用程式不會使用此專案，因為此應用程式僅使用持續性查詢。
+ `REACT_APP_AUTH_METHOD=`：偏好的驗證方法。 選擇性，根據預設不使用驗證。
   + `service-token`：使用服務認證取得AEM as a Cloud Service上的存取權杖
   + `dev-token`：在AEM as a Cloud Service上使用開發Token進行本機開發
   + `basic`：透過本機AEM Author將使用者/通行證用於本機開發
   + 留空可連線到AEM而不進行驗證
+ `REACT_APP_AUTHORIZATION=admin:admin`：設定在連線至AEM Author環境時要使用的基本驗證認證（僅供開發）。 如果連線到Publish環境，就不需要此設定。
+ `REACT_APP_DEV_TOKEN`：開發權杖字串。 若要連線到遠端執行個體，您可以在基本驗證(user：pass)旁邊，透過雲端主控台的DEV權杖使用持有者驗證
+ `REACT_APP_SERVICE_TOKEN`：服務認證檔案的路徑。 若要連線到遠端執行個體，也可使用服務權杖完成驗證(從Developer Console下載檔案)。

### Proxy AEM要求

使用webpack開發伺服器(`npm start`)時，專案依賴使用`http-proxy-middleware`的[Proxy安裝程式](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)。 檔案設定於[src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js)，且依賴於`.env`和`.env.development`上設定的數個自訂環境變數。

如果連線到AEM作者環境，則需要設定對應的[驗證方法](#environment-variables)。

### 跨原始資源共用(CORS)

此React應用程式依賴在目標AEM環境中執行的AEM型CORS設定，並假設React應用程式以開發模式在`http://localhost:3000`上執行。  請檢閱[AEM Headless部署檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html)，以取得如何設定和設定CORS的詳細資訊。
