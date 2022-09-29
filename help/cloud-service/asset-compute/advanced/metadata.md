---
title: 開發Asset compute中繼資料背景工作
description: 了解如何建立Asset compute中繼資料背景工作，該背景工作衍生影像資產中最常使用的顏色，並將顏色名稱寫回AEM中資產的中繼資料。
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# 開發Asset compute中繼資料背景工作

自訂Asset compute背景工作可產生XMP(XML)資料，並傳回至AEM並儲存為資產上的中繼資料。

常見的使用案例包括：

+ 與協力廠商系統的整合，例如PIM（產品資訊管理系統），其中必須擷取額外中繼資料並儲存在資產上
+ 與Adobe服務（例如內容與商務AI）整合，以透過其他機器學習屬性增加資產中繼資料
+ 從資產的二進位衍生資產的中繼資料，並將其儲存為AEM as a Cloud Service中的資產中繼資料

## 你將做什麼

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教學課程中，我們將建立Asset compute中繼資料背景工作，該背景工作衍生影像資產中最常使用的顏色，並將顏色名稱寫回AEM中資產的中繼資料。 雖然背景工作是基本的，但本教學課程會使用此工具來探索如何使用Asset compute背景工作將中繼資料回寫至AEMas a Cloud Service中的資產。

## asset compute元資料工作器調用的邏輯流

調用Asset compute元資料工作幾乎與調用 [二進位轉譯產生程式](../develop/worker.md)，主要差異為傳回類型是XMP(XML)轉譯，其值也會寫入資產的中繼資料。

asset compute背景工作實作Asset computeSDK背景工作API合約，位於 `renditionCallback(...)` 功能，其概念上：

+ __輸入：__ AEM資產的原始二進位和處理設定檔參數
+ __輸出：__ XMP(XML)轉譯會以轉譯形式持續存在AEM資產，以及保存在資產的中繼資料

![asset compute元資料工作邏輯流](./assets/metadata/logical-flow.png)

1. AEM製作服務會叫用Asset compute中繼資料背景工作，提供資產的 __(1a)__ 原始二進位和 __(1b)__ 處理設定檔中定義的任何參數。
1. asset computeSDK可協調自訂Asset compute中繼資料背景工作的執行 `renditionCallback(...)` 函式，根據資產的二進位檔衍生XMP(XML)轉譯 __(1a)__ 和任何處理設定檔參數 __(1b)__.
1. asset compute工作器將XMP(XML)表示保存到 `rendition.path`.
1. 寫入的XMP(XML)資料 `rendition.path` 會透過Asset computeSDK傳輸至AEM作者服務，並依 __(4a)__ 文字轉譯及 __(4b)__ 保存至資產的中繼資料節點。

## 設定manifest.yml{#manifest}

所有Asset compute員工都必須在 [manifest.yml](../develop/manifest.md).

開啟專案的 `manifest.yml` 並添加配置新員工的員工條目（在本例中） `metadata-colors`.

_記住 `.yml` 會區分大小寫。_

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

