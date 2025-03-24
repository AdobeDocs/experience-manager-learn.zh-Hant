---
title: 如何在AEM Headless中使用大型結果集
description: 瞭解如何使用AEM Headless處理大型結果集。
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---

# AEM Headless中的大型結果集

AEM Headless GraphQL查詢可能會傳回大量結果。 本文說明如何在AEM Headless中使用大型結果，以確保您的應用程式獲得最佳效能。

AEM Headless支援對較大結果集的較小子集進行[位移/限制](#list-query)和[游標式分頁](#paginated-query)查詢。 可以提出多個請求，以收集所需數量的結果。

以下範例使用結果的小子集（每個請求四個記錄）來示範技術。 在真實應用程式中，您會在每個請求中使用較大量的記錄來改善效能。 每個請求50筆記錄是很好的基準。

## 內容片段模型

分頁和排序可用於任何內容片段模式。

## GraphQL持續查詢

使用大型資料集時，位移和限制以及游標型分頁都可用來擷取資料的特定子集。 不過，這兩種技術之間有些差異，可能會讓其中一種在某些情況下比另一種更適合。

### 位移/限制

使用`limit`和`offset`的清單查詢提供直接的方法，指定起始點(`offset`)和要擷取的記錄數目(`limit`)。 此方法允許從完整結果集中的任何位置選擇結果的子集，例如跳至結果的特定頁面。 雖然實作容易，但在處理大量結果時可能會變慢且效率低下，因為擷取許多記錄需要掃描所有先前的記錄。 當位移值很高時，此方法也可能導致效能問題，因為它可能需要擷取和捨棄許多結果。

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

#### GraphQL回應

產生的JSON回應包含第2、第3、第4和第5最昂貴的冒險。 結果中的前兩個冒險具有相同的價格（`4500`，因此[清單查詢](#list-queries)會指定具有相同價格的冒險，然後依標題遞增排序。）

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

分頁查詢中可用的游標型分頁，涉及使用游標（對特定記錄的參照）來擷取下一組結果。 此方法更有效率，因為它不需要掃描所有先前的記錄來擷取所需的資料子集。 分頁查詢非常適合從頭到中間的某個點或到結尾反複處理大型結果集。 使用`limit`和`offset`的清單查詢提供直接的方法，指定起始點(`offset`)和要擷取的記錄數目(`limit`)。 此方法允許從完整結果集中的任何位置選擇結果的子集，例如跳至結果的特定頁面。 雖然實作容易，但在處理大量結果時可能會變慢且效率低下，因為擷取許多記錄需要掃描所有先前的記錄。 當位移值很高時，此方法也可能導致效能問題，因為它可能需要擷取和捨棄許多結果。

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

#### GraphQL回應

產生的JSON回應包含第2、第3、第4和第5最昂貴的冒險。 結果中的前兩個冒險具有相同的價格（`4500`，因此[清單查詢](#list-queries)會指定具有相同價格的冒險，然後依標題遞增排序。）

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

可以使用`after`引數以及上一個查詢的`endCursor`值來擷取下一組結果。 如果沒有其他可擷取的結果，`hasNextPage`就是`false`。

##### 查詢變數

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React範例

下列是React範例，示範如何使用[位移和限制](#offset-and-limit)以及[以游標為基礎的分頁](#cursor-based-pagination)方法。 通常每個請求的結果數量會較多，但在這些範例中，限制設為5。

### 位移和限制範例

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

使用位移和限制，可以輕鬆擷取和顯示結果子集。

#### useEffect勾點

`useEffect`勾點會叫用持續查詢(`adventures-by-offset-and-limit`)，以擷取Adventures清單。 查詢使用`offset`和`limit`引數來指定要擷取的結果的起點和數目。 當`page`值變更時，就會叫用`useEffect`連結。


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

#### 元件

元件使用`useOffsetLimitAdventures`連結來擷取Adventures清單。 `page`值會遞增或遞減，以擷取下一個和上一個結果集。 `hasMore`值是用來決定是否應啟用下一頁按鈕。

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

### 分頁範例

![分頁範例](./assets/large-results/paginated-example.png)

_每個紅色方塊代表分散式分頁HTTP GraphQL查詢。_

使用游標式分頁，可以透過增量收集結果並將其串連到現有結果來輕鬆擷取和顯示大型結果集。


#### UseEffect勾點

`useEffect`勾點會叫用持續查詢(`adventures-by-paginated`)，以擷取Adventures清單。 查詢使用`first`和`after`引數來指定要擷取的結果數目以及游標開始來源。 `fetchData`持續回圈，收集下一組分頁結果，直到沒有更多可擷取的結果為止。

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

#### 元件

元件使用`usePaginatedAdventures`連結來擷取Adventures清單。 `queryCount`值是用來顯示為了擷取Adventures清單而提出的HTTP要求數目。

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
