---
title: 探索AEMGraphQLAPI — 無頭的高級AEM概念 — GraphQL
description: 使用GraphiQL IDE發送GraphQL查詢。 瞭解使用篩選器、變數和指令的高級查詢。 查詢片段和內容引用，包括來自多行文本欄位的引用。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 0%

---

# 瀏覽AEMGraphQLAPI

中的GraphQLAPIAEM允許您將內容片段資料公開到下游應用程式。 在基本教程中 [多步GraphQL教程](../multi-step/explore-graphql-api.md)，您使用GraphiQL瀏覽器test和細化GraphQL查詢。

在本章中，您使用GraphiQL瀏覽器定義更高級的查詢，以收集在 [上一章](../advanced-graphql/author-content-fragments.md)。

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 在繼續本章之前，請確保已完成前幾章。

## 目標 {#objectives}

在本章中，您將學習如何：

* 使用查詢變數篩選帶引用的內容片段清單
* 用於片段引用內容的過濾器
* 從多行文本欄位查詢內聯內容和片段引用
* 使用指令進行查詢
* 查詢JSON對象內容類型

## 使用GraphiQL瀏覽器


的 [GraphiQL瀏覽器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) 工具使開發人員能夠針對當前環境中的內容建立和testAEM查詢。 GraphiQL工具還使用戶能夠 **保留或保存** 要由生產設定中的客戶端應用程式使用的查詢。

