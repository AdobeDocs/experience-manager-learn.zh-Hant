---
title: 開發資產計算元資料工作器
description: 瞭解如何建立「資產計算」中繼資料工作器，以衍生影像資產中最常用的顏色，並將顏色名稱寫回AEM中資產的中繼資料。
feature: asset-compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
translation-type: tm+mt
source-git-commit: c2a8e6c3ae6dcaa45816b1d3efe569126c6c1e60
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 1%

---


# 開發資產計算元資料工作器

自訂資產計算工作者可以產生XMP(XML)資料，並傳回至AEM並儲存為資產的中繼資料。

常見使用案例包括：

+ 與協力廠商系統的整合，例如PIM（產品資訊管理系統），在此系統中，必須擷取額外的中繼資料並儲存在資產上
+ 與Adobe服務（例如Content and Commerce AI）整合，以運用額外的機器學習屬性來增強資產中繼資料
+ 從資產的二進位元資料中衍生資產的中繼資料，並將它儲存為AEM中的資產中繼資料，做為雲端服務

## 您將做的事

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教學課程中，我們將建立「資產計算」中繼資料工作器，以衍生影像資產中最常用的顏色，並將顏色名稱寫回AEM中資產的中繼資料。 雖然工作者本身是基本的，但本教學課程使用它來探索如何使用資產計算工作者將中繼資料寫回AEM中的資產，做為雲端服務。

## 資產計算元資料工作器調用的邏輯流

「資產計算」中繼資料工作器的呼叫幾乎與產生工作器的二進位轉譯相同 [](../develop/worker.md)，其主要差異是傳回類型是XMP(XML)轉譯，其值也會寫入資產的中繼資料。

資產計算工作者在功能中實作資產計算SDK工作者API合 `renditionCallback(...)` 約，該合約在概念上：

+ __輸入：__ AEM資產的原始二進位和處理描述檔參數
+ __輸出：__ XMP(XML)轉譯會以轉譯形式持續存留至AEM資產，以及資產的中繼資料

![資產計算元資料工作器邏輯流](./assets/metadata/logical-flow.png)

1. AEM Author服務會叫用「資產計算」中繼資料工作器，提供資產的 __(1a)__ 原始二進位檔， __以及(1b)__ 「處理設定檔」中定義的任何參數。
1. Asset Compute SDK可協調自訂Asset Compute中繼資料工作者的功能，並根據資產的二進位(1a) `renditionCallback(...)` ，以及任何處理設定檔參數 __(1b)來衍生XMP(XML)轉譯______。
1. 「資產計算」工作人員將XMP(XML)表示法保存到 `rendition.path`。
1. 寫入的XMP(XML)資料會透過 `rendition.path` Asset Compute SDK傳輸至AEM Author Service，並以 __(4a)__ 、文字轉譯和 __(4b)__ （保存至資產的中繼資料節點）形式公開。

## 設定manifest.yml{#manifest}

所有資產計算工作者都必須在 [manifest.yml中註冊](../develop/manifest.md)。

開啟項目並添 `manifest.yml` 加配置新工作器的工作器條目（在本例中） `metadata-colors`。

_請記 `.yml` 住，空格很敏感。_

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` 指向在下一步中建立的工 [作器實施](#metadata-worker)。 按語義命名工作者(例如， `actions/worker/index.js` 可能更好地命名 `actions/rendition-circle/index.js`)，如 [員工URL中所示](#deploy) ，並確定工 [作者的測試套件資料夾名稱](#test)。

和 `limits` 是按 `require-adobe-auth` 每個工作者獨立配置的。 在此工作器中， `512 MB` 當代碼檢查（可能）大的二進位影像資料時分配記憶體。 其他項 `limits` 目則會移除，以使用預設值。

## 開發中繼資料工作者{#metadata-worker}

在「資產計算」專案中，為新工作者建立新的中繼資料工 [作者JavaScript檔案，位於為新工作者定義](#manifest)manifest.yml的路徑，網址為 `/actions/metadata-colors/index.js`

### 安裝npm模組

安裝此資產計算工[作器中將使用的額外npm模組(](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)[@adobe/asset-compute-xmp](https://www.npmjs.com/package/get-image-colors)、 [get-image-colors](https://www.npmjs.com/package/color-namer)和color-namer)。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 中繼資料工作程式碼

此工作器看起來非常類似 [於轉譯產生工作器](../develop/worker.md)，主要的差別在於，它會將XMP(XML)資料寫入， `rendition.path` 以儲存回AEM。


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces will be automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## 在本機執行中繼資料工作器{#development-tool}

工作代碼完成後，可以使用本地資產計算開發工具執行。

由於我們的「資產計算」項目包含兩個工作者(前一個 [圓圈轉譯](../develop/worker.md) , `metadata-colors` 和此工作者), [](../develop/development-tool.md) Asset Compute Development Tool的配置檔案定義列出了兩個工作者的執行配置檔案。 第二個配置檔案定義指向新工 `metadata-colors` 作器。

![XML中繼資料轉譯](./assets/metadata/metadata-rendition.png)

1. 從資產計算項目的根目錄
1. 執行 `aio app run` 以啟動資產計算開發工具
1. 在「選 __擇檔案……」中__ 下拉式清單，挑選 [要處理的](../assets/samples/sample-file.jpg) 範例影像
1. 在指向工作者的第二個描述檔定義設定 `metadata-colors` 中，當 `"name": "rendition.xml"` 此工作者產生XMP(XML)轉譯時更新。 （可選）添 `colorsFamily` 加參數(支 `basic`持的 `hex`值、 `html`、 `ntc`、 `pantone`、 `roygbiv`)。

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```
1. 點選 __「執行__ 」，然後等待XML轉譯產生
   + 由於兩個工作者都列在描述檔定義中，因此兩個轉譯都會產生。 （可選）可以刪除指向社交圈轉譯工 [作器的頂部配置檔案定義](../develop/worker.md) ，以避免從「開發工具」中執行該定義。
