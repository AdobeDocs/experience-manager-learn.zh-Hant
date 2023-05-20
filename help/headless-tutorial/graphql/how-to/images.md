---
title: 使用帶無頭的優化AEM影像
description: 瞭解如何使用無頭功能請求優化的AEM影像URL。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 97a311e043d3903070cd249d993036b5d88a21dd
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 5%

---

# 使用無頭優AEM化影像 {#images-with-aem-headless}

影像是 [發展豐富而富有吸引力AEM的無頭體驗](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)。 無AEM頭支援映像資產的管理及其優化交付。

用於無頭內容建AEM模的內容片段，通常引用影像資源，以便在無頭體驗中顯示。 可以AEM編寫GraphQL查詢，以根據引用影像的位置為影像提供URL。

的 `ImageRef` 類型有四個內容引用的URL選項：

+ `_path` 是中引用的路AEM徑，且不包AEM括源（主機名）
+ `_dynamicUrl` 是首選Web優化映像資產的完整URL。
   + 的 `_dynamicUrl` 不包含源AEM，因此域（AEM作者或AEM發佈服務）必須由客戶端應用程式提供。
+ `_authorUrl` 是AEM作者上影像資產的完整URL
   + [AEM作者](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用於提供無頭應用程式的預覽體驗。
+ `_publishUrl` 是AEM發佈上影像資產的完整URL
   + [AEM發佈](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是無頭應用程式的生產部署顯示映像的位置。

的 `_dynamicUrl` 是用於影像資產的首選URL，應替代 `_path`。 `_authorUrl`, `_publishUrl` 盡可能。

|  | AEM as a Cloud Service  | AEMas a Cloud Service | SDKAEM | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 是否支援Web優化映像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="影像與 AEM Headless"
>abstract="了解 AEM Headless 如何支援影像資產的管理及其最佳化傳遞。"

## 內容片段模型

確保包含影像引用的「內容片段」欄位是 __內容引用__ 資料類型。

在 [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，通過選擇欄位並檢查 __屬性__ 的上界。

![內容引用影像的內容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL永續查詢

在GraphQL查詢中，將欄位返回為 `ImageRef` 類型，並請求 `_dynamicUrl` 的子菜單。 例如，查詢 [WKND站點項目](https://github.com/adobe/aem-guides-wknd) 並在其中包括影像資產引用的影像URL `primaryImage` 欄位，可以使用新的永續查詢 `wknd-shared/adventure-image-by-path` 定義為：

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### 查詢變數

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

的 `$path` 在 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

的 `_assetTransform` 定義 `_dynamicUrl` 構造為優化所提供的影像再現。 還可以通過更改URL的查詢參數在客戶端上調整Web優化影像URL。

| GraphQL參數 | URL 參數 | 說明 | 必要 | GraphQL變數值 | URL參數值 | 示例URL參數 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/A | 影像資源的格式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/A | N/A |
| `seoName` | N/A | URL中檔案段的名稱。 如果未提供，則使用影像資產名稱。 | ✘ | 字母數字， `-`或 `_` | N/A | N/A |
| `crop` | `crop` | 從影像中提取的裁剪幀必須在影像的大小內 | ✘ | 在原始影像維的邊界內定義裁剪區域的正整數 | 數字坐標的逗號分隔字串 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 輸出影像（高度和寬度）的大小（以像素為單位）。 | ✘ | 正整數 | 按順序以逗號分隔的正整數 `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | 影像的旋轉（度）。 | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | 翻轉影像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | 影像質量（原始質量的百分比）。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 輸出影像的寬度（以像素為單位）。 當 `size` 提供 `width` 忽略。 | ✘ | 正整數 | 正整數 | `?width=1600` |
| `preferWebP` | `preferwebp` | 如果 `true` 並AEM在瀏覽器支援的情況下為WebP提供服務 `format`。 | ✘ | `true`、`false` | `true`、`false` | `?preferwebp=true` |

## GraphQL響應

生成的JSON響應包含請求的欄位，其中包含到影像資產的Web優化URL。

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

要載入應用程式中引用的映像的Web優化映像，請使用 `_dynamicUrl` 的 `primaryImage` 作為影像的源URL。

在React中，顯示AEM Publish中的Web優化影像如下所示：

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

記住， `_dynamicUrl` 不包括域AEM，因此必須提供要解析的影像URL的所需原點。

## 響應URL

上例顯示使用單一大小的影像，但是在Web體驗中，通常需要響應影像集。 可使用 [img srsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 或 [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)。 以下代碼段顯示如何使用 `_dynamicUrl` 作為基礎，並附加不同的寬度參數，以為不同的響應視圖提供動力。 不僅 `width` 使用查詢參數，但客戶端可以添加其他查詢參數，根據其需要進一步優化影像資產。

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## 反應示例

讓我們建立一個簡單的React應用程式，在後續顯示Web優化影像 [響應影像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/)。 響應影像有兩種主要模式：

+ [帶srcset的IMG元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 提高效能
+ [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 設計控制

### 帶srcset的IMG元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[帶曲線集的IMG元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 與 `sizes` 屬性，以針對不同的螢幕大小提供不同的影像資產。 當為不同的螢幕大小提供不同的影像資產時，IMG srcsets非常有用。

### 圖片元素

[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 與多個 `source` 元素，以針對不同螢幕大小提供不同的影像資源。 當為不同螢幕大小提供不同的影像呈現時，圖片元素非常有用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 示例代碼

此簡單的React應用使用 [無AEM頭SDK](./aem-headless-sdk.md) 查詢AEMAdventure內容的無頭API，並使用 [帶srcset的img元素](#img-element-with-srcset) 和 [畫素](#picture-element)。 的 `srcset` 和 `sources` 使用自定義 `setParams` 函式，將web優化的傳遞查詢參數附加到 `_dynamicUrl` 因此，根據Web客戶端的需要更改所傳送的影像格式副本。

在自定AEM義React掛接中執行查詢 [使用無頭SDKAEM的AdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries)。

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
