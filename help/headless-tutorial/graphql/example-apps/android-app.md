---
title: Android應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 提供的Android應用程式示範如何使用AEM的GraphQL API來查詢內容。 Apollo Client Android用於生成GraphQL查詢，並將資料映射到Swift對象，以支援應用程式。 SwiftUI用於呈現內容的簡單清單和詳細視圖。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 2%

---


# Android SwiftUI應用程式

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 提供的Android應用程式示範如何使用AEM的GraphQL API來查詢內容。 此 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 用於執行GraphQL查詢，並將資料對應至Java物件，以便為應用程式提供動力。

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## AEM需求

應用程式的設計目的是連線至AEM **發佈** 環境，與 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安裝。

* [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hant)

建議 [將WKND參考站點部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 使用 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 或 [AEM 6.5 QuickStart Jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 也可使用。

## 如何使用

1. 複製 `aem-guides-wknd-graphql` 存放庫：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 並開啟資料夾 `android-app`
1. 修改檔案 `config.properties` at `app/src/main/assets/config.properties` 和更新 `contentApi.endpoint` 若要符合您的target AEM環境：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下載 [Android虛擬裝置](https://developer.android.com/studio/run/managing-avds) （最低API 28）
1. 使用Android模擬器建立和部署應用程式。


### 連線至AEM環境

`10.0.2.2` 是 [特殊別名](https://developer.android.com/studio/run/emulator-networking) 當使用模擬器時用於localhost。 So `10.0.2.2:4502` 等於 `localhost:4502`. 如果連線至AEM發佈環境（建議），則不需要授權，且 `contentAPi.user` 和 `contentApi.password` 可保留為空白。

如果連線至AEM製作環境 [授權](https://github.com/adobe/aem-headless-client-java#using-authorization) 為必要項目。 預設情況下，應用程式設定為使用具有以下用戶名和密碼的基本身份驗證： `admin:admin`. 此 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供使用 [權杖式驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 若要在中使用以權杖為基礎的驗證更新用戶端產生器，請執行下列動作： `AdventureLoader.java` 和 `AdventuresLoader.java`:

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

以下是用於支援應用程式的重要檔案和代碼的簡要摘要。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 擷取內容

此 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 供應用程式用來對AEM執行GraphQL查詢，並將探險內容載入至應用程式中。

`AdventuresLoader.java` 是在應用程式的主畫面上擷取並載入歷險之初始清單的檔案。 它利用 [持續查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) whis&#39; [預封裝](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) 與WKND參考網站。 端點為 `/wknd/adventures-all`. `AEMHeadlessClientBuilder` 根據中設定的api端點實例化新例項 `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` 是擷取並載入每個詳細檢視之「冒險」內容的檔案。 再次 `AEMHeadlessClient` 用於執行查詢。 一般graphQL查詢會根據探險內容片段的路徑執行。 查詢會保留在名為 [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` 是透過GraphQL請求的JSON資料初始化的POJO。

`RemoteImagesCache.java` 是公用程式類別，可協助您準備快取中的遠端影像，以便與Android UI元素搭配使用。 冒險內容會透過URL參考AEM Assets中的影像，而此類別用於顯示該內容。

### 檢視

`AdventureListFragment.java`  — 呼叫時觸發 `AdventuresLoader` 並在清單中顯示傳回的歷險。

`AdventureDetailFragment.java`  — 初始化 `AdventureLoader` 並顯示單次冒險的詳細資訊。

## 其他資源

* [AEM Headless - GraphQL教學課程快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)