接下來，使用內置AEM的GraphiQL Explorer瞭解GraphQLAPI的功能。

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **GraphQL查詢編輯器**。

   ![導航到GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>在中，需要手AEM動安裝GraphiQL Explorer（又稱GraphiQL IDE）工具的某些版本(6.X.X)，請遵循 [這裡的指示](../how-to/install-graphiql-aem-6-5.md)。

1. 在右上角，確保端點設定為 **WKND共用終結點**。 更改 _端點_ 此處顯示下拉值 _永續查詢_ 左上角。

   ![設定GraphQL端點](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

這將將所有查詢範圍限定為在 **WKND共用** 項目。


## 使用查詢變數篩選內容片段清單

在上一個 [多步GraphQL教程](../multi-step/explore-graphql-api.md)，您定義並使用基本永續查詢來獲取內容片段資料。 在此，您可以擴展此知識，並通過將變數傳遞到永續查詢來過濾內容片段資料。

在開發客戶端應用程式時，通常需要基於動態參數篩選內容片段。 GraphQLAEM API允許您將這些參數作為變數在查詢中傳遞，以避免在運行時在客戶端上構造字串。 有關GraphQL變數的詳細資訊，請參見 [GraphQL文檔](https://graphql.org/learn/queries/#variables)。

在此示例中，查詢具有特定技能的所有教師。

1. 在GraphiQL IDE中，將以下查詢貼上到左側面板中：

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   的 `listPersonBySkill` 上面的查詢接受一個變數(`skillFilter`) `String`。 此查詢對所有人員內容片段執行搜索，並根據 `skills` 欄位和傳入的字串 `skillFilter`。

   的 `listPersonBySkill` 包括 `contactInfo` 屬性，它是對前幾章中定義的「聯繫人資訊」模型的片段引用。 「聯繫資訊」模型包含 `phone` 和 `email` 的子菜單。 查詢中至少必須存在其中一個欄位才能正確執行。

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. 接下來，我們定義 `skillFilter` 找到所有滑雪高手。 在GraphiQL IDE的「查詢變數」面板中貼上以下JSON字串：

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. 執行查詢。 結果應與以下內容類似：

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

按 **播放** 按鈕。 您應看到上一章中內容片段的結果：

![按技能結果列出的人員](assets/explore-graphql-api/person-by-skill.png)

## 用於片段引用內容的過濾器

GraphQLAEM API允許您查詢嵌套的內容片段。 在上一章中，您添加了三個對Adventure Content Fragment的新片段引用： `location`。 `instructorTeam`, `administrator`。 現在，讓我們過濾所有具有特定名稱的管理員的冒險。

>[!CAUTION]
>
>只有一個模型才能作為此查詢的引用才能正確執行。

1. 在GraphiQL IDE中，將以下查詢貼上到左側面板中：

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. 接下來，在「查詢變數」面板中貼上以下JSON字串：

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   的 `getAdventureAdministratorDetailsByAdministratorName` 查詢篩選所有冒險項，用於任何 `administrator` 共 `fullName` 「Jacob Wester」，從兩個嵌套內容片段返回資訊：探險和教練。

1. 執行查詢。 結果應與以下內容類似：

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## 從多行文本欄位查詢內聯引用 {#query-rte-reference}

GraphQLAEM API允許您查詢多行文本欄位中的內容和片段引用。 在上一章中，您將這兩個參照類型都添加到 **說明** 欄位。 現在，我們檢索這些參照。

1. 在GraphiQL IDE中，將以下查詢貼上到左側面板中：

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   的 `getTeamByAdventurePath` 查詢按路徑篩選所有冒險項並返回資料 `instructorTeam` 特定冒險的片段引用。

   `_references` 是系統生成的欄位，用於顯示引用，包括插入到多行文本欄位中的引用。

   的 `getTeamByAdventurePath` 查詢檢索多個引用。 首先，它使用 `ImageRef` 要檢索的對象 `_path` 和 `__typename` 作為內容引用插入多行文本欄位的影像。 接下來，它使用 `LocationModel` 以檢索插入到同一欄位中的位置內容片段的資料。

   查詢還包括 `_metadata` 的子菜單。 這允許您檢索團隊內容片段的名稱，並稍後在WKND應用中顯示它。

1. 接下來，在「查詢變數」面板中貼上以下JSON字串，以獲取Yosemite背包冒險：

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. 執行查詢。 結果應與以下內容類似：

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   的 `_references` 欄位顯示徽標影像和插入到中的Yosemite Valley Lodge內容片段 **說明** 的子菜單。


## 使用指令進行查詢

有時，在開發客戶端應用程式時，您需要有條件地更改查詢的結構。 在這種情況下，AEMGraphQLAPI允許您使用GraphQL指令，以便根據提供的條件更改查詢的行為。 有關GraphQL指令的詳細資訊，請參見 [GraphQL文檔](https://graphql.org/learn/queries/#directives)。

在 [上一節](#query-rte-reference)，您學習了如何在多行文本欄位中查詢內聯引用。 從 `description` 在 `plaintext` 的子菜單。 接下來，讓我們展開該查詢，並使用指令有條件地檢索 `description` 的 `json` 。

1. 在GraphiQL IDE中，將以下查詢貼上到左側面板中：

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   上面的查詢接受另一個變數(`includeJson`) `Boolean`，也稱為查詢指令。 指令可用於有條件地包括來自 `description` 的 `json` 基於傳入的布爾值的格式 `includeJson`。

1. 接下來，在「查詢變數」面板中貼上以下JSON字串：

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. 執行查詢。 您應獲得與上一節中相同的結果。 [如何在多行文本欄位中查詢內聯引用](#query-rte-reference)。

1. 更新 `includeJson` 指令 `true` 並再次執行查詢。 結果應與以下內容類似：

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## 查詢JSON對象內容類型

請記住，在上一章的創作內容片段中，您向 **各季天氣** 的子菜單。 現在，我們在位置內容片段中檢索該資料。

1. 在GraphiQL IDE中，將以下查詢貼上到左側面板中：

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. 接下來，在「查詢變數」面板中貼上以下JSON字串：

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. 執行查詢。 結果應與以下內容類似：

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   的 `weatherBySeason` 欄位包含在上一章中添加的JSON對象。

## 同時查詢所有內容

到目前為止，已執行了多個查詢來說明GraphQLAPIAEM的功能。

只能使用單個查詢檢索同一資料，此查詢稍後在客戶端應用程式中用於檢索其他資訊，如位置、團隊名稱、冒險的團隊成員：

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## 恭喜！

恭喜！您現在已經測試了高級查詢，以收集您在上一章中建立的內容片段的資料。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md)，您將學習如何永續GraphQL查詢，以及為什麼在應用程式中使用永續查詢是最佳做法。
