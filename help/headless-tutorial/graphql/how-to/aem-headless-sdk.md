---
title: 使用無AEM頭SDK
description: 瞭解如何使用無頭SDK進行GraphQLAEM查詢。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 31948793786a2c430533d433ae2b9df149ec5fc0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 10%

---

# 無AEM頭SDK

Headless AEM SDK是一組庫，客戶端可以使用這些庫通過HTTP快速而輕AEM松地與Headless API交互。

Headless AEM SDK適用於各種平台：

+ [適用於用戶端瀏覽器的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [適用於伺服器端/Node.js 的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [適用於 Java™ 的 AEM Headless SDK](https://github.com/adobe/aem-headless-client-java)

## 持續性 GraphQL 查詢

使用AEM永續查詢查詢使用GraphQL(與 [客戶端定義的GraphQL查詢](#graphl-queries))允許開發人員在中保留查詢(但不AEM是查詢結果)，然後請求按名稱執行查詢。 永續查詢與SQL資料庫中儲存過程的概念類似。

永續查詢比客戶端定義的GraphQL查詢效能更高，因為永續查詢是使用HTTPGET執行的，HTTP在CDN和Dispatcher層可進行緩AEM存。 永續查詢也有效，定義API，並解除開發人員瞭解每個內容片段模型的詳細資訊的必要性。

### 代碼示例{#persisted-graphql-queries-code-examples}

下面是如何對執行GraphQL永續查詢的代碼示AEM例。

+++ JavaScript示例

安裝 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) 通過運行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代碼示例說明如AEM何使用 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) npm模組 `async/await` 語法。 JavaScriptAEM的無頭SDK也支援 [Promise語法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client)。

此代碼假定具有名稱的永續查詢 `wknd/adventureNames` 已在AEM作者上建立並發佈到AEM發佈。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ 反應useEffect(..) 示例

安裝 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) 通過運行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代碼示例說明如何使用 [反應useEffect(..) 鈎](https://reactjs.org/docs/hooks-effect.html) 執行對GraphQL的非同步AEM呼叫。

使用 `useEffect` 在React中使非同步GraphQL呼叫非常有用，因為：

1. 它為對的非同步調用提供同步包AEM裝。
1. 它減少了不必要的AEM需求。

此代碼假定具有名稱的永續查詢 `wknd-shared/adventure-by-slug` 已在AEM作者上建立，並已使用GraphiQL發佈到AEM發佈。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

調用自定義反應 `useEffect` 從React元件的其他位置掛接。

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

新建 `useEffect` 可以為React應用程式使用的每個永續查詢建立掛接。

+++

<p> </p>

## GraphQL查詢

支AEM持客戶端定義的GraphQL查詢，但AEM是最好使用 [永續GraphQL查詢](#persisted-graphql-queries)。

## Webpack 5+

無AEM頭JS SDK依賴於 `util` 預設情況下，Webpack 5+中未包含此內容。 如果使用Webpack 5+，並收到以下錯誤：

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

添加以下內容 `devDependencies` 到 `package.json` 檔案：

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

然後運行 `npm install` 安裝依賴項。
