---
title: 永續GraphQL查詢 — 無頭的高級AEM概念 — GraphQL
description: 在Adobe Experience Manager(AEM)Headless的高級概念的本章中，瞭解如何使用參數建立和更新永續的GraphQL查詢。 瞭解如何在永續查詢中傳遞快取控制參數。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# 永續GraphQL查詢

永續查詢是儲存在Adobe Experience Manager(AEM)伺服器上的查詢。 客戶端可以發送具有查詢名稱的HTTPGET請求以執行該請求。 這種方法的好處是可快取。 雖然客戶端GraphQL查詢也可以使用無法快取的HTTPPOST請求執行，但永續查詢可以通過HTTP快取或CDN進行快取，從而提高效能。 永續查詢允許您簡化請求並提高安全性，因為您的查詢已封裝在伺服器上，而AEM且管理員完全控制了這些查詢。 在使用AEMGraphQL API時，最好使用永續查詢，並強烈建議使用它。

在上一章中，您探索了一些高級GraphQL查詢以收集WKND應用的資料。 在本章中，您將將這些查詢保留AEM到、更新它們，並瞭解如何對保留的查詢使用快取控制。

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 在繼續本章之前，請確保已完成前幾章。

本教程使用 [郵遞員](https://www.postman.com/) 執行HTTP請求。 在開始本章之前，請確保您已註冊到服務。 本教程還需要Postman應用的工作知識，如如何設定集合、建立變數和發出請求。 請參閱上 [構建請求](https://learning.postman.com/docs/sending-requests/requests/) 和 [發送您的第一個請求](https://learning.postman.com/docs/getting-started/sending-the-first-request/) 的子菜單。

在本章中，將上一章中探討的查詢保留為AEM。 您可以下載帶有這些標準GraphQL查詢的文本檔案 [這裡](assets/graphql-persisted-queries/advanced-concepts-aem-headless-graphql-queries.txt) 供參考。

## 目標 {#objectives}

在本章中，瞭解如何：

* 具有參數的Persist GraphQL查詢
* 更新保留的查詢
* 對永續查詢使用快取控制參數

## 永續查詢概述

此視頻概述了如何保留GraphQL查詢、更新查詢和使用快取控制。

>[!VIDEO](https://video.tv.adobe.com/v/340036/?quality=12&learn=on)

## 啟用保留查詢

首先，確保為實例中的WKND站點項目啟用永續查AEM詢。

1. 導航到 **工具** > **常規** > **配置瀏覽器**。

1. 選擇 **WKND站點**，然後選擇 **屬性** 的子菜單。

   ![設定瀏覽器](assets/graphql-persisted-queries/configuration-browser.png)

   在「配置屬性」頁上，您應看到 **GraphQL持久查詢** 權限已啟用。

   ![組態屬性](assets/graphql-persisted-queries/configuration-properties.png)

## 導入郵遞員集合

為了便於學習本教程，提供了郵遞員收藏。 或者，命令行工具如 `curl` 可以用。

1. 下載並安裝 [郵遞員](https://www.postman.com/)
1. 下載 [AdvancedConceptsofAEMHeadless.postman_collection.json](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/AdvancedConceptsofAEMHeadless.postman_collection.json)
1. 開啟郵遞員應用
1. 選擇 **檔案** > **導入** > **上載檔案** 選擇 `AdvancedConceptsofAEMHeadless.postman_collection.json` 的子菜單。

   ![導入郵遞員集合](assets/graphql-persisted-queries/import-postman-collection.png)

### 驗證

對作者實例發送查詢需AEM要身份驗證。 本教程基於AEMas a Cloud Service環境，並使用帶開發令牌的Bearer身份驗證。 要配置郵遞員集合的身份驗證，請執行以下步驟：

1. 要獲取開發令牌，請導航至雲開發者控制台，開啟 **整合** ，然後選擇 **獲取本地開發令牌**。

   ![獲取開發令牌](assets/graphql-persisted-queries/get-development-token.png)

1. 在郵遞員收藏中，導航至 **驗證** 頁籤 **持有者令牌** 的 **類型** 下拉菜單。

   ![在郵遞員中配置身份驗證](assets/graphql-persisted-queries/postman-authentication.png)

1. 將開發令牌輸入 **令牌** 的子菜單。 您可以通過變數傳遞令牌，如下一節所述。

   ![在郵遞員中添加開發令牌](assets/graphql-persisted-queries/postman-token.png)

### 變數 {#variables}

您可以通過郵遞員集合中的變數傳遞諸如身份驗證令牌和URI元件等值，以簡化流程。 在本教程中，請使用以下步驟建立變數：

1. 導航到 **變數** 頁籤，並建立以下變數：

   | 變數 | 值 |
   | --- | --- |
   | `AEM_SCHEME` | `https` |
   | `AEM_AUTH_TOKEN` | （您的開發令牌） |
   | `AEM_HOST` | (實例的主機AEM名) |
   | `AEM_PROJECT` | `wknd` |

1. 您還可以為要建立的每個永續查詢添加變數。 對於本教程，請保留以下查詢： `getAdventureAdministratorDetailsByAdministratorName`。 `getTeamByAdventurePath`。 `getLocationDetailsByLocationPath`。 `getTeamMembersByAdventurePath`。 `getLocationPathByAdventurePath`, `getTeamLocationByLocationPath`。

   建立以下變數：

   * `AEM_GET_ADVENTURE_ADMINISTRATOR_DETAILS_BY_ADMINISTRATOR_NAME` : `adventure-administrator-details-by-administrator-name`
   * `AEM_GET_ADVENTURE_ADMINISTRATOR_DETAILS_BY_ADMINISTRATOR_NAME` : `adventure-administrator-details-by-administrator-name`
   * `AEM_GET_TEAM_LOCATION_BY_LOCATION_PATH` : `team-location-by-location-path`
   * `AEM_GET_TEAM_MEMBERS_BY_ADVENTURE_PATH` : `team-members-by-adventure-path`
   * `AEM_GET_LOCATION_DETAILS_BY_LOCATION_PATH` : `location-details-by-location-path`
   * `AEM_GET_LOCATION_PATH_BY_ADVENTURE_PATH` : `location-path-by-adventure-path`
   * `AEM_GET_TEAM_BY_ADVENTURE_PATH` : `team-by-adventure-path`

   完成後， **變數** 的子菜單。

   ![郵遞員中的「變數」頁籤](assets/graphql-persisted-queries/postman-variables.png)

## 具有參數的Persist GraphQL查詢

在 [無AEM頭和GraphQL視頻系列](../video-series/graphql-persisted-queries.md)，您學習了如何建立永續GraphQL查詢。 在本節中，讓我們使用參數保持並執行GraphQL查詢。

### 建立永續查詢 {#create-persisted-query}

對於本示例，讓我們堅持 `getAdventureAdministratorDetailsByAdministratorName` 查詢。

>[!NOTE]
>
>HTTPPUT方法用於建立永續查詢，而HTTPPOST方法用於更新該查詢。

1. 首先，在郵遞員集合中添加新請求。 選擇HTTPPUT方法以建立永續查詢並使用以下請求URI:

   ```plaintext
   {{AEM_SCHEME}}://{{AEM_HOST}}/graphql/persist.json/{{AEM_PROJECT}}/{{AEM_GET_ADVENTURE_ADMINISTRATOR_DETAILS_BY_ADMINISTRATOR_NAME}}
   ```

   請注意，URI使用 `/graphql/persist.json` 操作。

1. 貼上 `getAdventureAdministratorDetailsByAdministratorName` GraphQL查詢到請求主體。 請注意，它是帶變數的標準GraphQL查詢 `name` 需要 `String`。

   ![將GraphQL查詢貼上到請求正文中](assets/graphql-persisted-queries/create-put-request.png)

1. 執行請求。 您應收到以下響應：

   ![執行PUT請求](assets/graphql-persisted-queries/success-put-request.png)

   您已成功建立名為 `adventure-administrator-details-by-administrator-name`。

### 執行永續查詢

讓我們執行您建立的永續查詢。

1. 使用以下請求URI在郵遞員集合中建立新GET請求：

   ```plaintext
   {{AEM_SCHEME}}://{{AEM_HOST}}/graphql/execute.json/{{AEM_PROJECT}}/{{AEM_GET_ADVENTURE_ADMINISTRATOR_DETAILS_BY_ADMINISTRATOR_NAME}}
   ```

   請注意，請求URI現在包括 `execute.json` 操作。

   如果按原樣執行此請求，則會引發錯誤，因為查詢需要變數 `name`。 必須將此變數作為參數傳遞到請求URI中。

   ![執行GET錯誤](assets/graphql-persisted-queries/execute-get-error.png)

1. 接下來，檢索名為Jacob Wester的管理員。 永續GraphQL查詢的參數必須與以前的URI元件分隔為 `;` 並在將它們傳入請求URI之前進行編碼。 在瀏覽器控制台中執行以下命令：

   ```js
   encodeURIComponent(";name=Jacob Wester")
   ```

   ![對URI參數進行編碼](assets/graphql-persisted-queries/encode-uri-parameter.png)

1. 從控制台複製結果，然後在請求URI的末尾以Postman形式貼上。 您應具有以下請求URI:

   ```plaintext
   {{AEM_SCHEME}}://{{AEM_HOST}}/graphql/execute.json/{{AEM_PROJECT}}/{{AEM_GET_ADVENTURE_ADMINISTRATOR_DETAILS_BY_ADMINISTRATOR_NAME}}%3Bname%3DJacob%20Wester
   ```

1. 執行GET請求。 您應收到以下響應：

   ![對URI參數進行編碼](assets/graphql-persisted-queries/success-get-request.png)

您現在已建立並執行帶參數的永續GraphQL查詢。

您可以按照上述步驟保留GraphQL查詢的其餘部分 [文本檔案](assets/graphql-persisted-queries/advanced-concepts-aem-headless-graphql-queries.txt) 使用在 [本章開始](#variables)。

滿 [郵遞員收藏](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/AdvancedConceptsofAEMHeadless.postman_collection.json) 也可用於下載和導入。

## 更新保留的查詢

使用PUT請求建立保留查詢時，必須使用POST請求更新現有的保留查詢。 在本教程中，讓我們更新名為 `adventure-administrator-details-by-administrator-name` 建立的 [上一節](#create-persisted-query)。

1. 複製上一節中用於PUT請求的頁籤。 在副本中，將HTTP方法更改為POST。

1. 在GraphQL查詢中，讓我們刪除 `plaintext` 格式 `administratorDetails` 的子菜單。

   ![更新GraphQL查詢](assets/graphql-persisted-queries/update-graphql-query.png)

1. 執行請求。 您應得到以下響應：

   ![POST響應](assets/graphql-persisted-queries/post-response.png)

您現在已更新 `adventure-administrator-details-by-administrator-name` 永續查詢。 在中始終更新GraphQL查詢AEM（如果進行更改）非常重要。

## 在永續查詢中傳遞快取控制參數 {#cache-control-all-adventures}

GraphQL APIAEM允許您向查詢添加快取控制參數以提高效能。

使用 `getAllAdventureDetails` 在上一章中建立的查詢。 查詢響應很大，控制其是有用的 `age` 在快取中。

此永續查詢稍後用於更新 [客戶端應用程式](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。

1. 在郵遞員集合中，建立新變數：

   ```plaintext
   AEM_GET_ALL_AT_ONCE: all-adventure-details
   ```

1. 建立新PUT請求以保留此查詢。

1. 在 **身體** 頁籤，選擇 **生** 資料類型。

   ![選擇原始資料類型](assets/graphql-persisted-queries/raw-data-type.png)

1. 要在查詢中使用快取控制，您需要將查詢包裝在JSON結構中，並在末尾添加快取控制參數。 將以下查詢複製並貼上到請求正文中：

   ```json
   {
   "query": " query getAllAdventureDetails($fragmentPath: String!) { adventureByPath(_path: $fragmentPath){ item { _path adventureTitle adventureActivity adventureType adventurePrice adventureTripLength adventureGroupSize adventureDifficulty adventurePrice adventurePrimaryImage{ ...on ImageRef{ _path mimeType width height } } adventureDescription { html json } adventureItinerary { html json } location { _path name description { html json } contactInfo{ phone email } locationImage{ ...on ImageRef{ _path } } weatherBySeason address{ streetAddress city state zipCode country } } instructorTeam { _metadata{ stringMetadata{ name value } } teamFoundingDate description { json } teamMembers { fullName contactInfo { phone email } profilePicture{ ...on ImageRef { _path } } instructorExperienceLevel skills biography { html } } } administrator { fullName contactInfo { phone email } biography { html } } } _references { ...on ImageRef { _path mimeType } ...on LocationModel { _path __typename } } } }", 
   "cache-control": { "max-age": 300 }
   }
   ```

   >[!CAUTION]
   >
   >包裝查詢不能包含換行符。

   您的請求現在應如下所示：

   ![PUT具有快取控制參數的請求](assets/graphql-persisted-queries/put-cache-control.png)

1. 執行請求。 您應該得到的響應表明 `all-adventure-details` 已成功建立永續查詢。

   ![使用快取控制參數建立永續查詢](assets/graphql-persisted-queries/success-put-cache-control.png)

## 恭喜！

恭喜！ 您現在已學會了如何使用參數永續GraphQL查詢、更新永續查詢以及將快取控制參數用於永續查詢。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)，您將在WKND應用中實現永續查詢的請求。

雖然本教程是可選的，但請確保在真實生產環境中發佈所有內容。 有關中的「作者」和「發佈」環境的AEM回顧，請參閱 [無AEM頭和GraphQL視頻系列](../video-series/author-publish-architecture.md)。