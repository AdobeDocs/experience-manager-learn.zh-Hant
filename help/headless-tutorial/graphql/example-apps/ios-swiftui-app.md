---
title: iOS應用 — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此iOS應用程式演示了如何使用AEMGraphQL API使用永續查詢來查詢內容。
version: Cloud Service
mini-toc-levels: 2
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 8b2c116ceb6ab8c3a009dcec6629c2e97d815b7b
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 0%

---

# iOS應用

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此iOS應用程式演示了如何使用AEMGraphQL API使用永續查詢來查詢內容。

![iOSSwiftUI應AEM用](./assets/ios-swiftui-app/ios-app.png)

查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app)

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (需要macOS)
+ [蠢貨](https://git-scm.com/)

## AEM要求

iOS應用程式可與以下部AEM署選項配合使用。 所有部署都需要 [WKND站點v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝。

+ [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本地設定使用 [AEM Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM6.5 SP13+快速啟動](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

iOS應用程式設計為連接到 __AEM發佈__ 但是，如果在iOS應用程式的配置中提供了身份驗證，則它可以從AEM Author中源出內容。

## 如何使用

1. 克隆 `adobe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 啟動 [Xcode](https://developer.apple.com/xcode/) 開啟資料夾 `ios-swiftui-app`
1. 修改檔案 `Config.xcconfig` 檔案和更新 `AEM_SCHEME` 和 `AEM_HOST` 匹配目標AEM發佈服務。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   如果連接到AEM作者，請添加 `AEM_AUTH_TYPE` 並支援驗證屬性 `Config.xcconfig`。

   __基本身份驗證__

   的 `AEM_USERNAME` 和 `AEM_PASSWORD` 驗證本AEM地用戶對WKND GraphQL內容的訪問權限。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __令牌驗證__

   的 `AEM_TOKEN` 是 [訪問令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 對具有WKND GraphQL內AEM容訪問權限的用戶進行身份驗證。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode構建應用程式並將應用程式部署到iOS模擬器
1. WKND站點的冒險清單應顯示在應用程式上。 選擇冒險可開啟探險細節。 在冒險清單視圖中，從中獲取以刷新數AEM據。

## 代碼

以下是iOS應用程式構建方式、它如何連接到AEMHeadless以使用GraphQL永續查詢檢索內容以及如何顯示該資料的摘要。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app)。

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

通AEM過HTTPGET執行永續查詢，因此不能使用使用HTTPPOST（如Apollo）的常用GraphQL庫。 而是建立一個自定義類，該類執行對的永續查詢HTTPGET請AEM求。

`AEM/Aem.swift` 實例化 `Aem` 類，用於與Headless的所AEM有交互。 模式是：

1. 每個永續查詢都具有相應的公用函式(例如 `getAdventures(..)` 或 `getAdventureBySlug(..)`)調用iOS應用程式的視圖以獲取冒險資料。
1. 公共函式調用私有函式 `makeRequest(..)` 調用非同步HTTPGET請求AEM到Headless並返回JSON資料。
1. 然後，每個公共函式對JSON資料進行解碼，並執行任何所需的檢查或轉換，然後才將Adventure資料返回到視圖。

+ 使AEM用中定義的結構/類對GraphQL JSON資料進行解碼 `AEM/Models.swift`，映射到JSON對象時返回了AEM我的Headless。

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

### GraphQL響應資料模型

iOS喜歡將JSON對象映射到類型化的資料模型。

的 `src/AEM/Models.swift` 定義 [脫](https://developer.apple.com/documentation/swift/decodable) 映射到JSON響應返回AEM的JSON響應的SwiftAEM結構和類。

### 檢視

SwiftUI用於應用程式中的各種視圖。 Apple提供入門教程 [使用SwiftUI生成清單和導航](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)。

+ `WKNDAdventuresApp.swift`

   應用程式的輸入，包括 `AdventureListView` 誰 `.onAppear` 事件處理程式通過 `aem.getAdventures()`。 共用 `aem` 對象在此處初始化，並作為 [環境對象](https://developer.apple.com/documentation/swiftui/environmentobject)。

+ `Views/AdventureListView.swift`

   顯示冒險清單(基於來自 `aem.getAdventures()`)，並使用 `AdventureListItemView`。

+ `Views/AdventureListItemView.swift`

   顯示冒險清單中的每個項(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

   顯示冒險的詳細資訊，包括標題、說明、價格、活動類型和主映像。 此視圖使用AEM查詢完整的冒險詳細資訊 `aem.getAdventureBySlug(slug: slug)`的子菜單。 `slug` 參數根據選擇清單行傳入。

### 遠程映像

由提供冒險內容片段引用的圖AEM像。 此iOS應用使用路徑 `_path` GraphQL響應中的欄位，並為 `AEM_SCHEME` 和 `AEM_HOST` 的子菜單。

如果連接到需要授權AEM的受保護資源，則還必須將憑據添加到映像請求中。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [SDWebImage](https://github.com/SDWebImage/SDWebImage) 用於從中載入填充AdventureAEM映像的遠程映像 `AdventureListItemView` 和 `AdventureDetailView` 的子菜單。

的 `aem` 類(在 `AEM/Aem.swift`)以兩種方AEM式方便使用影像：

1. `aem.imageUrl(path: String)` 在視圖中用於預AEM定方案和主機到映像路徑，從而建立完全限定的URL。

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. 的 `convenience init(..)` 在 `Aem` 根據iOS應用程式配置在映像HTTP請求上設定HTTP授權標頭。

   + 如果 __基本認證__ 配置，然後將基本身份驗證附加到所有映像請求。

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

   + 如果 __令牌驗證__ 配置，然後將令牌身份驗證附加到所有映像請求。

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

   + 如果 __無驗證__ 配置，則不會將身份驗證附加到映像請求。



類似的方法可與SwiftUI-native一起使用 [非同步映像](https://developer.apple.com/documentation/swiftui/asyncimage)。 `AsyncImage` 在iOS15.0+上支援。

## 其他資源

+ [無頭入門AEM- GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI清單和導航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
