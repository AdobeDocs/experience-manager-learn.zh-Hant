---
title: 開發Asset compute中繼資料工作者
description: 瞭解如何建立Asset compute中繼資料工作器，以衍生影像資產中最常用的色彩，並將色彩名稱寫回資產的中繼資料AEM。
feature: Asset Compute Microservices
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1442'
ht-degree: 1%

---


# 開發Asset compute中繼資料工作者

自訂Asset compute工作XMP者可產生(XML)資料，並傳回AEM至資產並儲存為中繼資料。

常見使用案例包括：

+ 與協力廠商系統的整合，例如PIM（產品資訊管理系統），在此系統中，必須擷取額外的中繼資料並儲存在資產上
+ 與Adobe服務（例如Content and Commerce AI）整合，以運用額外的機器學習屬性來增強資產中繼資料
+ 從資產的二進位元資料衍生資產的中繼資料，並將其儲存為資產中繼資料，AEM作為Cloud Service

## 您將做的事

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教學課程中，我們將建立Asset compute中繼資料工作器，以衍生影像資產中最常用的色彩，並將色彩名稱寫回資產的中繼資料AEM。 雖然工作者本身是基本的，但本教學課程會使用它來探索如何使用Asset compute工作者將中繼資料寫回AEMCloud Service中的資產。

## asset compute元資料工作器調用的邏輯流

