---
title: Android應用 — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Android應用程式演示了如何使用的GraphQL API查詢內AEM容。
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 985d52f02025dc9cb2b9c70ead4a88af07c63f29
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---

# Android應用

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Android應用程式演示了如何使用的GraphQL API查詢內AEM容。 的 [用AEM於Java的無頭客戶端](https://github.com/adobe/aem-headless-client-java) 用於執行GraphQL查詢並將資料映射到Java對象以為應用程式提供電源。

![帶無頭的Android Java應AEM用](./assets/android-java-app/android-app.png)


查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [安卓工作室](https://developer.android.com/studio)
+ [蠢貨](https://git-scm.com/)

## AEM要求

Android應用程式可與以下部AEM署選項配合使用。 所有部署都需要 [WKND站點v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本地設定使用 [AEM Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM6.5 SP13+快速啟動](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

Android應用程式設計為連接到 __AEM發佈__ 但是，如果Android應用程式的配置中提供了身份驗證，則它可以從AEM Author中源出內容。

## 如何使用

1. 克隆 `adobe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 啟動 [安卓工作室](https://developer.android.com/studio) 開啟資料夾 `android-app`
1. 修改檔案 `config.properties` 在 `app/src/main/assets/config.properties` 更新 `contentApi.endpoint` 與目標環境AEM匹配：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __基本身份驗證__

   的 `contentApi.user` 和 `contentApi.password` 驗證本AEM地用戶對WKND GraphQL內容的訪問權限。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下載 [Android虛擬設備](https://developer.android.com/studio/run/managing-avds) （最少API 28）。
1. 使用Android模擬器生成和部署應用。


### 連接到環AEM境

`10.0.2.2` 是 [特殊別名IP](https://developer.android.com/studio/run/emulator-networking) 用於當使用模擬器建立時的localhost `10.0.2.2:4502` 等於 `localhost:4502`。 如果連接到發AEM布環境（推薦），則不需要授權， `contentAPi.user` 和 `contentApi.password` 可以保留為空。

如果連接到作者AEM環境 [授權](https://github.com/adobe/aem-headless-client-java#using-authorization) 。 預設情況下，應用程式設定為使用基本身份驗證，用戶名和密碼為 `admin:admin`。 的 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供了使用 [基於令牌的身份驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)。 在中使用基於令牌的驗證更新客戶端生成器 `AdventureLoader.java` 和 `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## 代碼

以下是用於為應用程式提供動力的重要檔案和代碼的簡要摘要。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)。

### 永續查詢

遵循AEM無頭最佳實踐，iOS應用程式使AEM用GraphQL持久查詢來查詢冒險資料。 應用程式使用兩個永續查詢：

+ `wknd/adventures-all` 永續查詢，返回帶有AEM一組刪除屬性的所有冒險。 此永續查詢驅動初始視圖的冒險清單。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 永續查詢，返回單個冒險 `slug` （具有完整屬性集的唯一標識冒險的自定義屬性）。 此持久查詢可支援冒險詳細資訊視圖。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### 執行GraphQL永續查詢

通AEM過HTTPGET執行永續查詢， [用AEM於Java的無頭客戶端](https://github.com/adobe/aem-headless-client-java) 用於執行針對永續GraphQL查詢AEM並將冒險內容載入到應用中。

每個永續查詢都具有相應的「loader」類，該類非同步調用AEMHTTPGET端點，並使用自定義的查詢返回冒險資料 [資料模型](#data-models)。

+ `loader/AdventuresLoader.java`

   使用 `wknd-shared/adventures-all` 永續查詢。

+ `loader/AdventureLoader.java`

   通過 `slug` 參數，使用 `wknd-shared/adventure-by-slug` 永續查詢。

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL響應資料模型{#data-models}

`Adventure.java` 是一個Java POJO，它使用GraphQL請求中的JSON資料初始化，並為Android應用程式視圖中的使用建模。

### 檢視

Android應用程式使用兩個視圖來呈現移動體驗中的冒險資料。

+ `AdventureListFragment.java`

   調用 `AdventuresLoader` 並在清單中顯示返回的冒險。

+ `AdventureDetailFragment.java`

   調用 `AdventureLoader` 使用 `slug` 參數通過 `AdventureListFragment` 查看，並顯示單個冒險的詳細資訊。

### 遠程映像

`loader/RemoteImagesCache.java` 是一個實用程式類，可幫助在快取中準備遠程映像，以便它們可以與Android UI元素一起使用。 冒險內容通過URL引用AEM Assets的影像，此類用於顯示該內容。

## 其他資源

+ [無頭入門AEM- GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [用AEM於Java的無頭客戶端](https://github.com/adobe/aem-headless-client-java)
