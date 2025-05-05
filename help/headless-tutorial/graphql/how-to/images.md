---
title: 搭配AEM Headless使用最佳化的影像
description: 瞭解如何使用AEM Headless請求最佳化的影像URL。
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# 利用 AEM Headless 的最佳化影像 {#images-with-aem-headless}

影像是[開發豐富、極具吸引力的AEM Headless體驗的重要方面](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hant)。 AEM Headless支援影像資產的管理及其最佳化傳送。

AEM Headless內容模型中使用的內容片段，通常會參照要在Headless體驗中顯示的影像資產。 AEM的GraphQL查詢可以寫入，以根據影像參考來源提供URL給影像。

`ImageRef`型別有四個URL選項供內容參照使用：

+ `_path`是AEM中的參考路徑，不包含AEM來源（主機名稱）
+ `_dynamicUrl`是影像資產網頁最佳化傳遞的URL。
   + `_dynamicUrl`不包含AEM來源，因此網域(AEM Author或AEM Publish服務)必須由使用者端應用程式提供。
+ `_authorUrl`是AEM作者上影像資產的完整URL
   + [AEM Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=zh-Hant)可用於提供Headless應用程式的預覽體驗。
+ `_publishUrl`是AEM發佈上影像資產的完整URL
   + [AEM Publish](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=zh-Hant)通常是Headless應用程式的生產部署顯示影像的位置。

`_dynamicUrl`是建議用於影像資產傳送的URL，應儘可能取代使用`_path`、`_authorUrl`和`_publishUrl`。

|                                | AEM as a Cloud Service  | AEM AS A CLOUD SERVICE RDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 支援Web最佳化的影像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="影像與 AEM Headless"
>abstract="了解 AEM Headless 如何支援影像資產的管理及其最佳化傳遞。"

## 內容片段模型

確定包含影像參考的內容片段欄位是&#x200B;__內容參考__&#x200B;資料型別。

在[內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=zh-Hant)中檢閱欄位型別，方法是選取欄位並檢查右側的&#x200B;__屬性__&#x200B;索引標籤。

![內容片段模型參考影像](./assets/images/content-fragment-model.jpeg)

## GraphQL持續查詢

在GraphQL查詢中，傳回欄位作為`ImageRef`型別，並要求`_dynamicUrl`欄位。 例如，在[WKND網站專案](https://github.com/adobe/aem-guides-wknd)中查詢冒險並在其`primaryImage`欄位中包含影像資產參考的影像URL，可以使用定義如下`wknd-shared/adventure-image-by-path`的新持續查詢：

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

`_path`篩選器中使用的`$path`變數需要內容片段的完整路徑（例如`/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`）。

`_assetTransform`定義如何建構`_dynamicUrl`以最佳化提供的影像轉譯。 您也可以變更URL的查詢引數，在使用者端上調整Web最佳化的影像URL。

| GraphQL引數 | 描述 | 必填 | GraphQL變數值 |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | 影像資產的格式。 | ✔ | `GIF`、`PNG`、`PNG8`、`JPG`、`PJPG`、`BJPG`、`WEBP`、`WEBPLL`、`WEBPLY` |
| `seoName` | URL中的檔案區段名稱。 若未提供，則會使用影像資產名稱。 | ✘ | 英數、`-`或`_` |
| `crop` | 裁切影格從影像中取出，必須在影像大小範圍內 | ✘ | 正整數，定義原始影像尺寸範圍內的裁切區域 |
| `size` | 輸出影像的大小（高度和寬度），以畫素為單位。 | ✘ | 正整數 |
| `rotation` | 影像的旋轉，以度為單位。 | ✘ | `R90`, `R180`, `R270` |
| `flip` | 翻轉影像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | 影像品質，以原始品質的百分比表示。 | ✘ | 1-100 |
| `width` | 輸出影像的寬度（畫素）。 提供`size`時，會忽略`width`。 | ✘ | 正整數 |
| `preferWebP` | 如果`true`和AEM在瀏覽器支援的情況下提供WebP （不論`format`為何）。 | ✘ | `true`、`false` |


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

若要載入應用程式中參照影像的網頁最佳化影像，請使用`primaryImage`的`_dynamicUrl`做為影像的來源URL。

在React中，從AEM Publish顯示網頁最佳化影像的外觀如下：

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

請記住，`_dynamicUrl`不包含AEM網域，因此您必須提供所要的原點以供影像URL解析。

## 回應式URL

上述範例顯示使用單一大小的影像，不過在網頁體驗中，通常需要回應式影像集。 可以使用[img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)或[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)實作回應式影像。 下列程式碼片段顯示如何使用`_dynamicUrl`作為基底。 `width`是URL引數，您可以將其附加至`_dynamicUrl`以支援不同的回應式檢視。

```javascript
// The AEM host is usually read from a environment variable of the SPA.
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

讓我們建立簡易的React應用程式，顯示遵循[回應式影像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/)的網頁最佳化影像。 回應式影像的主要模式有兩種：

+ [具有srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)的Img元素以提高效能
+ 設計控制項的[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)

### 具有srcset的Img元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[具有srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)的Img元素與`sizes`屬性搭配使用，為不同的熒幕大小提供不同的影像資產。 針對不同的熒幕大小提供不同的影像資產時，影像資料集相當實用。

### 圖片元素

[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)與多個`source`元素搭配使用，為不同的熒幕大小提供不同的影像資產。 為不同的熒幕大小提供不同的影像轉譯時，圖片元素相當實用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 範常式式碼

這個簡單的React應用程式使用[AEM Headless SDK](./aem-headless-sdk.md)來查詢AEM Headless API的Adventure內容，並使用具有srcset[&#128279;](#img-element-with-srcset)和[picture element](#picture-element)的img元素來顯示網頁最佳化的影像。 `srcset`和`sources`使用自訂`setParams`函式，將Web最佳化傳遞查詢引數附加至影像的`_dynamicUrl`，因此請變更根據網頁使用者端需求傳遞的影像轉譯。

在使用AEM Headless SDK[&#128279;](./aem-headless-sdk.md#graphql-persisted-queries)的自訂React勾點useAdventureByPath中執行針對AEM的查詢。

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
