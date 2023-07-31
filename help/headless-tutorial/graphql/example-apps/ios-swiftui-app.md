---
title: iOS應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此iOS應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 3%

---

# iOS應用程式

範例應用程式是探索Adobe Experience Manager (AEM)無周邊功能的絕佳方式。 此iOS應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。

![iOS SwiftUI應用程式搭配AEM Headless](./assets/ios-swiftui-app/ios-app.png)

檢視 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 必備條件 {#prerequisites}

下列工具應在本機安裝：

+ [Xcode](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM需求

iOS應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即將安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用進行本機設定 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)

iOS應用程式是專為連線至 __AEM發佈__ 不過，如果在iOS應用程式的設定中提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 並開啟資料夾 `ios-app`
1. 修改檔案 `Config.xcconfig` 檔案和更新 `AEM_SCHEME` 和 `AEM_HOST` 以符合您的目標AEM Publish服務。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   如果連線到AEM作者，請新增 `AEM_AUTH_TYPE` 和支援的驗證屬性 `Config.xcconfig`.

   __基本驗證__

   此 `AEM_USERNAME` 和 `AEM_PASSWORD` 驗證可存取WKND GraphQL內容的本機AEM使用者。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __權杖驗證__

   此 `AEM_TOKEN` 是 [存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 會向可存取WKND GraphQL內容的AEM使用者進行驗證。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode建立應用程式，並將應用程式部署到iOS模擬器
1. WKND網站中的冒險清單應顯示在應用程式上。 選取冒險會開啟冒險詳細資料。 在冒險清單檢視上，提取以從AEM重新整理資料。

## 程式碼

以下摘要說明如何建立iOS應用程式、其如何連線至AEM Headless以使用GraphQL持續查詢來擷取內容，以及資料如何呈現。 完整的程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 持久查詢

依照AEM Headless最佳實務，iOS應用程式會使用AEM GraphQL持續性查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all` 持久查詢，它會傳回AEM中所有冒險和一組刪節的屬性。 此持續查詢會驅動初始檢視的冒險清單。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` 持久查詢，會依據以下條件傳回單一冒險 `slug` （可唯一識別冒險的自訂屬性）和完整的屬性集。 此持續性查詢可為冒險詳細資料檢視提供支援。

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

AEM持續查詢會透過HTTPGET執行，因此使用HTTPPOST的常見GraphQL程式庫（例如Apollo）無法使用。 請改為建立自訂類別，對AEM執行持續查詢HTTPGET請求。

`AEM/Aem.swift` 例項化 `Aem` 用於與AEM Headless的所有互動的類別。 模式為：

1. 每個持續查詢都有對應的公用函式(例如 `getAdventures(..)` 或 `getAdventureBySlug(..)`)叫用iOS應用程式的檢視來取得冒險資料。
1. 公用函式呼叫私人函式 `makeRequest(..)` 它會叫用對AEM Headless的非同步HTTPGET請求，並傳回JSON資料。
1. 接著，每個公用函式都會解碼JSON資料，並執行任何必要的檢查或轉換，然後將冒險資料傳回檢視。

   + AEM GraphQL JSON資料會使用中定義的結構/類別來解碼 `AEM/Models.swift`，對應到傳回我的AEM Headless的JSON物件。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL回應資料模型

iOS偏好將JSON物件對應至輸入的資料模型。

此 `src/AEM/Models.swift` 定義 [可解碼](https://developer.apple.com/documentation/swift/decodable) Swift結構和類別會對應至AEM JSON回應傳回的AEM JSON回應。

### 檢視

SwiftUI用於應用程式中的各種檢視。 Apple提供快速入門教學課程，適用於 [使用SwiftUI建立清單和導覽](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  應用程式的專案，包括 `AdventureListView` 其 `.onAppear` 事件處理常式是用來透過擷取所有冒險資料 `aem.getAdventures()`. 共用的 `aem` 物件在這裡初始化，並公開給其他檢視做為 [環境物件](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  顯示冒險清單(根據來自 `aem.getAdventures()`)和顯示每個冒險的清單專案，使用 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  顯示冒險清單中的每個專案(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

  顯示冒險的詳細資訊，包括標題、說明、價格、活動型別和主要影像。 此檢視會查詢AEM以取得完整的冒險詳細資訊，使用 `aem.getAdventureBySlug(slug: slug)`，其中 `slug` 會根據選取清單列傳入引數。

### 遠端影像

冒險內容片段參考的影像由AEM提供。 此iOS應用程式使用路徑 `_dynamicUrl` GraphQL欄位中輸入值，並在前面加上 `AEM_SCHEME` 和 `AEM_HOST` 以建立完整限定的URL。 如果針對AEM SDK開發， `_dynamicUrl` 傳回null，因此對於開發，會遞補為影像的 `_path` 欄位。

如果連線到AEM上需要授權的受保護資源，則也必須將憑證新增到影像請求。

[SdwebimageswiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [Sdwebimage](https://github.com/SDWebImage/SDWebImage) 是用來從AEM載入遠端影像，並填入 `AdventureListItemView` 和 `AdventureDetailView` 檢視。

此 `aem` 類別(在 `AEM/Aem.swift`)以兩種方式加速使用AEM影像：

1. `aem.imageUrl(path: String)` 在檢視中使用，在AEM配置前面加上，並託管到影像的路徑，以建立完全限定的URL。

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. 此 `convenience init(..)` 在 `Aem` 根據iOS應用程式設定，在影像HTTP請求上設定HTTP授權標頭。

   + 如果 __基本驗證__ 設定，然後將基本驗證附加到所有影像要求。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + 如果 __權杖驗證__ 完成設定後，權杖驗證就會附加至所有影像要求。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + 如果 __無驗證__ 設定，則不會將驗證附加到影像要求。

類似的方法可用於SwiftUI原生 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` 在iOS 15.0+上支援。

## 其他資源

+ [AEM Headless快速入門 — GraphQL教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI清單與導覽教學課程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
