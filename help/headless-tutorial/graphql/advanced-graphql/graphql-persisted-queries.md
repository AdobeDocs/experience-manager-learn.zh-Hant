---
title: 持續性GraphQL查詢 — AEM Headless的進階概念 — GraphQL
description: 在Adobe Experience Manager (AEM) Headless的進階概念的本章中，瞭解如何使用引數建立和更新持續的GraphQL查詢。 瞭解如何在持續性查詢中傳遞快取控制引數。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 183
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# 持續性 GraphQL 查詢

持續查詢是儲存在Adobe Experience Manager (AEM)伺服器上的查詢。 使用者端可以傳送具有查詢名稱的HTTP GET要求來執行它。 此方法的好處是可快取。 雖然使用者端GraphQL查詢也可以使用HTTP POST請求執行（無法快取），但HTTP快取或CDN可以快取持續查詢，從而提高效能。 持續查詢可讓您簡化要求並提高安全性，因為您的查詢會封裝在伺服器上，且AEM管理員可完全控制這些查詢。 使用AEM GraphQL API時，是&#x200B;**最佳實務並強烈建議**&#x200B;使用持續查詢。

在上一章中，您已探索一些進階GraphQL查詢，以收集WKND應用程式的資料。 在本章中，您會將查詢保留到AEM，並瞭解如何對保留的查詢使用快取控制。

## 先決條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 在繼續此章節之前，請確定已完成[先前的章節](explore-graphql-api.md)。

## 目標 {#objectives}

在本章中，瞭解如何：

* 使用引數保留GraphQL查詢
* 對持久查詢使用cache-control引數

## 檢閱&#x200B;_GraphQL持續查詢_&#x200B;組態設定

假設已在您的AEM執行個體中為WKND網站專案啟用&#x200B;_GraphQL持續查詢_。

1. 瀏覽至&#x200B;**工具** > **一般** > **設定瀏覽器**。

1. 選取「**WKND共用**」，然後在頂端導覽列中選取「**屬性**」以開啟設定屬性。 在[組態屬性]頁面上，您應該會看到&#x200B;**GraphQL持續查詢**&#x200B;許可權已啟用。

   ![組態屬性](assets/graphql-persisted-queries/configuration-properties.png)

## 使用內建GraphiQL Explorer工具保留GraphQL查詢

在本節中，我們將保留GraphQL查詢，該查詢稍後用於使用者端應用程式中，以擷取及轉譯冒險內容片段資料。

1. 在GraphiQL Explorer中輸入以下查詢：

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

   在儲存查詢之前，請驗證查詢是否有效。

1. 接著點選「另存新檔」，並輸入`adventure-details-by-slug`作為查詢名稱。

   ![保留GraphQL查詢](assets/graphql-persisted-queries/persist-graphql-query.png)

## 透過編碼特殊字元來執行包含變數的持久查詢

讓我們瞭解含有變數的持續查詢如何由使用者端應用程式藉由編碼特殊字元來執行。

若要執行持續查詢，使用者端應用程式會使用下列語法發出GET要求：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

若要使用變數&#x200B;_執行持續查詢_，上述語法會變更為：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

特殊字元(如分號(；)、等號(=)、斜線(/)和空格)必須轉換為使用對應的UTF-8編碼。

從命令列終端機執行`getAllAdventureDetailsBySlug`查詢，我們檢閱這些概念的實際運作情況。

1. 開啟GraphiQL Explorer，然後按一下永久查詢`getAllAdventureDetailsBySlug`旁的&#x200B;**省略符號** (...)，然後按一下&#x200B;**複製URL**。 將複製的URL貼到文字板中，如下所示：

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 將`yosemite-backpacking`新增為變數值

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 編碼分號(；)和等號(=)特殊字元

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 開啟命令列終端機，並使用[Curl](https://curl.se/)執行查詢

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    如果對AEM作者環境執行上述查詢，您必須傳送認證。 請參閱[本機開發存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html?lang=zh-Hant)以示範，並參閱[呼叫AEM API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=zh-Hant#calling-the-aem-api)以取得檔案詳細資訊。

此外，請檢閱[如何執行持久查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=zh-Hant#execute-persisted-query)、[使用查詢變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=zh-Hant#query-variables)和[編碼查詢URL以供應用程式使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=zh-Hant#encoding-query-url)，以瞭解使用者端應用程式如何執行持久查詢。

## 更新持續性查詢中的快取控制引數 {#cache-control-all-adventures}

AEM GraphQL API可讓您更新查詢的預設快取控制引數，以提高效能。 預設cache-control值為：

* 60秒是使用者端（例如瀏覽器）的預設(maxage=60) TTL

* 7200秒是Dispatcher和CDN的預設(s-maxage=7200) TTL；也稱為共用快取

使用`adventures-all`查詢來更新快取控制引數。 查詢回應很大，在快取中控制其`age`很有用。 此持續查詢稍後將用於更新[使用者端應用程式](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。

1. 開啟GraphiQL Explorer，然後按一下永久查詢旁的&#x200B;**省略符號** (...)，然後按一下&#x200B;**標題**&#x200B;以開啟&#x200B;**快取設定**&#x200B;強制回應視窗。

   ![保留GraphQL標頭選項](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 在&#x200B;**快取設定**&#x200B;強制回應視窗中，將`max-age`標頭值更新為`600 `秒（10分鐘），然後按一下&#x200B;**儲存**

   ![保留GraphQL快取設定](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


檢閱[快取您的持續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=zh-Hant#caching-persisted-queries)，以取得預設快取控制引數的詳細資訊。


## 恭喜！

恭喜！您現在已瞭解如何使用引數保留GraphQL查詢、更新持久查詢，以及對持久查詢使用快取控制引數。

## 後續步驟

在[下一個章節](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)中，您將會在WKND應用程式中實作持續性查詢的要求。
