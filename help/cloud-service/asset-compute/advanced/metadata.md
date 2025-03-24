---
title: 開發Asset Compute中繼資料背景工作
description: 瞭解如何建立Asset Compute中繼資料背景工作，衍生影像資產中最常使用的顏色，並將顏色名稱寫入回AEM中的資產中繼資料。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# 開發Asset Compute中繼資料背景工作

自訂Asset Compute背景工作可產生XMP (XML)資料，這些資料會傳回AEM並儲存為資產上的中繼資料。

常見使用案例包含：

+ 與協力廠商系統(例如PIM （產品資訊管理系統）)的整合，其中必須擷取其他中繼資料並儲存在資產上
+ 與Adobe服務(例如內容和Commerce AI)整合，使用其他機器學習屬性來增強資產中繼資料
+ 從資產的二進位檔取得資產的相關中繼資料，並將其儲存為AEM as a Cloud Service中的資產中繼資料

## 您將要執行的動作

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教學課程中，我們將建立Asset Compute中繼資料背景工作，它會衍生影像資產中最常使用的顏色，並將顏色名稱寫入回AEM中的資產中繼資料。 雖然背景工作程式本身至關重要，但本教學課程將利用此背景工作程式來探索如何使用Asset Compute背景工作程式將中繼資料回寫至AEM as a Cloud Service中的資產。

## Asset Compute中繼資料背景工作引動處理的邏輯流程

Asset Compute中繼資料背景工作程式的引動方式與[產生背景工作程式的二進位轉譯](../develop/worker.md)的引動方式幾乎相同，主要差異在於傳回型別是XMP (XML)轉譯，其值也會寫入資產的中繼資料。

Asset Compute背景工作在`renditionCallback(...)`函式中實作Asset Compute SDK背景工作API合約，其概念為：

+ __輸入：__ AEM資產的原始二進位檔及處理設定檔引數
+ __輸出：__ XMP (XML)轉譯持續儲存至AEM資產作為轉譯，以及資產的中繼資料

![Asset Compute中繼資料背景工作邏輯流程](./assets/metadata/logical-flow.png)

1. AEM作者服務會叫用Asset Compute中繼資料背景工作，提供資產的&#x200B;__(1a)__&#x200B;原始二進位檔，以及&#x200B;__(1b)__&#x200B;處理設定檔中定義的任何引數。
1. Asset Compute SDK會根據資產的二進位&#x200B;__(1a)__&#x200B;和任何處理設定檔引數&#x200B;__(1b)__，協調執行自訂Asset Compute中繼資料背景工作程式的`renditionCallback(...)`函式，以衍生XMP (XML)轉譯。
1. Asset Compute Worker會將XMP (XML)表示儲存至`rendition.path`。
1. 寫入至`rendition.path`的XMP (XML)資料會透過Asset Compute SDK傳輸至AEM Author Service，並將它公開為&#x200B;__(4a)__&#x200B;文字轉譯，而&#x200B;__(4b)__&#x200B;會儲存至資產的中繼資料節點。

## 設定manifest.yml{#manifest}

所有Asset Compute背景工作程式都必須在[manifest.yml](../develop/manifest.md)中註冊。

開啟專案的`manifest.yml`，並新增設定新背景工作的背景工作專案，在此例中為`metadata-colors`。

_記住`.yml`有空白字元限制。_

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

