---
title: iOS應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此iOS應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 1%

---

# iOS應用程式

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此iOS應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。

![iOS SwiftUI應用程式與AEM Headless](./assets/ios-swiftui-app/ios-app.png)

檢視 [GitHub原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM需求

iOS應用程式可搭配下列AEM部署選項使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

iOS應用程式的設計目的，是連線至 __AEM發佈__ 環境，但若iOS應用程式的設定中提供驗證，則可從AEM Author取得內容。

## 如何使用

1. 複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 並開啟資料夾 `ios-app`
1. 修改檔案 `Config.xcconfig` 檔案和更新 `AEM_SCHEME` 和 `AEM_HOST` 以符合您的目標AEM發佈服務。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   如果連線至AEM作者，請新增 `AEM_AUTH_TYPE` 和支援驗證屬性 `Config.xcconfig`.

   __基本驗證__

   此 `AEM_USERNAME` 和 `AEM_PASSWORD` 驗證本機AEM使用者是否可存取WKND GraphQL內容。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Token驗證__

   此 `AEM_TOKEN` 是 [存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 對具有WKND GraphQL內容存取權的AEM使用者進行驗證。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode建立應用程式，並將應用程式部署至iOS模擬器
1. 應用程式中應顯示來自WKND網站的歷險清單。 選擇冒險活動可開啟探險活動的詳細資訊。 在歷險清單檢視中，提取以從AEM重新整理資料。

## 程式碼

以下是如何建置iOS應用程式、如何連線至AEM無周邊以使用GraphQL持續查詢擷取內容，以及如何呈現該資料的摘要。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 持續查詢

依照AEM無周邊最佳實務，iOS應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續的查詢：

+ `wknd/adventures-all` 持續查詢，會傳回AEM中具有一組縮略屬性的所有歷險。 這個持續的查詢會驅動初始檢視的探險清單。

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

+ `wknd/adventure-by-slug` 持續查詢，其返回單個歷程，方法為 `slug` （可唯一識別冒險的自訂屬性），包含完整的屬性集。 這個持續的查詢可支援探險詳細資訊的檢視。

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

### 執行GraphQL持續查詢

AEM持續查詢會透過HTTPGET執行，因此，使用HTTPPOST（例如Apollo）的通用GraphQL程式庫將無法使用。 請改為建立自訂類別，對AEM執行持續的查詢HTTPGET要求。

`AEM/Aem.swift` 實例化 `Aem` 用於與AEM Headless所有互動的類別。 模式是：

1. 每個保存的查詢都具有相應的公用函式(例如 `getAdventures(..)` 或 `getAdventureBySlug(..)`)iOS應用程式的檢視會叫用以取得冒險資料。
1. 公用函式調用一個專用函式 `makeRequest(..)` 會叫用非同步HTTPGET要求至AEM Headless，並傳回JSON資料。
1. 每個公用函式隨後會解碼JSON資料，並執行任何必要的檢查或轉換，然後再將Adventure資料返回到視圖。

   + AEM GraphQL JSON資料會使用 `AEM/Models.swift`，會將對應至JSON物件傳回我的AEM Headless。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

此 `src/AEM/Models.swift` 定義 [可脫離](https://developer.apple.com/documentation/swift/decodable) 對應至AEM JSON回應傳回之AEM JSON回應的Swift結構和類別。

### 檢視

SwiftUI用於應用程式中的各種視圖。 Apple提供的快速入門教學課程 [使用SwiftUI建立清單和導航](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   應用程式的條目，包括 `AdventureListView` who `.onAppear` 事件處理常式可透過 `aem.getAdventures()`. 共用 `aem` 物件在此初始化，並以 [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   顯示歷險清單(根據 `aem.getAdventures()`)，並使用 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   顯示歷險清單中的每個項目(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

   顯示冒險的詳細資訊，包括標題、說明、價格、活動類型和主要影像。 此檢視會查詢AEM，以取得完整的冒險詳細資訊，使用 `aem.getAdventureBySlug(slug: slug)`，其中 `slug` 參數會根據選取清單列傳入。

### 遠端影像

由冒險內容片段參考的影像由AEM提供。 此iOS應用程式使用路徑 `_path` 欄位（在GraphQL回應中），並將 `AEM_SCHEME` 和 `AEM_HOST` 來建立完全限定的URL。

如果在AEM上連線至需要授權的受保護資源，也必須將憑證新增至影像要求。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [SDWebImage](https://github.com/SDWebImage/SDWebImage) 會用來從AEM載入遠端影像，以填入冒險影像於 `AdventureListItemView` 和 `AdventureDetailView` 檢視。

此 `aem` 類(在 `AEM/Aem.swift`)有助於透過兩種方式使用AEM影像：

1. `aem.imageUrl(path: String)` 用於檢視中，可在AEM配置的前端加上附加元件，並托管至影像的路徑，借此建立完全限定的URL。

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. 此 `convenience init(..)` in `Aem` 根據iOS應用程式設定，在影像HTTP要求上設定HTTP授權標題。

   + 若 __基本驗證__ 設定後，基本驗證會附加至所有影像要求。

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

   + 若 __權杖驗證__ 設定後，代號驗證會附加至所有影像要求。

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

   + 若 __無驗證__ 設定，則不會將驗證附加至影像要求。



SwiftUI-native可使用類似的方法 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` iOS 15.0+支援。

## 其他資源

+ [AEM Headless - GraphQL教學課程快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI清單和導航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