1. 「轉 __譯__ 」區段會預覽產生的轉譯。 點選以 `rendition.xml` 下載它，然後在VS程式碼（或您最愛的XML/文字編輯器）中開啟它以進行審核。

## 測試工作者{#test}

中繼資料工作者可使用與二進位轉譯 [相同的資產計算測試架構進行測試](../test-debug/test.md)。 唯一的差異是測 `rendition.xxx` 試案例中的檔案必須是預期的XMP(XML)轉譯。

1. 在「資產計算」項目中建立以下結構：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用 [範例檔](../assets/samples/sample-file.jpg) ，做為測試案例的範例檔 `file.jpg`。
3. 將下列JSON新增至 `params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   請注意， `"fmt": "xml"` 必須指示測試套裝產生文 `.xml` 字型轉譯。

4. 在檔案中提供預期的XML `rendition.xml` 。 這可透過下列方式取得：
   + 透過開發工具執行測試輸入檔案，並將（已驗證）的XML轉譯儲存出來。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 從資 `aio app test` 產計算專案的根目錄執行，以執行所有測試套裝。

### 將工作者部署至Adobe I/O Runtime{#deploy}

若要從AEM Assets叫用這個新的中繼資料工作者，必須使用下列命令將其部署至Adobe I/O Runtime:

```
$ aio app deploy
```

![aio app deploy](./assets/metadata/aio-app-deploy.png)

請注意，這將部署項目中的所有員工。 檢視如何 [部署至「舞台](../deploy/runtime.md) 」和「生產」工作區的簡略部署指示。

### 與AEM處理設定檔整合{#processing-profile}

從AEM呼叫工作者，方法是建立新的工作者，或修改現有的自訂處理設定檔服務，以叫用此已部署的工作者。

![處理設定檔](./assets/metadata/processing-profile.png)

1. 以 __AEM管理員身分登入AEM做為雲端服務作者服務__
1. 導覽至「 __工具>資產>處理設定檔」__
1. __建立新__ 、編輯和 __現有__ 「處理設定檔」
1. 點選「自 __訂__ 」標籤，點選「新 __增」__
1. 定義新服務
   + __建立中繼資料轉譯__:切換至作用中
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 這是在部署期間或使用命令時獲 [得](#deploy) 的工作器的URL `aio app get-url`。 根據AEM做為雲端服務環境，確保URL點位於正確的工作區。
   + __服務參數__
      + 點選「 __新增參數」__
         + 關鍵: `colorFamily`
         + 值: `pantone`
            + 支援的值： `basic`、 `hex`、 `html`、 `ntc`、 `pantone`、 `roygbiv`
   + __Mime 類型__
      + __包括：__`image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + 這些是第三方NPM模組支援的唯一用於導出顏色的MIME類型。
      + __排除：__ `Leave blank`
1. 點選 __右上__ 「儲存」
1. 如果尚未將「處理設定檔」套用至AEM Assets檔案夾

### 更新中繼資料結構{#metadata-schema}

要查看顏色元資料，請將影像元資料架構上的兩個新欄位映射到工作者填充的新元資料資料屬性。

![中繼資料結構](./assets/metadata/metadata-schema.png)

1. 在AEM Author服務中，導覽至「工具> __資產>中繼資料結構」__
1. 導覽至預 __設值__ ，然後選取和編輯 __影像__ ，並新增唯讀表單欄位，以公開產生的色彩中繼資料
1. 新增單 __行文字__
   + __欄位標籤__: `Colors Family`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __規則>欄位>停用編輯__:已勾選
1. 新增多 __值文字__
   + __欄位標籤__: `Colors`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colors`
1. 點選 __右上__ 「儲存」

## 處理資產

![資產詳細內容](./assets/metadata/asset-details.png)

1. 在AEM Author服務中，導覽至「資產> __檔案」__
1. 導覽至資料夾或子資料夾，「處理設定檔」會套用至
1. 將新影像（JPEG、PNG、GIF或SVG）上傳至檔案夾，或使用更新的「處理設定檔」重新處理現 [有影像](#processing-profile)
1. 處理完成時，選取資產，並點選頂端動 __作列中的__ 「屬性」以顯示其中繼資料
1. 查看從自 `Colors Family` 訂Asset Compute中繼資 `Colors` 料工作器回寫的中繼資料 [](#metadata-schema) ，以及中繼資料欄位。

將顏色中繼資料寫入資產的中繼資料（在資源上）後，使用這些詞語透過搜尋為此中繼資料建立索引，以增強資產發現能力，而且如果在資源上呼叫 `[dam:Asset]/jcr:content/metadata`____ DAM中繼資料回寫工作流程，則甚至可將這些中繼資料回寫資產的二進位檔案。

### AEM Assets中的中繼資料轉譯

![AEM Assets中繼資料轉譯檔案](./assets/metadata/cqdam-metadata-rendition.png)

資產計算元資料工作器生成的實際XMP檔案也儲存為資產上的離散格式副本。 此檔案通常不會使用，而是會使用套用至資產中繼資料節點的值，但AEM中會提供來自工作者的原始XML輸出。

## Github上的中繼資料色彩工作程式碼

Github上 `metadata-colors/index.js` 提供最終版本：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Github上提 `test/asset-compute/metadata-colors` 供最終測試套件，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