`function`指向在[下一個步驟](#metadata-worker)中建立的背景工作實作。 以語意為背景工作者命名（例如，`actions/worker/index.js`可能更適合命名為`actions/rendition-circle/index.js`），因為這些名稱會顯示在[背景工作者的URL](#deploy)中，而且也會決定[背景工作者的測試套件資料夾名稱](#test)。

`limits`與`require-adobe-auth`是分別設定給每個背景工作程式。 在這個Worker中，`512 MB`的記憶體配置給程式碼檢查（可能）大型二進位影像資料。 其他`limits`已移除以使用預設值。

## 開發中繼資料背景工作{#metadata-worker}

在Asset Compute專案中建立新的中繼資料背景工作JavaScript檔案，路徑為[定義新背景工作](#manifest)，路徑為`/actions/metadata-colors/index.js`

### 安裝npm模組

安裝此Asset Compute背景工作程式中使用的額外npm模組([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)和[color-namer](https://www.npmjs.com/package/color-namer))。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 中繼資料背景工作代碼

此背景工作程式看起來與[產生轉譯背景工作程式](../develop/worker.md)非常類似，主要差異在於它會將XMP (XML)資料寫入`rendition.path`以儲存回AEM。


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

背景工作程式碼完成後，可使用本機Asset Compute開發工具執行。

由於我們的Asset Compute專案包含兩個背景工作（先前的[circle轉譯](../develop/worker.md)和此`metadata-colors`背景工作），[Asset Compute開發工具的](../develop/development-tool.md)設定檔定義會列出兩個背景工作的執行設定檔。 第二個設定檔定義指向新的`metadata-colors`背景工作。

![XML中繼資料轉譯](./assets/metadata/metadata-rendition.png)

1. 從Asset Compute專案的根目錄
1. 執行`aio app run`以啟動Asset Compute開發工具
1. 在&#x200B;__選取檔案……__&#x200B;下拉式清單中，挑選要處理的[範例影像](../assets/samples/sample-file.jpg)
1. 在指向`metadata-colors`背景工作程式的第二個設定檔定義設定中，當此背景工作程式產生XMP (XML)轉譯時請更新`"name": "rendition.xml"`。 選擇性地新增`colorsFamily`引數（支援的值`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）。

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

1. 點選&#x200B;__執行__，然後等待產生XML轉譯
   + 由於兩個背景工作都列在設定檔定義中，因此兩個轉譯都會產生。 可選擇性地刪除指向[circle轉譯背景工作](../develop/worker.md)的上層設定檔定義，以避免從開發工具執行它。
1. __轉譯__&#x200B;區段會預覽產生的轉譯。 點選`rendition.xml`以下載它，然後以VS Code （或您最愛的XML/文字編輯器）開啟它以進行檢閱。

## 測試背景工作{#test}

中繼資料背景工作可以使用[與二進位轉譯相同的Asset Compute測試架構進行測試](../test-debug/test.md)。 唯一的差異是測試案例中的`rendition.xxx`檔案必須是預期的XMP (XML)轉譯。

1. 在Asset Compute專案中建立下列結構：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用[範例檔案](../assets/samples/sample-file.jpg)作為測試案例的`file.jpg`。
3. 將下列JSON新增至`params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   請注意，需要`"fmt": "xml"`才能指示測試套件產生`.xml`文字型轉譯。

4. 在`rendition.xml`檔案中提供預期的XML。 這可透過以下方式取得：
   + 透過開發工具執行測試輸入檔案並儲存（已驗證的） XML轉譯。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 從Asset Compute專案的根目錄執行`aio app test`以執行所有測試套裝。

### 將背景工作部署至Adobe I/O Runtime{#deploy}

若要從AEM Assets叫用這個新的中繼資料工作程式，必須使用命令將其部署到Adobe I/O Runtime：

```
$ aio app deploy
```

![aio應用程式部署](./assets/metadata/aio-app-deploy.png)

請注意，這將部署專案中的所有背景工作。 檢閱[未刪節的部署指示](../deploy/runtime.md)，瞭解如何部署至中繼和生產工作區。

### 與AEM處理設定檔整合{#processing-profile}

透過建立新的或修改現有的自訂處理設定檔服務從AEM叫用背景工作，以叫用這個已部署的背景工作。

![正在處理設定檔](./assets/metadata/processing-profile.png)

1. 以&#x200B;__AEM as a Cloud Service管理員__&#x200B;身分登入AEM作者服務
1. 導覽至&#x200B;__工具> Assets >處理設定檔__
1. __建立__&#x200B;新的處理設定檔，或&#x200B;__編輯__&#x200B;現有的處理設定檔
1. 點選&#x200B;__自訂__&#x200B;標籤，然後點選&#x200B;__新增__
1. 定義新服務
   + __建立中繼資料轉譯__：切換為使用中
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 這是在[部署](#deploy)期間或使用命令`aio app get-url`取得的背景工作程式的URL。 根據AEM as a Cloud Service環境，確保URL指向正確的工作區。
   + __服務引數__
      + 點選&#x200B;__新增引數__
         + 索引鍵： `colorFamily`
         + 值： `pantone`
            + 支援的值： `basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __Mime型別__
      + __包含：__ `image/jpeg`、`image/png`、`image/gif`、`image/svg`
         + 這是第三方npm模組唯一支援的MIME型別，用來衍生顏色。
      + __排除：__ `Leave blank`
1. 點選右上方的&#x200B;__儲存__
1. 將處理設定檔套用至AEM Assets資料夾（如果尚未套用）

### 更新中繼資料結構{#metadata-schema}

若要檢閱色彩中繼資料，請將影像中繼資料結構描述上的兩個新欄位，對應到背景工作填入的新中繼資料屬性。

![中繼資料結構](./assets/metadata/metadata-schema.png)

1. 在AEM作者服務中，導覽至&#x200B;__工具> Assets >中繼資料結構__
1. 導覽至&#x200B;__預設值__，選取並編輯&#x200B;__影像__，並新增唯讀表單欄位以公開產生的色彩中繼資料
1. 新增&#x200B;__單行文字__
   + __欄位標籤__： `Colors Family`
   + __對應到屬性__： `./jcr:content/metadata/wknd:colorsFamily`
   + __規則>欄位>停用編輯__：已核取
1. 新增&#x200B;__多值文字__
   + __欄位標籤__： `Colors`
   + __對應到屬性__： `./jcr:content/metadata/wknd:colors`
1. 點選右上方的&#x200B;__儲存__

## 正在處理資產

![資產詳細資料](./assets/metadata/asset-details.png)

1. 在AEM作者服務中，導覽至&#x200B;__Assets >檔案__
1. 導覽至該資料夾或子資料夾，處理設定檔將套用至
1. 上傳新影像(JPEG、PNG、GIF或SVG)至資料夾，或使用更新的[處理設定檔](#processing-profile)重新處理現有影像
1. 處理完成時，請選取資產，然後點選頂端動作列中的&#x200B;__屬性__&#x200B;以顯示其中繼資料
1. 檢閱`Colors Family`和`Colors` [中繼資料欄位](#metadata-schema)，瞭解自訂Asset Compute中繼資料背景工作回寫的中繼資料。

將色彩中繼資料寫入資產的中繼資料後，在`[dam:Asset]/jcr:content/metadata`資源上，此中繼資料會建立索引，以透過搜尋使用這些字詞提高資產探索能力，而且如果在該資產上叫用&#x200B;__DAM中繼資料回寫__&#x200B;工作流程，甚至可以將這些字詞回寫至資產的二進位檔。

### AEM Assets中的中繼資料轉譯

![AEM Assets中繼資料轉譯檔案](./assets/metadata/cqdam-metadata-rendition.png)

Asset Compute中繼資料背景工作產生的實際XMP檔案也會儲存為資產上的獨立轉譯。 系統通常不會使用此檔案，而是會使用套用至資產中繼資料節點的值，但背景工作的原始XML輸出可在AEM中使用。

## Github上的metadata-colors背景工作代碼

Github上的最終`metadata-colors/index.js`位於：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Github上的最終`test/asset-compute/metadata-colors`測試套件位於：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
