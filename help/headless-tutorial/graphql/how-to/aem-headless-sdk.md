---
title: 使用無AEM頭SDK
description: 瞭解如何使用無頭SDK進行AEMGraphQL查詢。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# 無AEM頭SDK

Headless AEM SDK是一組庫，客戶端可以使用這些庫通過HTTP快速而輕AEM松地與Headless API交互。

Headless AEM SDK適用於各種平台：

+ [用AEM於客戶端瀏覽器(JavaScript)的無頭SDK](https://github.com/adobe/aem-headless-client-js)
+ [用AEM於server-side/Node.js的無頭SDK(JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [用AEM於Java™的無頭SDK](https://github.com/adobe/aem-headless-client-java)

## GraphQL查詢

使AEM用GraphQL查詢(與 [永續GraphQL查詢](#persisted-graphql-queries))允許開發人員在代碼中定義查詢，並準確指定從中請求的AEM內容。

GraphQL查詢的效能往往比永續查詢低，因為它們是使用HTTPPOST執行的，而CDN和Dispatcher層的快取可AEM用性較低。

### 代碼示例{#graphql-queries-code-examples}

以下是如何執行GraphQL查詢的代碼示AEM例。

+++ JavaScript示例

安裝 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) 通過運行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代碼示例說明如AEM何使用 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) npm模組 `async/await` 語法。 JavaScriptAEM的無頭SDK也支援 [Promise語法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client)。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ 反應useEffect(..) 示例

安裝 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) 通過運行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代碼示例說明如何使用 [反應useEffect(..) 鈎](https://reactjs.org/docs/hooks-effect.html) 執行對GraphQL的異AEM步調用。

使用 `useEffect` 使React中的非同步GraphQL調用非常有用，因為它：

1. 提供非同步調用的同步包AEM裝。
1. 減少不必要的AEM請求。

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

導入和使用 `useGraphQL` 掛接到要查詢的React元件AEM。

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## 永續GraphQL查詢

使AEM用GraphQL使用永續查詢進行查詢(與 [常規GraphQL查詢](#graphl-queries))允許開發人員在中保留查詢(但不AEM是查詢結果)，然後請求按名稱執行查詢。 永續查詢與SQL資料庫中儲存過程的概念類似。

永續查詢通常比常規GraphQL查詢效能更高，因為永續查詢是使用HTTPGET執行的，而HTTP在CDN和Dispatcher層更具有快取AEM能力。 永續查詢也有效，定義API，並解除開發人員瞭解每個內容片段模型的詳細資訊的必要性。

### 代碼示例{#persisted-graphql-queries-code-examples}

下面是如何針對執行GraphQL永續查詢的代碼示AEM例。

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
```

+++

+++ 反應useEffect(..) 示例

安裝 [@adobe/aem無頭客戶端 — js](https://github.com/adobe/aem-headless-client-js) 通過運行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代碼示例說明如何使用 [反應useEffect(..) 鈎](https://reactjs.org/docs/hooks-effect.html) 執行對GraphQL的異AEM步調用。

使用 `useEffect` 在React中使非同步GraphQL調用非常有用，因為：

1. 它為對的非同步調用提供同步包AEM裝。
1. 它減少了不必要的AEM需求。

此代碼假定具有名稱的永續查詢 `wknd/adventureNames` 已在AEM作者上建立並發佈到AEM發佈。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

從React代碼的其他位置調用此代碼。

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
