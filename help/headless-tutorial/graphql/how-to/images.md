---
title: 搭配AEM Headless使用最佳化影像
description: 了解如何使用AEM Headless要求最佳化的影像URL。
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

# 使用AEM Headless最佳化影像 {#images-with-aem-headless}

影像是 [開發豐富、引人入勝的AEM無頭體驗](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html). AEM Headless支援影像資產的管理及其最佳化傳送。

AEM無頭內容模型中使用的內容片段，通常會參考用於無頭體驗中顯示的影像資產。 AEM GraphQL查詢可以撰寫，以根據影像參考的位置，提供影像的URL。

此 `ImageRef` 類型有四個內容參考的URL選項：

+ `_path` 是AEM中的參考路徑，且不包含AEM來源（主機名稱）
+ `_dynamicUrl` 是偏好網頁最佳化影像資產的完整URL。
   + 此 `_dynamicUrl` 不包含AEM來源，因此網域（AEM作者或AEM發佈服務）必須由用戶端應用程式提供。
+ `_authorUrl` 是AEM Author上影像資產的完整URL
   + [AEM作者](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用來提供無頭應用程式的預覽體驗。
+ `_publishUrl` 是AEM發佈上影像資產的完整URL
   + [AEM發佈](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是無頭應用程式的生產部署顯示影像的位置。

此 `_dynamicUrl` 是用於影像資產的偏好URL，應取代的使用 `_path`, `_authorUrl`，和 `_publishUrl` 盡可能。

|  | AEM as a Cloud Service  | AEMas a Cloud ServiceRDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 是否支援Web優化的映像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="影像與 AEM Headless"
>abstract="了解 AEM Headless 如何支援影像資產的管理及其最佳化傳遞。"

## 內容片段模型

確保包含影像參考的「內容片段」欄位為 __內容參考__ 資料類型。

欄位類型會在 [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，方法是選取欄位並檢查 __屬性__ 標籤。

![內容參考影像的內容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL持續查詢

在GraphQL查詢中，將欄位傳回為 `ImageRef` 類型，並要求 `_dynamicUrl` 欄位。 例如，在 [WKND Site項目](https://github.com/adobe/aem-guides-wknd) 並在其中納入影像資產參考的影像URL `primaryImage` 欄位，可使用新的持續查詢完成 `wknd-shared/adventure-image-by-path` 定義為：

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

此 `$path` 變數 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

此 `_assetTransform` 定義 `_dynamicUrl` 被建構以最佳化提供的影像轉譯。 Web最佳化影像URL也可透過變更URL的查詢參數在用戶端上調整。

| GraphQL參數 | URL 參數 | 說明 | 必要 | GraphQL變數值 | URL參數值 | 範例URL參數 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/A | 影像資產的格式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/A | N/A |
| `seoName` | N/A | URL中的檔案區段名稱。 若未提供，則會使用影像資產名稱。 | ✘ | 字母數字， `-`，或 `_` | N/A | N/A |
| `crop` | `crop` | 從影像中擷取的裁切影格必須在影像大小內 | ✘ | 在原始影像維的邊界內定義裁切區域的正整數 | 數字座標的逗號分隔字串 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 輸出影像的大小（包括高度和寬度）（像素）。 | ✘ | 正整數 | 順序中的逗號分隔正整數 `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | 影像的旋轉度。 | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | 翻轉影像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | 影像品質（以原始品質的百分比表示）。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 輸出影像的寬度（像素）。 當 `size` 提供 `width` 會忽略。 | ✘ | 正整數 | 正整數 | `?width=1600` |
| `preferWebP` | `preferwebp` | 若 `true` 和AEM會提供WebP（若瀏覽器支援），無論 `format`. | ✘ | `true`, `false` | `true`, `false` | `?preferwebp=true` |

## GraphQL回應

產生的JSON回應包含要求的欄位，其中包含影像資產的網頁最佳化URL。

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

若要載入應用程式中參考影像的Web最佳化影像，請使用 `_dynamicUrl` 的 `primaryImage` 作為影像的來源URL。

在React中，從AEM Publish顯示網頁最佳化影像看起來類似：

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

記住， `_dynamicUrl` 不包含AEM網域，因此您必須提供影像URL所需的來源才能解析。

## 回應式URL

上述範例顯示使用單一大小的影像，但在網頁體驗中，通常需要回應式影像集。 回應式影像可使用 [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 或 [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). 下列程式碼片段示範如何使用 `_dynamicUrl` 作為基礎，並附加不同寬度參數，以支援不同的回應式檢視。 不僅 `width` 查詢參數可供使用，但用戶端可新增其他查詢參數，以根據影像資產的需求進一步最佳化影像資產。

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

## React範例

讓我們建立簡單的React應用程式，在之後顯示Web最佳化影像 [回應式影像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). 回應式影像有兩個主要模式：

+ [具有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 提高效能
+ [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 設計控制

### 具有srcset的Img元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[具有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 搭配使用 `sizes` 屬性來針對不同的螢幕大小提供不同的影像資產。 針對不同的螢幕大小提供不同的影像資產時，Img srcsets很有用。

### 圖片元素

[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 搭配多個 `source` 元素，針對不同的螢幕大小提供不同的影像資產。 針對不同的螢幕大小提供不同的影像轉譯時，圖片元素很實用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 范常式式碼

這個簡單的React應用程式使用 [AEM Headless SDK](./aem-headless-sdk.md) 查詢AEM無頭API以取得冒險內容，並使用 [帶有srcset的img元素](#img-element-with-srcset) 和 [圖片元素](#picture-element). 此 `srcset` 和 `sources` 使用自訂 `setParams` 函式，將網頁最佳化的傳送查詢參數附加至 `_dynamicUrl` ，因此請根據web用戶端需求變更傳送的影像轉譯。

在自訂React鈎點中執行AEM查詢 [使用AEM Headless SDK的useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries).

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
