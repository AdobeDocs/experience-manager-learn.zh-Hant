---
title: 管理AEMGraphQL主AEM機
description: 瞭解如何在無AEM頭應用中AEM配置主機。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
source-git-commit: ec2609ed256ebe6cdd7935f3e8d476c1ff53b500
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 1%

---

# 管理主AEM機

部署無AEM頭應用程式需要注AEM意如何構建URL，以確保使用正AEM確的主機/域。 要注意的主URL/請求類型有：

+ HTTP請求到 __[GraphQLAEMAPI](#aem-graphql-api-requests)__
+ __[影像URL](#aem-image-urls)__ 到內容片段中引用的映像資產，並由

通常，無AEM頭應用會與GraphQLAPI和AEM映像請求的單個服務交互。 服AEM務根據無頭應用AEM部署更改：

| 無AEM頭部署類型 | AEM環境 | AEM服務 |
|-------------------------------|:---------------------:|:----------------:|
| 生產 | 生產 | 發佈 |
| 創作預覽 | 生產 | 預覽 |
| 開發 | 開發 | 發佈 |

要處理部署類型排列，每個應用程式部署都使用指定要連接的AEM服務的配置來構建。 然後，AEM使用配置的服務的主機/域來構造AEMGraphQLAPI URL和映像URL。 要確定管理構建相關配置的正確方法，請參考AEMHeadless應用的框架(例如，React、iOS、Android™等)文檔，因為方法因框架而異。

| 客戶端類型 | [單頁應用(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [行動](../mobile.md) | [伺服器到伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM主機配置 | ✔ | ✔ | ✔ | ✔ |

以下是構建URL的可能方法示例 [AEMGraphQL](#aem-graphql-api-requests) 和 [影像請求](#aem-image-requests)，適用於幾種流行的無頭框架和平台。

## AEMGraphQLAPI請求

必須將從無頭應用到AEMGraphQLAPI的HTTPGET請求配置為與正確的AEM服務交互，如 [上](#managing-aem-hosts)。

使用時 [無AEM頭SDK](../../how-to/aem-headless-sdk.md) （適用於基於瀏覽器的JavaScript、基於伺服器的JavaScript和Java™）,AEM主機可以使用服務初始化AEMHeadless客戶端對AEM像以進行連接。

開發自定義AEMHeadless客戶端時，確AEM保服務的主機可基於生成參數進行參數化。

### 範例

以下是GraphQLAPI請AEM求如何使主機值可AEM以針對各種無頭應用程式框架進行配置的示例。

+++ 反應示例

此示例基於 [無AEM頭反應應用](../../example-apps/react-app.md)，說明如何AEM將GraphQLAPI請求配置為根據環境變AEM量連接到不同的服務。

應用應使用 [用AEM於JavaScript的無頭客戶端](../../how-to/aem-headless-sdk.md) 與GraphQLAPIAEM交互。 Headless AEMClient for JavaScript提AEM供的Headless客戶端必須使用其連接的AEM服務主機初始化。

#### 反應環境檔案

反應用 [自定義環境檔案](https://create-react-app.dev/docs/adding-custom-environment-variables/)或 `.env` 檔案，儲存在項目的根中以定義生成特定的值。 例如， `.env.development` 檔案包含開發期間使用的值，而 `.env.production` 包含用於生產生成的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 用於其他用途的檔案 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 後裝 `.env` 和語義描述符，如 `.env.stage` 或 `.env.production`。 不同 `.env` 運行或構建React應用時，可通過設定 `REACT_APP_ENV` 執行前 `npm` 的子菜單。

例如， React應用 `package.json` 可能包含以下內容 `scripts` 配置：

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### 無AEM頭客戶

的 [用AEM於JavaScript的無頭客戶端](../../how-to/aem-headless-sdk.md) 包含AEM向GraphQLAPI發出HTTP請求的無AEM頭客戶端。 Headless客AEM戶端必須使用與其交AEM互的主機初始化，使用活動 `.env` 的子菜單。

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### 反應useEffect(..) 鈎

自定義React useEffect掛接代表呈AEM現視圖的React元件調用與主AEM機初始化的Headless客戶端。

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### 反應組分

自定義useEffect掛接， `useAdventureByPath` 導入，用於使用AEMHeadless客戶端獲取資料，並最終向最終用戶呈現內容。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™示例

此示例基於 [示例AEM無頭iOS™應用](../../example-apps/ios-swiftui-app.md)，說明如AEM何配置GraphQLAPI請求以根據 [特定於生成的配置變數](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)。

iOS™應用需要自AEM定義無頭客戶端與AEMGraphQLAPI交互。 必AEM須編寫無頭客戶端，以便AEM可配置服務主機。

#### 生成配置

XCode配置檔案包含預設配置詳細資訊。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 初始化自定義無AEM頭客戶端

的 [示例無AEM頭iOS應用](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) 使用使用AEM的配置值初始化的自定義無頭客戶端 `AEM_SCHEME` 和 `AEM_HOST`。

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

自定義無AEM頭客戶端(`api/Aem.swift`)包含方法 `makeRequest(..)` 為GraphQLAEMAPI請求添加已配置的前AEM綴 `scheme` 和 `host`。

+ `api/Aem.swift`

```swift
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
    
    return request;
}
```

[可以建立新的生成配置檔案](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 連接到不同AEM的服務。 的特定於生成的值 `AEM_SCHEME` 和 `AEM_HOST` 基於XCode中的選定內部版本使用，導致定制AEM的Headless客戶端與正確的服務AEM連接。

+++

+++ Android™示例

此示例基於 [示例AEM無頭Android™應用](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，說明如何AEM配置GraphQLAPI請求，以根AEM據特定於生成（或風格）的配置變數連接到不同的服務。

Android™應用程式（當用Java™編寫時）應使用 [用AEM於Java™的無頭客戶端](https://github.com/adobe/aem-headless-client-java) 與GraphQLAPIAEM交互。 Headless AEM Client for Java™提AEM供的Headless客戶端必須使用其連接的AEM服務主機初始化。

#### 生成配置檔案

Android™應用定義「productFlavors」，用於生成不同用途的工件。
此示例說明如何定義兩種Android™產品風格，提供不同AEM的服務主機(`AEM_HOST`)開發值(`dev`)和生產(`prod`)使用。

在應用中 `build.gradle` 檔案，新 `flavorDimension` 命名 `env` 的子菜單。

在 `env` 維，二 `productFlavors` 定義： `dev` 和 `prod`。 每個 `productFlavor` 使用 `buildConfigField` 設定定義要連接的服務AEM的生成特定變數。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™載入器

初始化 `AEMHeadlessClient` 生成器，由AEMHeadless Client for Java™提供， `AEM_HOST` 值 `buildConfigField` 的子菜單。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

為不同用途構建Android™應用時，請指定 `env` 並使用相應AEM的主機值。

+++

## 圖AEM像URL

必須將來自無頭應用的映AEM像請求配置為與正確的AEM服務交互，如 [上表](#managing-aem-hosts)。

Adobe建議使用 [優化影像](../../how-to/images.md) 通過 `_dynamicUrl` 的子AEM版本。 的 `_dynamicUrl` 欄位返回一個無主機URL，該URL可以用用於查詢AEMGraphQLAPI的服務主AEM機前置詞。 對於 `_dynamicUrl` GraphQL響應中的欄位如下：

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 範例

以下是影像URL如何為可針對各種無頭AEM應用框架配置的主機值加前置詞的示例。 這些示例假定使用GraphQL查詢，這些查詢使用 `_dynamicUrl` 的子菜單。

例如：

#### GraphQL永續查詢

此GraphQL查詢返回影像引用的 `_dynamicUrl`。 如 [GraphQL響應](#examples-react-graphql-response) 不包括主機。

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL響應

此GraphQL響應返回影像引用的 `_dynamicUrl` 不包括主機。

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ 反應示例

此示例基於 [示例無AEM頭React應用](../../example-apps/react-app.md)，說明如何根據環境變數將映像URL配置AEM為連接到正確的服務。

此示例說明如何為影像引用添加前置詞 `_dynamicUrl` 欄位，可配置 `REACT_APP_AEM_HOST` 反應環境變數。

#### 反應環境檔案

反應用 [自定義環境檔案](https://create-react-app.dev/docs/adding-custom-environment-variables/)或 `.env` 檔案，儲存在項目的根中以定義生成特定的值。 例如， `.env.development` 檔案包含開發期間使用的值，而 `.env.production` 包含用於生產生成的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 用於其他用途的檔案 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 後裝 `.env` 和語義描述符，如 `.env.stage` 或 `.env.production`。 不同 `.env` 運行或生成React應用時，可通過設定 `REACT_APP_ENV` 執行前 `npm` 的子菜單。

例如， React應用 `package.json` 可能包含以下內容 `scripts` 配置：

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### 反應組分

「反應」(React)元件導入 `REACT_APP_AEM_HOST` 環境變數，並為映像添加前置詞 `_dynamicUrl` 值，以提供完全可解析的影像URL。

同樣 `REACT_APP_AEM_HOST` 環境變數用於初始化AEM由 `useAdventureByPath(..)` custom useEffect掛接，用於從中提取GraphQLAEM資料。 使用同一變數將GraphQLAPI請求構造為影像URL，確保React應用與兩個使用案例的同AEM一服務交互。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™示例

此示例基於 [示例AEM無頭iOS™應用](../../example-apps/ios-swiftui-app.md)，說明AEM如何將映像URL配置為根據 [特定於生成的配置變數](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)。

#### 生成配置

XCode配置檔案包含預設配置詳細資訊。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 影像URL生成器

在 `Aem.swift`，自定義無AEM頭客戶端實現，自定義函式 `imageUrl(..)` 採用中提供的影像路徑 `_dynamicUrl` 在GraphQL響應中，並與主持人AEM預先。 然後，每當呈現影像時，在iOS視圖中調用此函式。

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS觀

iOS視圖和前置詞影像 `_dynamicUrl` 值，以提供完全可解析的影像URL。

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[可以建立新的生成配置檔案](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 連接到不同AEM的服務。 的特定於生成的值 `AEM_SCHEME` 和 `AEM_HOST` 基於XCode中的選定內部版本使用，導致定制AEMHeadless客戶端與正確的服務AEM交互。

+++

+++ Android™示例

此示例基於 [示例AEM無頭Android™應用](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，說明了AEM如何根據生成特定（或類型）配置變AEM量將映像URL配置為連接到不同的服務。

#### 生成配置檔案

Android™應用定義「productFlavors」，用於生成不同用途的工件。
此示例說明如何定義兩種Android™產品風格，提供不同AEM的服務主機(`AEM_HOST`)開發值(`dev`)和生產(`prod`)使用。

在應用中 `build.gradle` 檔案，新 `flavorDimension` 命名 `env` 的子菜單。

在 `env` 維，二 `productFlavors` 定義： `dev` 和 `prod`。 每個 `productFlavor` 使用 `buildConfigField` 設定定義要連接的服務AEM的生成特定變數。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### 載入圖AEM像

Android™使用 `ImageGetter` 從中獲取並本地快取映像數AEM據。 在 `prepareDrawableFor(..)` 在活動AEM生成配置中定義的服務主機用於為建立可解析URL的映像路徑添加前置詞AEM。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™視圖

Android™視圖通過 `RemoteImagesCache` 使用 `_dynamicUrl` 價值來自GraphQL。

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

為不同用途構建Android™應用時，請指定 `env` 並使用相應AEM的主機值。

+++
