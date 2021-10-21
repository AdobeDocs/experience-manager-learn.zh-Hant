---
title: iOS SwiftUI App - AEM Headless Example
description: Example applications are a great way to explore the headless capabilities of Adobe Experience Manager (AEM). 提供的iOS應用程式示範如何使用AEM的GraphQL API來查詢內容。 Apollo Client iOS用於生成GraphQL查詢，並將資料映射到Swift對象，以支援應用程式。 SwiftUI用於呈現內容的簡單清單和詳細視圖。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 1%

---


# iOS SwiftUI應用

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此iOS應用程式示範如何使用AEM的GraphQL API來查詢內容。 Apollo Client iOS用於生成GraphQL查詢，並將資料映射到Swift對象，以支援應用程式。 SwiftUI用於呈現內容的簡單清單和詳細視圖。

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## AEM需求

應用程式的設計目的是連線至AEM **發佈** 環境，與 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安裝。

* [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hant)

建議 [將WKND參考站點部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 使用 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 或 [AEM 6.5 QuickStart Jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 也可使用。

## How to use

1. Clone the `aem-guides-wknd-graphql` repository:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) and open the folder `ios-swiftui-app`
1. 修改檔案 `Config.xcconfig` 檔案和更新 `AEM_HOST` 以符合您的目標AEM發佈環境

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Build the application using Xcode and deploy the app to iOS simulator
1. 應用程式中應顯示來自WKND參考網站的歷險清單。

## 程式碼

Below is a brief summary of the important files and code used to power the application. 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### 阿波羅·iOS

The [Apollo iOS](https://www.apollographql.com/docs/ios/) client is used by the app to execute the GraphQL query against AEM. 官員 [阿波羅教學課程](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) 有關如何安裝和使用的詳細資訊。

`schema.json` 是一個檔案，它表示安裝了WKND參考站點的AEM環境中的GraphQL架構。 `schema.json` 從AEM下載，並新增至專案。 Apollo客戶檢查副檔名為的檔案 `.graphql` 作為自訂建置階段的一部分。 Apollo客戶接著使用 `schema.json` 檔案和任何 `.graphql` 查詢以自動生成檔案 `API.swift`.

這為應用程式提供了強類型模型，用於執行查詢和表示結果的模型。

![Xcode custom build phase](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` 包含用於查詢歷險的查詢：

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` 構造 `ApolloClient`. The `endpointURL` used is constructed by reading the values of the `Config.xcconfig` file. If you wanted to connect to an AEM **Author** instance and needed to add additional headers for authentication, you would want to modify the `ApolloClient` here.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### 冒險資料

The application is designed to display a list of Adventures and then a detail view of each adventure.

`AdventuresDataModel.swift` 是包含函式的類 `fetchAdventures()`. 此函式會使用 `ApolloClient` ，以執行查詢。 On a successful query the results array will be of type `AdventureListQuery.Data.AdventureList.Item`, auto-generated by the `API.swift` file.

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

It is possible to use `AdventureListQuery.Data.AdventureList.Item` directly to power the application. 但是，很可能有些資料不完整，因此某些屬性可能為Null。

`Adventure.swift` 是引進的自訂模型，可作為阿波羅產生之模型的包裝函式。 `Adventure` 初始化為 `AdventureListQuery.Data.AdventureList.Item`. A `typealias` 可縮短，讓程式碼更易讀：

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

此 `Adventure` 使用 `AdventureData` 物件：

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

這可讓我們提供預設值，並對不完整的資料執行其他檢查。 然後，我們可以使用 `Adventure` 模型可安全地支援各種UI元素，且不需要持續檢查null值。

In AEM, pieces of content are uniquely identified by `_path`. In `Adventure.swift` we populate the `id` property with the value of `_path`. 這允許 `Adventure` 實作 `Identifiable` 介面，可讓您更輕鬆地迭代運算陣列或清單。

### 檢視

SwiftUI用於應用程式中的各種視圖。 適用於 [建立清單和導覽](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) 可在Apple的開發人員網站上找到。 此應用程式的程式碼是鬆散衍生的。

`WKNDAdventuresApp.swift` 是應用程式的項目。 包括 `AdventureListView` 和 `.onAppear` 事件可用來擷取冒險資料。

`AdventureListView.swift`  — 建立 `NavigationView` 和由 `AdventureRowView`. 導覽至 `AdventureDetailView` 已設定於此。

`AdventureRowView`  — 連續顯示「冒險」的主影像和「冒險」標題。

`AdventureDetailView`  — 顯示個別冒險的完整詳細資訊，包括標題、說明、價格、活動類型和主要影像。

When the Apollo CLI runs and re-generates `API.swift` it causes the preview to stop. 若要使用「自動預覽」功能，您必須更新 **阿波羅CLI** 建立階段並檢查以執行指令碼 **僅用於安裝組建**.

![僅檢查是否安裝組建](assets/ios-swiftui-app/update-build-phases.png)

### 遠端影像

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) and [SDWEbImage](https://github.com/SDWebImage/SDWebImage) are used to load the remote images from AEM that populate the Adventure primary image on the Row and Detail views.

此 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) 是本機的SwiftUI視圖，也可使用。 `AsyncImage` 僅支援iOS 15.0+。

## 其他資源

* [AEM Headless - GraphQL教學課程快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [SwiftUI清單和導航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Apollo iOS用戶端教學課程](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

