---
title: 使用帶頭的圖AEM像
description: 瞭解如何請求影像內容引用URL，以及將自定義格式副本與「AEM無頭」一起使用。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---

# 無頭圖AEM像

影像是 [發展豐富而富有吸引力AEM的無頭體驗](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)。 無AEM頭支援映像資產的管理及其優化交付。

用於無頭內容建AEM模的內容片段，通常引用影像資源，以便在無頭體驗中顯示。 可AEM以編寫GraphQL查詢，以根據引用影像的位置為影像提供URL。

的 `ImageRef` 類型具有三個內容引用的URL選項：

+ `_path` 是中引用的路AEM徑，且不包AEM括源（主機名）
+ `_authorUrl` 是AEM作者上影像資產的完整URL
   + [AEM作者](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用於提供無頭應用程式的預覽體驗。
+ `_publishUrl` 是AEM發佈上影像資產的完整URL
   + [AEM發佈](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是無頭應用程式的生產部署顯示映像的位置。

根據以下條件，最好使用這些欄位：

| ImageRef欄位 | 客戶端Web應用從AEM | 客戶端應用查詢AEM作者 | 客戶端應用查詢AEM發佈 |
|--------------------|:------------------------------:|:-----------------------------:|:------------------------------:|
| `_path` | ✔ | ✔（應用必須在URL中指定主機） | ✔（應用必須在URL中指定主機） |
| `_authorUrl` | ✘ | ✔ | ✘ |
| `_publishUrl` | ✘ | ✘ | ✔ |

使用 `_authorUrl` 和 `_publishUrl` 應與用AEM於源GraphQL響應的GraphQL終結點對齊。

## 內容片段模型

確保包含影像引用的「內容片段」欄位是 __內容引用__ 資料類型。

在 [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，通過選擇欄位並檢查 __屬性__ 的上界。

![內容引用影像的內容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL永續查詢

在GraphQL查詢中，將欄位返回為 `ImageRef` 類型，並請求相應的欄位 `_path`。 `_authorUrl`或 `_publishUrl` 應用程式所需的。 例如，查詢 [WKND參考演示項目](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) 並在其中包括影像資產引用的影像URL `primaryImage` 欄位，可以使用新的永續查詢 `wknd-shared/adventure-image-by-path` 定義為：

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

的 `$path` 在 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

## GraphQL響應

生成的JSON響應包含包含映像資產的URls的請求欄位。

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_authorUrl": "https://author-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_publishUrl": "https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"
        }
      }
    }
  }
}
```

要在應用程式中載入引用的映像，請使用相應的欄位， `_path`。 `_authorUrl`或 `_publishUrl` 的 `adventurePrimaryImage` 作為影像的源URL。

的域 `_authorUrl` 和 `_publishUrl` 由AEMas a Cloud Service使用 [外部化器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/externalizer.html)。

在React中，顯示AEM Publish中的影像如下所示：

```html
<img src={ data.adventureByPath.item.primaryImage._publishUrl } />
```

## 影像格式副本

映像資產支援可定製 [格式](../../../assets/authoring/renditions.md)，是原始資產的替代表。 自定義格式副本有助於優化無頭體驗。 與請求原始影像資產（通常是大型高解析度檔案）不同，優化的格式副本可由無頭應用程式請求。

### 建立格式副本

AEM Assets管理員使用「處理配置檔案」定義自定義格式副本。 然後，「處理配置檔案」可以直接應用於特定資料夾樹或資產，以便為這些資產生成格式副本。

#### 處理設定檔

資產格式副本規範在中定義 [處理配置檔案](../../../assets/configuring//processing-profiles.md) 由AEM Assets管理人負責。

建立或更新「處理配置檔案」，並添加無頭應用程式所需的影像大小的格式副本定義。 格式副本可以命名為任何內容，但應在語義上命名。

![無AEM頭優化格式副本](./assets/images/processing-profiles.jpg)

在此示例中，將建立三個格式副本：

| 格式副本名稱 | 副檔名 | 最大寬度 |
|----------------|:---------:|----------:|
| 大 | jpeg | 1200像素 |
| 中 | jpeg | 900像素 |
| 小 | jpeg | 600像素 |

上表中調用的屬性非常重要：

+ __格式副本名稱__ 用於請求格式副本。
+ __擴展__ 是用於請求 __格式副本名稱__。
+ __最大寬度__ 用於根據在無頭應用程式中的使用情況通知開發人員應該使用哪些格式副本。

格式副本定義取決於無頭應用程式的需要，因此請確保為使用案例定義最佳格式副本集，並在使用方式方面以語義命名。

#### 重新處理資產{#reprocess-assets}

建立（或更新）「處理配置檔案」後，重新處理資產以生成在「處理配置檔案」中定義的新格式副本。 在使用處理配置檔案處理資產之前，將不存在新格式副本。

+ 最好， [已將「處理配置檔案」分配給資料夾](../../../assets/configuring//processing-profiles.md) 因此，任何新資產都會自動生成格式副本。 現有資產必須採用下面的臨時辦法重新處理。

+ 或者，通過選擇資料夾或資產，選擇 __重新處理資產__，然後選擇新的「處理配置檔案」名稱。

   ![臨時重新處理資產](./assets/images/ad-hoc-reprocess-assets.jpg)

#### 查看格式副本

格式副本可通過 [開啟資產的格式副本視圖](../../../assets/authoring/renditions.md)，並在格式副本欄中選擇新格式副本以進行預覽。 如果缺少格式副本， [確保使用處理配置檔案處理資產](#reprocess-assets)。

![查看格式副本](./assets/images/review-renditions.jpg)

#### 發佈資產

確保具有新格式副本的資產 [（重新發佈）](../../../assets/sharing/publish.md) 因此，新格式副本可在AEM Publish（AEM發佈）上訪問。

### 訪問格式副本

通過附加 __格式副本名稱__ 和 __格式副本擴展__ 在「處理配置檔案」中定義到資產的URL。

| 資產網址 | 格式副本子路徑 | 格式副本名稱 | 格式副本擴展 |  | 格式副本URL |
|-----------|:------------------:|:--------------:|--------------------:|:--:|---|
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr內容/格式副本/ | 大 | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/large.jpeg |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr內容/格式副本/ | 中 | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/medium.jpeg |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr內容/格式副本/ | 小 | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/small.jpeg |

{style=&quot;table-layout:auto&quot;}

### GraphQL查詢{#renditions-graphl-query}

AEMGraphQL確實需要額外語法來請求影像格式副本。 但 [影像查詢](#images-graphql-query) 以常規方式，並以代碼形式指定所需的格式副本。 重要的是 [確保無頭應用程式使用的影像資產具有同名格式副本](#reprocess-assets)。

### 反應示例

讓我們建立一個簡單的「反應」應用程式，該應用程式顯示單個影像資產的三個格式副本：小、中和大。

![影像資產格式副本反應示例](./assets/images/react-example-renditions.jpg)

#### 建立影像元件{#react-example-image-component}

建立呈現影像的「反應」元件。 此元件接受四個屬性：

+ `assetUrl`:通過GraphQL查詢的響應提供的影像資產URL。
+ `renditionName`:要載入的格式副本的名稱。
+ `renditionExtension`:要載入的格式副本的擴展。
+ `alt`:影像的Alt文本；輔助功能非常重要！

此元件構建 [格式副本URL，使用中概述的格式 __訪問格式副本__](#access-renditions)。 安 `onError` 處理程式設定為在格式副本丟失時顯示原始資產。

此示例使用原始資產url作為 `onError` 處理程式，在該事件中缺少格式副本。

```javascript
// src/Image.js

