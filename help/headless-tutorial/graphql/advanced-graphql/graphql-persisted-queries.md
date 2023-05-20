---
title: 永續GraphQL查詢 — 無頭的AEM高級概念 — GraphQL
description: 在Adobe Experience Manager(AEM)Headless的高級概念的本章中，瞭解如何使用參數建立和更新永續GraphQL查詢。 瞭解如何在永續查詢中傳遞快取控制參數。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# 持續性 GraphQL 查詢

永續查詢是儲存在Adobe Experience Manager(AEM)伺服器上的查詢。 客戶端可以發送具有查詢名稱的HTTPGET請求以執行該請求。 這種方法的好處是可快取。 雖然客戶端GraphQL查詢也可以使用無法快取的HTTPPOST請求執行，但永續查詢可以通過HTTP快取或CDN快取，從而提高效能。 永續查詢允許您簡化請求並提高安全性，因為您的查詢已封裝在伺服器上，而AEM且管理員完全控制了這些查詢。 是 **最佳實踐和強烈推薦** 使用GraphQLAPI時使AEM用保留查詢。

在上一章中，您已探索了一些高級GraphQL查詢，以收集WKND應用的資料。 在本章中，您將查詢保留AEM到並瞭解如何對永續查詢使用快取控制。

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 請確保 [上一章](explore-graphql-api.md) 在繼續本章之前已完成。

## 目標 {#objectives}

在本章中，瞭解如何：

* 使用參數保留GraphQL查詢
* 對永續查詢使用快取控制參數

## 審閱 _GraphQL永續查詢_ 配置設定

讓我們回顧一下 _GraphQL永續查詢_ 為實例中的WKND站點項目啟AEM用。

1. 導航到 **工具** > **常規** > **配置瀏覽器**。

1. 選擇 **WKND共用**，然後選擇 **屬性** 的子菜單。 在「配置屬性」頁上，您應看到 **GraphQL持久查詢** 權限已啟用。

   ![組態屬性](assets/graphql-persisted-queries/configuration-properties.png)

## 使用內置GraphiQL瀏覽器工具保留GraphQL查詢

在本節中，讓我們保留稍後在客戶端應用程式中使用的GraphQL查詢，以提取和呈現冒險內容片段資料。

1. 在GraphiQL瀏覽器中輸入以下查詢：

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   在保存查詢之前，請驗證該查詢是否有效。

1. 下一步點擊「另存為」，然後輸入 `adventure-details-by-slug` 作為查詢名稱。

   ![持久GraphQL查詢](assets/graphql-persisted-queries/persist-graphql-query.png)

## 通過編碼特殊字元執行帶變數的永續查詢

讓我們瞭解客戶端應用程式如何通過編碼特殊字元來執行具有變數的永續查詢。

要執行永續查詢，客戶端應用程式使用以下語法發出GET請求：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

執行永續查詢 _具有變數_，上述語法更改為：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

必須轉換特殊字元，如分號(;)、等號(=)、斜槓(/)和空格，才能使用相應的UTF-8編碼。

通過運行 `getAllAdventureDetailsBySlug` 從命令行終端查詢，我們在操作中回顧這些概念。

1. 開啟GraphiQL瀏覽器，然後按一下 **橢圓** (...) `getAllAdventureDetailsBySlug`，然後按一下 **複製URL**。 將複製的URL貼上到文本板中，如下所示：

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 添加 `yosemite-backpacking` 作為變數值

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 編碼分號(;)和等號(=)特殊字元

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 開啟命令行終端並使用 [屈爾](https://curl.se/) 運行查詢

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    如果針對AEM作者環境運行上述查詢，則必須發送憑據。 請參閱 [本地開發訪問令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) 來演示 [調用AEMAPI](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) 的子菜單。

另外， [如何執行永續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query)。 [使用查詢變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables), [對查詢URL進行編碼，供應用使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) 瞭解客戶端應用程式的永續查詢執行。

## 更新永續查詢中的快取控制參數 {#cache-control-all-adventures}

GraphQLAEM API允許您將預設的快取控制參數更新到查詢，以提高效能。 預設的快取控制值為：

* 60秒是客戶端（例如瀏覽器）的預設TTL（最大值=60）

* 7200秒是Dispatcher和CDN的預設(s-maxage=7200)TTL;也稱為共用快取

使用 `adventures-all` 查詢以更新快取控制參數。 查詢響應很大，控制其是有用的 `age` 在快取中。 此永續查詢稍後用於更新 [客戶端應用程式](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。

1. 開啟GraphiQL瀏覽器，然後按一下 **橢圓** (..)，然後按一下 **標題** 開啟 **快取配置** 模式。

   ![保留GraphQL頭選項](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 在 **快取配置** 模式，更新 `max-age` 標題值 `600 `秒（10分鐘），然後按一下 **保存**

   ![保留GraphQL快取配置](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


審閱 [快取永續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) 的子菜單。


## 恭喜！

恭喜！您現在已學會了如何使用參數保留GraphQL查詢、更新永續查詢以及將快取控制參數用於永續查詢。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)，您將在WKND應用中實現永續查詢的請求。
