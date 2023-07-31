---
title: Android應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此Android應用程式示範了如何使用AEM的GraphQL API來查詢內容。
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 5%

---

# Android應用程式

範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此Android應用程式示範了如何使用AEM的GraphQL API來查詢內容。 此 [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java) 可用來執行GraphQL查詢，並將資料對應至Java物件以支援應用程式。

![使用AEM Headless的Android Java應用程式](./assets/android-java-app/android-app.png)


檢視 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 必備條件 {#prerequisites}

下列工具應在本機安裝：

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM需求

Android應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即將安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

Android應用程式是專為連線至 __AEM發佈__ 不過，如果在Android應用程式的設定中提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 並開啟資料夾 `android-app`
1. 修改檔案 `config.properties` 在 `app/src/main/assets/config.properties` 和更新 `contentApi.endpoint` 比對目標AEM環境：

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __基本驗證__

   此 `contentApi.user` 和 `contentApi.password` 驗證可存取WKND GraphQL內容的本機AEM使用者。

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下載 [Android虛擬裝置](https://developer.android.com/studio/run/managing-avds) （最低API 28）。
1. 使用Android模擬器建置和部署應用程式。


### 連線到AEM環境

如果連線到AEM作者環境 [authorization](https://github.com/adobe/aem-headless-client-java#using-authorization) 為必要項。 此 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供使用 [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 若要在中使用權杖型驗證更新使用者端產生器 `AdventureLoader.java` 和 `AdventuresLoader.java`：

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## 程式碼

以下是用來啟動應用程式的重要檔案和程式碼的簡短摘要。 完整的程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 持久查詢

依照AEM Headless最佳實務，iOS應用程式會使用AEM GraphQL持續性查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all` 持久查詢，它會傳回AEM中所有冒險和一組刪節的屬性。 此持續查詢會驅動初始檢視的冒險清單。

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
                _dynamicUrl
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 持久查詢，會依據以下條件傳回單一冒險 `slug` （可唯一識別冒險的自訂屬性）和完整的屬性集。 此持續性查詢可為冒險詳細資料檢視提供支援。

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
          _dynamicUrl
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

### 執行GraphQL持久查詢

AEM持續查詢會透過HTTPGET執行，因此 [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java) 用於對AEM執行持續的GraphQL查詢，並將冒險內容載入應用程式。

每個持續查詢都有對應的「載入器」類別，可非同步呼叫AEM HTTPGET端點，並使用自訂定義的傳回冒險資料 [資料模型](#data-models).

+ `loader/AdventuresLoader.java`

  使用擷取應用程式主畫面上的Adventures清單 `wknd-shared/adventures-all` 持久查詢。

+ `loader/AdventureLoader.java`

  透過以下方式擷取單一冒險選項： `slug` 引數，使用 `wknd-shared/adventure-by-slug` 持久查詢。

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

### GraphQL回應資料模型{#data-models}

`Adventure.java` 是一種Java POJO，會使用GraphQL請求中的JSON資料進行初始化，並為用於Android應用程式檢視中的冒險建模。

### 檢視

Android應用程式使用兩個檢視，在行動體驗中呈現冒險資料。

+ `AdventureListFragment.java`

  叫用 `AdventuresLoader` 並在清單中顯示傳回的冒險。

+ `AdventureDetailFragment.java`

  叫用 `AdventureLoader` 使用 `slug` 引數已透過上冒險選項傳入 `AdventureListFragment` 檢視，並顯示單一冒險的詳細資訊。

### 遠端影像

`loader/RemoteImagesCache.java` 是一個公用程式類別，可協助準備快取中的遠端影像，以便與Android UI元素搭配使用。 冒險內容透過URL參考AEM Assets中的影像，此類別用於顯示該內容。

## 其他資源

+ [AEM Headless快速入門 — GraphQL教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java)