export default function Image({ assetUrl, renditionName, renditionExtension, alt }) {
  // Construct the rendition Url in the format:
  //   <ASSET URL>/_jcr_content/renditions<RENDITION NAME>.<RENDITION EXTENSION>
  const renditionUrl = `${assetUrl}/_jcr_content/renditions/${renditionName}.${renditionExtension}`;

  // Load the original image asset in the event the named rendition is missing
  const handleOnError = (e) => { e.target.src = assetUrl; }

  return (
    <>
      <img src={renditionUrl} 
            alt={alt} 
            onError={handleOnError}/>
    </>
  );
}
```

#### 定義 `App.js`{#app-js}

這個簡單 `App.js` 查AEM詢冒險影像，然後顯示該影像的三個格式副本：小、中和大。

在自定AEM義React掛接中執行查詢 [使用無頭SDKAEM的AdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries)。

查詢結果和特定格式副本參數將傳遞給 [影像反應元件](#react-example-image-component)。

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'
import Image from "./Image";

function App() {

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  let { data, error } = useAdventureByPath("/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp");

  // Wait for GraphQL to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h2>Small rendition</h2>
      {/* Render the small rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="small"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Medium rendition</h2>
      {/* Render the medium rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="medium"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Large rendition</h2>
      {/* Render the large rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="large"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.title}
      />
    </div>
  );
}

export default App;
```
