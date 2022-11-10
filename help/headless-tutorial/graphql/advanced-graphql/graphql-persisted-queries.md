---
title: 持續GraphQL查詢 — AEM無周邊的進階概念 — GraphQL
description: 在Adobe Experience Manager(AEM)無周邊的進階概念章節中，了解如何使用參數建立和更新持續存在的GraphQL查詢。 了解如何在持續查詢中傳遞快取控制參數。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 0%

---

# 持續GraphQL查詢

持續查詢是儲存在Adobe Experience Manager(AEM)伺服器上的查詢。 用戶端可傳送具有查詢名稱的HTTPGET要求以執行它。 此方法的好處是可快取性。 雖然用戶端GraphQL查詢也可以使用HTTPPOST請求（無法快取）來執行，但可由HTTP快取或CDN快取持續的查詢，以提升效能。 持續查詢可讓您簡化請求並提高安全性，因為您的查詢已封裝在伺服器上，而AEM管理員可完全控制這些請求。 是 **最佳實務，強烈建議** 使用AEM GraphQL API時使用持續查詢的方式。

在上一章中，您已探索了一些進階GraphQL查詢，以收集WKND應用程式的資料。 在本章中，您將查詢保留至AEM，並了解如何對持續的查詢使用快取控制。

## 必備條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 請確保 [上一章](explore-graphql-api.md) 已完成，然後再繼續處理本章。

## 目標 {#objectives}

在本章中，了解如何：

* 使用參數保留GraphQL查詢
* 將快取控制參數與持續查詢搭配使用

## 檢閱 _GraphQL持續查詢_ 配置設定

讓我們回顧一下 _GraphQL持續查詢_ 在AEM例項中為WKND Site專案啟用。

1. 導覽至 **工具** > **一般** > **配置瀏覽器**.

1. 選擇 **WKND共用**，然後選取 **屬性** ，開啟設定屬性。 在設定屬性頁面上，您應會看到 **GraphQL持久查詢** 權限已啟用。

   ![組態屬性](assets/graphql-persisted-queries/configuration-properties.png)

## 使用GraphQL瀏覽器工具保存GraphQL查詢

在本節中，我們將保留GraphQL查詢，該查詢稍後用於用戶端應用程式，以擷取及呈現冒險內容片段資料。

1. 在GraphiQL資源管理器中輸入以下查詢：

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

   在保存查詢之前，請驗證該查詢是否工作。

1. 下一步點選「另存新檔」並輸入 `adventure-details-by-slug` 作為查詢名稱。

   ![Persist GraphQL查詢](assets/graphql-persisted-queries/persist-graphql-query.png)

## 通過對特殊字元進行編碼來使用變數執行持續查詢

讓我們來了解具有變數的持續查詢如何由用戶端應用程式透過編碼特殊字元來執行。

若要執行持續查詢，用戶端應用程式會使用下列語法提出GET要求：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

執行持續查詢 _含變數_，上述語法變更為：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

必須轉換分號(;)、等號(=)、斜線(/)和空格等特殊字元，才能使用對應的UTF-8編碼。

執行 `getAllAdventureDetailsBySlug` 從命令行終端查詢，我們將在實際操作中回顧這些概念。

1. 開啟GraphiQL瀏覽器，然後按一下 **橢圓** (...) `getAllAdventureDetailsBySlug`，然後按一下 **複製URL**. 將複製的URL貼到文字面板中，如下所示：

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 新增 `yosemite-backpacking` 作為變數值

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 將分號(;)和等號(=)編碼為特殊字元

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 開啟命令列終端並使用 [捲曲](https://curl.se/) 運行查詢

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    如果針對AEM製作環境執行上述查詢，您必須傳送憑證。 請參閱 [本機開發存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) 以展示 [呼叫AEM API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) 如需檔案詳細資訊。

此外，請檢閱 [如何執行持續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [使用查詢變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)，和 [對查詢URL進行編碼，以供應用程式使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) 了解客戶端應用程式持續執行的查詢。

## 更新持續查詢中的快取控制參數 {#cache-control-all-adventures}

AEM GraphQL API可讓您將預設的快取控制參數更新至查詢，以提升效能。 預設的快取控制值為：

* 用戶端（例如瀏覽器）的預設TTL為60秒（最大值=60）

* Dispatcher和CDN的預設TTL為7200秒(s-maxage=7200);也稱為共用快取

使用 `adventures-all` 查詢以更新快取控制參數。 查詢回應很大，控制其很實用 `age` 在快取中。 此持續查詢稍後將用於更新 [客戶端應用程式](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. 開啟GraphiQL瀏覽器，然後按一下 **橢圓** (...)，然後按一下永久查詢旁的 **標題** 開啟 **快取配置** 模式。

   ![保留GraphQL標頭選項](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 在 **快取配置** 強制回應，更新 `max-age` 標題值 `600 `秒（10分鐘），然後按一下 **儲存**

   ![Persist GraphQL快取配置](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


檢閱 [快取持續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) 以取得預設快取控制參數的詳細資訊。


## 恭喜！

恭喜！您現在已學習如何使用參數保留GraphQL查詢、更新持續查詢，以及使用快取控制參數來保留查詢。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)，您將在WKND應用程式中實作持續查詢的要求。
