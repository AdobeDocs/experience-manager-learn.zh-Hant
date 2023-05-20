---
title: 如何在無頭中使用大結AEM果集
description: 瞭解如何使用無頭大型結果AEM集。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: f47ce344-310f-4b4c-9340-b0506289f468
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 1%

---

# 無頭大結AEM果集

無AEM頭GraphQL查詢可返回大結果。 本文介紹如何在Headless中使用大AEM的結果，以確保應用程式的最佳效能。

AEM無頭支撐 [偏移/限制](#list-query) 和 [基於游標的分頁](#paginated-query) 查詢到較大結果集的較小子集。 可以發出多個請求以收集所需的盡可能多的結果。

以下示例使用小的結果子集（每個請求有四條記錄）來演示這些技術。 在實際應用程式中，每次請求都會使用更多的記錄來提高效能。 每個請求50條記錄是一個良好的基線。

## 內容片段模型

分頁和排序可用於任何內容片段模型。

## GraphQL永續查詢

在處理大型資料集時，可使用偏移和限制以及基於游標的分頁來檢索資料的特定子集。 但是，這兩種技術之間存在著一些差異，在某些情況下，這可能使一種技術比另一種技術更合適。

### 偏移/限制

列出查詢，使用 `limit` 和 `offset` 提供了明確起點(`offset`)和要檢索的記錄數(`limit`)。 此方法允許從完整結果集內的任何位置選擇結果子集，例如跳到特定結果頁面。 雖然易於實施，但在處理大結果時速度慢且效率低，因為檢索許多記錄需要掃描以前的所有記錄。 當偏移值較高時，這種方法也會導致效能問題，因為它可能需要檢索和丟棄許多結果。

#### GraphQL查詢

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### 查詢變數

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL響應

產生的JSON響應包含第2、第3、第4和第5個最昂貴的冒險。 前兩次的結果價格相同(`4500` 所以 [清單查詢](#list-queries) 指定價格相同的冒險，然後按標題按升序排序。)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### 分頁查詢

基於游標的分頁（在分頁查詢中可用）涉及使用游標（對特定記錄的引用）來檢索下一組結果。 這種方法效率更高，因為它避免了掃描所有先前記錄以檢索所需資料子集的需要。 已分頁的查詢非常適合於在大型結果集中從開始、到中間的某個點或到最後進行迭代。 列出查詢，使用 `limit` 和 `offset` 提供了明確起點(`offset`)和要檢索的記錄數(`limit`)。 此方法允許從完整結果集內的任何位置選擇結果子集，例如跳到特定結果頁面。 雖然易於實施，但在處理大結果時速度慢且效率低，因為檢索許多記錄需要掃描以前的所有記錄。 當偏移值較高時，這種方法也會導致效能問題，因為它可能需要檢索和丟棄許多結果。

#### GraphQL查詢

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### 查詢變數

```json
{
  "first": 3
}
```

#### GraphQL響應

產生的JSON響應包含第2、第3、第4和第5個最昂貴的冒險。 前兩次的結果價格相同(`4500` 所以 [清單查詢](#list-queries) 指定價格相同的冒險，然後按標題按升序排序。)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### 下一組分頁結果

可以使用 `after` 參數和 `endCursor` 值。 如果沒有更多結果可以讀取， `hasNextPage` 是 `false`。

##### 查詢變數

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## 反應示例

以下是演示如何使用的React示例 [偏移和限制](#offset-and-limit) 和 [基於游標的分頁](#cursor-based-pagination) 接近。 通常，每個請求的結果數較大，但就這些示例而言，限制設定為5。

### 偏移和限制示例

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

使用偏移和限制，可以容易地檢索和顯示結果子集。

#### useEffect掛接

的 `useEffect` 掛接調用保留查詢(`adventures-by-offset-and-limit`)，檢索歷險記清單。 查詢使用 `offset` 和 `limit` 參數，指定要檢索的起始點和結果數。 的 `useEffect` 在 `page` 值更改。


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Component

元件使用 `useOffsetLimitAdventures` 鈎子來取出歷險記清單。 的 `page` 值將遞增和遞減，以提取下一組和上一組結果。 的 `hasMore` 值用於確定是否應啟用下一頁按鈕。

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### 分頁示例

![分頁示例](./assets/large-results/paginated-example.png)

_每個紅色框表示離散分頁的HTTPGraphQL查詢。_

使用基於游標的分頁，可以通過增量收集結果並將其與現有結果連接來輕鬆檢索和顯示大型結果集。


#### 使用效果掛接

的 `useEffect` 掛接調用保留查詢(`adventures-by-paginated`)，檢索歷險記清單。 查詢使用 `first` 和 `after` 參數，以指定要檢索的結果數和要從中啟動的游標數。 `fetchData` 連續循環，收集下一組分頁結果，直到沒有更多要提取的結果。

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Component

元件使用 `usePaginatedAdventures` 鈎子來取出歷險記清單。 的 `queryCount` 值用於顯示為檢索冒險項清單而發出的HTTP請求數。

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