`function` 指向在 [下一步](#metadata-worker). 在語義上為背景工作命名(例如 `actions/worker/index.js` 可能更好地指明了 `actions/rendition-circle/index.js`)，如 [工作人員URL](#deploy) 並確定 [工作人員的測試套裝資料夾名稱](#test).

此 `limits` 和 `require-adobe-auth` 是按工作人員進行離散配置的。 在這個工人里， `512 MB` 當代碼檢查（可能）大的二進位影像資料時，分配儲存器。 另一個 `limits` 會移除以使用預設值。

## 開發中繼資料背景工作{#metadata-worker}

在路徑的Asset compute專案中建立新的中繼資料背景工作JavaScript檔案 [為新工作人員定義的manifest.yml](#manifest)，於 `/actions/metadata-colors/index.js`

### 安裝npm模組

安裝額外的npm模組([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors)，和 [顏色名稱](https://www.npmjs.com/package/color-namer))，此Asset compute背景工作中使用。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 中繼資料背景代碼

這個工人看起來非常類似 [轉譯產生背景工作](../develop/worker.md)，主要差異在於會將XMP(XML)資料寫入 `rendition.path` 才能儲存回AEM。


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
      // These namespaces are automatically registered in AEM if they do not yet exist
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

## 在本機執行中繼資料背景工作{#development-tool}

工作程式碼完成後，即可使用本機Asset compute開發工具執行。

因為我們的Asset compute專案包含兩個背景工作(前一個 [圓形轉譯](../develop/worker.md) 和這個 `metadata-colors` ), [asset compute開發工具的](../develop/development-tool.md) 配置檔案定義列出兩個工作的執行配置檔案。 第二個輪廓定義指向新的 `metadata-colors` 工人。

![XML元資料轉譯](./assets/metadata/metadata-rendition.png)

1. 從Asset compute專案的根目錄
1. 執行 `aio app run` 啟動Asset compute開發工具
1. 在 __選擇檔案……__ 下拉，選擇 [樣本影像](../assets/samples/sample-file.jpg) 處理
1. 在第二個設定檔定義設定中，此設定指向 `metadata-colors` 工作人員，更新 `"name": "rendition.xml"` 因為此工作器會生成XMP(XML)格式副本。 （可選）新增 `colorsFamily` 參數（支援的值） `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`)。

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

1. 點選 __執行__ 並等待XML格式副本生成
   + 由於兩個背景工作都列在設定檔定義中，因此兩個轉譯都會產生。 （可選）指向 [社交轉譯工作人員](../develop/worker.md) 可刪除，以避免從開發工具執行。
1. 此 __轉譯__ 區段會預覽產生的轉譯。 點選 `rendition.xml` 若要下載，請在VS Code（或您最喜愛的XML/文字編輯器）中開啟它以進行檢閱。

## 測試工作人員{#test}

中繼資料背景工作可使用 [與二進位轉譯相同的Asset compute測試架構](../test-debug/test.md). 唯一的差別是 `rendition.xxx` 測試案例中的檔案必須是預期的XMP(XML)轉譯。

1. 在Asset compute專案中建立下列結構：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用 [範例檔案](../assets/samples/sample-file.jpg) 作為測試案例 `file.jpg`.
3. 將下列JSON新增至 `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   請注意 `"fmt": "xml"` 需要，才能指示測試套裝產生 `.xml` 文字轉譯。

4. 在 `rendition.xml` 檔案。 這可透過下列方式取得：
   + 透過開發工具執行測試輸入檔案，並儲存（已驗證）XML轉譯。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 執行 `aio app test` 從Asset compute專案的根目錄，執行所有測試套裝。

### 將工作人員部署到Adobe I/O Runtime{#deploy}

若要從AEM Assets叫用此新中繼資料背景工作，必須使用命令將其部署至Adobe I/O Runtime:

```
$ aio app deploy
```

![aio app deploy](./assets/metadata/aio-app-deploy.png)

請注意，這將部署項目中的所有員工。 檢閱 [未刪除的部署說明](../deploy/runtime.md) 以了解如何部署至預備和生產工作區。

### 與AEM處理設定檔整合{#processing-profile}

通過建立新的或修改調用此已部署工作器的現有自定義處理配置檔案服務，從AEM中調用該工作器。

![處理設定檔](./assets/metadata/processing-profile.png)

1. 登入AEMas a Cloud Service作者服務，作為 __AEM管理員__
1. 導覽至 __工具>資產>處理設定檔__
1. __建立__ 新的，或 __編輯__ 和現有，處理設定檔
1. 點選 __自訂__ 標籤，然後點選 __新增__
1. 定義新服務
   + __建立中繼資料轉譯__:切換為作用中
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 這是工作人員在 [部署](#deploy) 或使用命令 `aio app get-url`. 根據AEMas a Cloud Service環境，確定URL點位在正確的工作區。
   + __服務參數__
      + 點選 __新增參數__
         + 關鍵: `colorFamily`
         + 值: `pantone`
            + 支援的值： `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Mime 類型__
      + __包括：__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + 這些是用於衍生顏色的第三方npm模組支援的唯一MIME類型。
      + __不包括：__ `Leave blank`
1. 點選 __儲存__ 在右上角
1. 將處理設定檔套用至AEM Assets資料夾（如果尚未這麼做的話）

### 更新中繼資料結構{#metadata-schema}

要查看顏色元資料，請將影像元資料結構上的兩個新欄位映射到工作人員填充的新元資料資料屬性。

![中繼資料結構](./assets/metadata/metadata-schema.png)

1. 在AEM Author服務中，導覽至 __工具>資產>中繼資料結構__
1. 導覽至 __預設__ 並選擇和編輯 __影像__ 以及新增唯讀表單欄位，以公開產生的顏色中繼資料
1. 新增 __單行文字__
   + __欄位標籤__: `Colors Family`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __規則>欄位>停用編輯__:已勾選
1. 新增 __多值文字__
   + __欄位標籤__: `Colors`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colors`
1. 點選 __儲存__ 在右上角

## 處理資產

![資產詳細內容](./assets/metadata/asset-details.png)

1. 在AEM Author服務中，導覽至 __資產>檔案__
1. 導覽至資料夾或子資料夾，處理設定檔便會套用至
1. 將新影像(JPEG、PNG、GIF或SVG)上傳至資料夾，或使用更新的重新處理現有影像 [處理設定檔](#processing-profile)
1. 處理完成時，選取資產，然後點選 __屬性__ 顯示其中繼資料
1. 檢閱 `Colors Family` 和 `Colors` [中繼資料欄位](#metadata-schema) 從自訂Asset compute中繼資料背景工作寫入的中繼資料。

在將顏色中繼資料寫入至資產的中繼資料後， `[dam:Asset]/jcr:content/metadata` 資源，此中繼資料會透過搜尋，以增加使用這些詞語的資產探索能力，而且若使用，這些中繼資料甚至可以寫回資產的二進位檔 __DAM中繼資料回寫__ 調用工作流。

### AEM Assets中的中繼資料轉譯

![AEM Assets中繼資料轉譯檔案](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute中繼資料背景工作產生的實際XMP檔案也會儲存為資產上的離散轉譯。 通常不會使用此檔案，而是會使用套用至資產中繼資料節點的值，但背景工作的原始XML輸出可在AEM中使用。

## Github上的中繼資料色彩背景代碼

最後 `metadata-colors/index.js` 可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最後 `test/asset-compute/metadata-colors` Github上提供測試套裝：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata/colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
