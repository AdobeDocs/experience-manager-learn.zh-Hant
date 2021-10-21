---
title: React App - AEM Headless Example
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 提供的React應用程式示範如何使用AEM的GraphQL API來查詢內容。 The AEM Headless Client for JavaScript is used to execute the GraphQL queries that power the app.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 3%

---


# React應用程式

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 提供的React應用程式示範如何使用AEM的GraphQL API來查詢內容。 The AEM Headless Client for JavaScript is used to execute the GraphQL queries that power the app.

![React Application](./assets/react-screenshot.png)

提供完整的逐步教學課程 [此處](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM需求

應用程式的設計目的是連線至AEM **作者** 或 **發佈** 環境，與 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安裝。

* [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hant)

建議 [將WKND參考站點部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). A local setup using [the AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) or [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) can also be used.

## How to use

1. Clone the `aem-guides-wknd-graphql` repository:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd/react-app/.env.development` 檔案，並確保 `REACT_APP_HOST_URI` 指向您的target AEM例項。 更新驗證方法（如果連線至製作例項）。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Open a terminal and run the commands:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. 新瀏覽器視窗應載入 [http://localhost:3000](http://localhost:3000)
1. A list of adventures from the WKND reference site should be displayed on the application.

## 程式碼

Below is a brief summary of the important files and code used to power the application. The full code can be found on [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM Headless Client for JavaScript

此 [AEM無頭用戶端](https://github.com/adobe/aem-headless-client-js) 用於執行GraphQL查詢。 The AEM Headless Client provides two methods for executing queries, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) and [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` 會針對AEM內容執行標準GraphQL查詢，這是最常見的查詢執行類型。

[Persisted Queries](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) are a feature in AEM that caches the results of a GraphQL query and then makes the result available over GET. 持續查詢應用於將不斷執行的通用查詢。 In this application the list of Adventures is the first query executed on the home screen. 此查詢將非常受歡迎，因此應使用持續查詢。 `runPersistedQuery` 對持續查詢端點執行要求。

`src/api/useGraphQL.js` 是 [反應效果鈎](https://reactjs.org/docs/hooks-overview.html#effect-hook) 會監聽參數的變更 `query` 和 `path`. 若 `query` 空白，則會根據 `path`. Here is where the AEM Headless Client is constructed and used to fetch data.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Adventure content

應用程式主要會顯示歷險記清單，並提供使用者點選以進入歷險記詳細資訊的選項。

`Adventures.js`  — 顯示歷險記的卡片清單。