調用Asset compute元資料工作器與生成工作器](../develop/worker.md)的[二進位格式副本的調用幾乎相同，主要區別是返回類型是XMP(XML)格式副本，其值也寫入到資產的元資料。

asset compute員工在`renditionCallback(...)`函式中實作Asset computeSDK員工API合約，其概念是：

+ __輸入：__ 資AEM產的原始二進位和處理描述檔參數
+ __輸出：__ (XMPXML)轉譯會以轉譯的形式持AEM續存留至資產，以及資產的中繼資料

![asset compute元資料工作器邏輯流](./assets/metadata/logical-flow.png)

1. AEM Author服務會叫用Asset compute中繼資料工作者，提供資產的&#x200B;__(1a)__&#x200B;原始二進位檔，以及&#x200B;__(1b)__&#x200B;處理設定檔中定義的任何參數。
1. asset computeSDK協調自訂Asset compute中繼資料工作者的`renditionCallback(...)`功能，根據資產的二進位&#x200B;__(1a)__&#x200B;和任何處理描述檔參數&#x200B;__(1b)__&#x200B;來衍生XMP(XML)轉譯。
1. asset compute工作者將XMP(XML)表示法保存到`rendition.path`。
1. 寫入XMP至`rendition.path`的(XML)資料會透過Asset computeSDK傳輸至AEM Author Service，並以&#x200B;__(4a)__&#x200B;文字轉譯和保存至資產中繼資料節點的&#x200B;__(4b)__&#x200B;形式公開。

## 設定manifest.yml{#manifest}

所有Asset compute工作者都必須在[manifest.yml](../develop/manifest.md)中註冊。

開啟項目的`manifest.yml`並添加一個配置新工作器的工作器條目，在本例中為`metadata-colors`。

_記住 `.yml` 對空格很敏感。_

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

`function` 指向在下一步中建立的工 [作器實施](#metadata-worker)。以語義方式為工作者命名（例如，`actions/worker/index.js`可能更好地命名為`actions/rendition-circle/index.js`），如[工作者的URL](#deploy)中所示，並且還確定[工作者的測試套件資料夾名稱](#test)。

`limits`和`require-adobe-auth`是按每個工作者單獨配置的。 在此工作器中，當代碼檢查（可能）大的二進位影像資料時，將分配`512 MB`記憶體。 其他`limits`會被移除，以使用預設值。

## 開發元資料工作器{#metadata-worker}

在Asset compute項目中，為新工作者](#manifest)的`/actions/metadata-colors/index.js`路徑[定義的manifest.yml中建立新的元資料工作者JavaScript檔案

### 安裝npm模組

安裝此Asset compute工作者將使用的額外npm模組（[@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)和[color-namer](https://www.npmjs.com/package/color-namer)）。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 中繼資料工作程式碼

此工作器外觀與[格式副本生成工作器](../develop/worker.md)非常相似，主要區別在於它將XMP(XML)資料寫入`rendition.path`以保存回AEM。


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

在工作程式碼完成後，就可使用本機Asset compute開發工具來執行。

由於我們的Asset compute項目包含兩個工作程式（前一個[圓轉譯](../develop/worker.md)和此`metadata-colors`工作程式），因此[Asset compute開發工具的](../develop/development-tool.md)配置檔案定義列出了兩個工作程式的執行配置檔案。 第二個配置檔案定義指向新的`metadata-colors`工作器。

![XML中繼資料轉譯](./assets/metadata/metadata-rendition.png)

1. 從Asset compute項目的根
1. 執行`aio app run`以啟動Asset compute開發工具
1. 在&#x200B;__選擇檔案……__&#x200B;下拉式清單，挑選[範例影像](../assets/samples/sample-file.jpg)進行處理
1. 在指向`metadata-colors`工作器的第二個配置檔案定義配置中，當此工作器生成(XML)格式副本時更新XMP`"name": "rendition.xml"`。 或者，可以添加`colorsFamily`參數（支援的值`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）。

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
1. 點選「__執行__」，等待XML轉譯產生
   + 由於兩個工作者都列在描述檔定義中，因此兩個轉譯都會產生。 或者，可以刪除指向[圓形轉譯工作者](../develop/worker.md)的頂部配置檔案定義，以避免從「開發工具」中執行該定義。
1. __轉譯__&#x200B;區段會預覽產生的轉譯。 點選`rendition.xml`以下載它，然後在VS程式碼（或您最愛的XML/文字編輯器）中開啟它以進行審核。

## 測試工作者{#test}

可使用與二進位轉譯](../test-debug/test.md)相同的Asset compute測試框架來測試元資料工作器。 [唯一的差異是測試案例中的`rendition.xxx`檔案必須是預期的XMP(XML)轉譯。

1. 在Asset compute專案中建立下列結構：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用[範例檔](../assets/samples/sample-file.jpg)作為測試案例的`file.jpg`。
3. 將下列JSON新增至`params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   請注意，`"fmt": "xml"`必須指示測試套裝產生`.xml`文字轉譯。

4. 在`rendition.xml`檔案中提供預期的XML。 這可透過下列方式取得：
   + 透過開發工具執行測試輸入檔案，並將（已驗證）的XML轉譯儲存出來。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 從Asset compute項目的根目錄執行`aio app test`以執行所有測試套件。

### 將員工部署到Adobe I/O Runtime{#deploy}

要從AEM Assets調用此新的元資料工作器，必須使用以下命令將其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![aio app deploy](./assets/metadata/aio-app-deploy.png)

請注意，這將部署項目中的所有員工。 請參閱[未加細部署指示](../deploy/runtime.md)，瞭解如何部署至「舞台(Stage)」和「生產(Production)」工作區。

### 與處AEM理描述檔整合{#processing-profile}

通過建立新工AEM作器或修改調用此已部署工作器的現有自定義處理配置檔案服務來調用該工作器。

![處理設定檔](./assets/metadata/processing-profile.png)

1. 以&lt;AEMa0/>管理員&lt;a1/AEM>的身分登入Cloud Service作者服務&#x200B;____
1. 導覽至「__工具>資產>處理設定檔__」
1. __建__ 立新的或編輯 ____ 和現有的處理描述檔
1. 點選「__Custom__」標籤，點選「__新增__」
1. 定義新服務
   + __建立中繼資料轉譯__:切換至作用中
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 這是在[deploy](#deploy)或使用命令`aio app get-url`期間獲取的工作員的URL。 根據作為Cloud Service環境，確保URL點位於AEM正確的工作區。
   + __服務參數__
      + 點選&#x200B;__新增參數__
         + 關鍵: `colorFamily`
         + 值: `pantone`
            + 支援的值：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __Mime 類型__
      + __包括：__ `image/jpeg`、 `image/png`、 `image/gif`、  `image/svg`
         + 這些是第三方NPM模組支援的唯一用於導出顏色的MIME類型。
      + __排除：__ `Leave blank`
1. 點選右上角的&#x200B;__Save__
1. 如果尚未將處理設定檔套用至AEM Assets檔案夾

### 更新中繼資料結構{#metadata-schema}

要查看顏色元資料，請將影像元資料架構上的兩個新欄位映射到工作者填充的新元資料資料屬性。

![中繼資料結構](./assets/metadata/metadata-schema.png)

1. 在AEM Author服務中，導覽至&#x200B;__工具>資產>中繼資料結構__
1. 導覽至&#x200B;__default__&#x200B;並選取並編輯&#x200B;__image__，並新增唯讀表單欄位，以公開產生的色彩中繼資料
1. 新增&#x200B;__單行文字__
   + __欄位標籤__: `Colors Family`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __規則>欄位>停用編輯__:已勾選
1. 新增&#x200B;__多值文字__
   + __欄位標籤__: `Colors`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colors`
1. 點選右上角的&#x200B;__Save__

## 處理資產

![資產詳細內容](./assets/metadata/asset-details.png)

1. 在AEM Author服務中，導覽至「__資產>檔案__」
1. 導覽至資料夾或子資料夾，「處理設定檔」會套用至
1. 將新影像（JPEG、PNG、GIF或SVG）上傳至檔案夾，或使用更新的「處理描述檔[」重新處理現有影像](#processing-profile)
1. 處理完成時，請選取資產，然後點選頂端動作列中的&#x200B;__屬性__&#x200B;以顯示其中繼資料
1. 查看從自定義Asset compute元資料工作器回寫的元資料的`Colors Family`和`Colors` [元資料欄位](#metadata-schema)。

在`[dam:Asset]/jcr:content/metadata`資源上，將顏色中繼資料寫入資產的中繼資料，透過搜尋，利用這些詞語為此中繼資料建立索引，並且，如果在資源上呼叫&#x200B;__DAM中繼資料回寫__&#x200B;工作流程，甚至可將這些中繼資料回寫至資產的二進位檔案。

### AEM Assets中繼資料轉譯

![AEM Assets元資料轉譯檔案](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元XMP資料工作器生成的實際檔案也儲存為資產上的離散格式副本。 通常不會使用此檔案，而是使用應用到資產元資料節點的值，但工作器的原始XML輸出可在中使AEM用。

## Github上的中繼資料色彩工作程式碼

最終`metadata-colors/index.js`可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最終`test/asset-compute/metadata-colors`測試套件可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
