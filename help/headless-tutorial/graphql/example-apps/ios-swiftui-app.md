---
title: iOS應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此iOS應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS應用程式

範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此iOS應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。

使用AEM Headless的![iOS SwiftUI應用程式](./assets/ios-swiftui-app/ios-app.png)

在GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)上檢視[原始程式碼

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

+ [Xcode](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM需求

iOS應用程式可與下列AEM部署選項搭配使用。 所有部署都需要安裝[WKND網站v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)進行本機設定

iOS應用程式設計來連線至&#x200B;__AEM Publish__&#x200B;環境，不過，如果iOS應用程式的設定中有提供驗證，則可以從AEM Author取得內容。

## 使用方式

1. 複製`adobe/aem-guides-wknd-graphql`存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟[Xcode](https://developer.apple.com/xcode/)並開啟資料夾`ios-app`
1. 修改檔案`Config.xcconfig`並更新`AEM_SCHEME`和`AEM_HOST`，以符合您的目標AEM發佈服務。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   如果連線到AEM Author，請將`AEM_AUTH_TYPE`和支援的驗證屬性新增到`Config.xcconfig`。

   __基本驗證__

   `AEM_USERNAME`和`AEM_PASSWORD`會驗證可存取WKND GraphQL內容的本機AEM使用者。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __權杖驗證__

   `AEM_TOKEN`是[存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)，其會向具有WKND GraphQL內容存取權的AEM使用者進行驗證。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode建立應用程式，並將應用程式部署到iOS模擬器
1. WKND網站中的冒險清單應顯示在應用程式上。 選取冒險會開啟冒險詳細資料。 在冒險清單檢視上，提取以從AEM重新整理資料。

## 程式碼

以下摘要說明如何建立iOS應用程式、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現。 您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)上找到完整程式碼。

### 持久查詢

依照AEM Headless最佳實務，iOS應用程式會使用AEM GraphQL持續性查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all`持續查詢，此查詢會傳回AEM中所有冒險的摘要。 此持續查詢會驅動初始檢視的冒險清單。

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

+ `wknd/adventure-by-slug`持續查詢，會傳回`slug`的單一冒險（唯一識別冒險的自訂屬性）和完整屬性集。 此持續性查詢可為冒險詳細資料檢視提供支援。

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

AEM的持久查詢會透過HTTP GET執行，因此使用HTTP POST的常見GraphQL程式庫（例如Apollo）無法使用。 請改為建立自訂類別，對AEM執行持續查詢HTTP GET請求。

`AEM/Aem.swift`會具現化所有與AEM Headless的互動所使用的`Aem`類別。 模式為：

1. 每個持續查詢都有對應的公用函式(例如 `getAdventures(..)`或`getAdventureBySlug(..)`) iOS應用程式的檢視叫用以取得冒險資料。
1. 公用函式會呼叫私人函式`makeRequest(..)` (該函式會叫用向AEM Headless發出的非同步HTTP GET請求)，並傳回JSON資料。
1. 接著，每個公用函式都會解碼JSON資料，並執行任何必要的檢查或轉換，然後將冒險資料傳回檢視。

   + AEM的GraphQL JSON資料會使用`AEM/Models.swift`中定義的結構/類別來解碼，這些結構/類別對應到傳回我的AEM Headless的JSON物件。

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

`src/AEM/Models.swift`定義了[可解碼的](https://developer.apple.com/documentation/swift/decodable) Swift結構和類別，這些結構和類別對應到AEM JSON回應所傳回的AEM JSON回應。

### 檢視

SwiftUI用於應用程式中的各種檢視。 Apple提供[使用SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)建立清單和導覽的快速入門教學課程。

+ `WKNDAdventuresApp.swift`

  應用程式的專案，並包含其`.onAppear`事件處理常式用來透過`aem.getAdventures()`擷取所有冒險資料的`AdventureListView`。 已在此初始化共用`aem`物件，並以[EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject)的形式公開給其他檢視。

+ `Views/AdventureListView.swift`

  顯示冒險清單（根據`aem.getAdventures()`的資料），並使用`AdventureListItemView`顯示每個冒險的清單專案。

+ `Views/AdventureListItemView.swift`

  顯示Adventures清單(`Views/AdventureListView.swift`)中的每個專案。

+ `Views/AdventureDetailView.swift`

  顯示冒險的詳細資訊，包括標題、說明、價格、活動型別和主要影像。 此檢視會使用`aem.getAdventureBySlug(slug: slug)`查詢AEM以取得完整的冒險詳細資料，其中`slug`引數是根據選取清單列傳入。

### 遠端影像

由冒險內容片段參考的影像，由AEM提供。 此iOS應用程式在GraphQL回應中使用路徑`_dynamicUrl`欄位，並在`AEM_SCHEME`和`AEM_HOST`加上前置詞，以建立完整限定的URL。 如果針對AE SDK開發，`_dynamicUrl`會傳回null，所以對於開發，會退回影像的`_path`欄位。

如果連線到AEM上需要授權的受保護資源，則也必須將憑證新增到影像請求。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI)和[SDWebImage](https://github.com/SDWebImage/SDWebImage)是用來從AEM載入遠端影像，這些影像會在`AdventureListItemView`和`AdventureDetailView`檢視中填入Adventure影像。

`aem`類別（在`AEM/Aem.swift`中）以兩種方式方便使用AEM影像：

1. `aem.imageUrl(path: String)`用於檢視中，以將AEM的配置加在影像的路徑前面，並建立完整限定的URL。

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. `Aem`中的`convenience init(..)`會根據iOS應用程式設定，在影像HTTP要求上設定HTTP授權標頭。

   + 如果已設定&#x200B;__基本驗證__，則會將基本驗證附加到所有影像要求。

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

   + 如果已設定&#x200B;__權杖驗證__，則權杖驗證會附加至所有影像要求。

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

   + 如果未設定&#x200B;__驗證__，則不會將驗證附加到影像要求。

類似的處理方式可用於SwiftUI-native [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage)。 iOS 15.0+支援`AsyncImage`。

## 其他資源

+ [AEM Headless快速入門 — GraphQL教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI清單與導覽教學課程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
