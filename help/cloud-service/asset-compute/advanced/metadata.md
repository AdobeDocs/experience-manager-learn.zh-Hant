---
title: 開發Asset compute元資料工作程式
description: 瞭解如何建立導出影像資產中最常用顏色的Asset compute元資料工作程式，並將顏色名稱寫回資產的元資料AEM。
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

# 開發Asset compute元資料工作程式

自定義Asset compute工XMP作人員可以生成(XML)資料，這些資料被發回AEM並儲存為資產上的元資料。

常見使用案例包括：

+ 與第三方系統(如PIM（產品資訊管理系統）)的整合，在該整合中，必須檢索附加元資料並將其儲存在資產上
+ 與Adobe服務（如Content和Commerce AI）整合，以通過附加的機器學習屬性來增強資產元資料
+ 從資產的二進位檔案中導出資產的元資料，並將其作為資產元資料儲存在AEMas a Cloud Service

## 你將做什麼

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我們將建立一個Asset compute元資料輔助程式，該輔助程式派生影像資產中最常用的顏色，並將顏色的名稱寫回該資產的元資料AEM。 雖然工作人員本身是基本的，但本教程使用它來探討如何使用Asset compute工作人員將元資料寫回到as a Cloud Service的AEM資產。

## asset compute元資料工作器調用的邏輯流

調用Asset compute元資料工作程式與調用 [二進位格式副本生成程式](../develop/worker.md)，主要區別是返回類型是XMP(XML)格式副本，其值也寫入資產的元資料。

asset compute工作程式執行Asset computeSDK工作程式API合同，在 `renditionCallback(...)` 函式，即概念上：

+ __輸入：__ 資產AEM的原始二進位和處理配置檔案參數
+ __輸出：__ 將XMP(XML)格式副本作為格AEM式副本保留到資產和資產的元資料

![asset compute元資料工作進程邏輯流](./assets/metadata/logical-flow.png)

1. AEM Author服務調用Asset compute元資料工作程式，提供資產 __(1a)__ 原始二進位 __(1b)__ 在「加工輪廓」中定義的任何參數。
1. asset computeSDK協調自定義Asset compute元資料工作程式的執行 `renditionCallback(...)` 函式，根據XMP資產的二進位檔案導出(XML)格式副本 __(1a)__ 和任何處理配置檔案參數 __(1b)__。
1. asset compute工作人員將XMP(XML)表示法保存到 `rendition.path`。
1. 寫入XMP到的(XML)資料 `rendition.path` 通過Asset computeSDK傳輸到AEM作者服務，並將其顯示為 __(4a)__ 文本格式副本和 __(4b)__ 永續到資產的元資料節點。

## 配置manifest.yml{#manifest}

所有Asset compute工人必須在 [manifest.yml](../develop/manifest.md)。

開啟項目 `manifest.yml` 並添加配置新工作人員的工作人員條目 `metadata-colors`。

_記住 `.yml` 對空格敏感。_

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

`function` 指向在 [下一步](#metadata-worker)。 以語義命名工作程式(例如， `actions/worker/index.js` 也許名字更準 `actions/rendition-circle/index.js`)，如 [工作人員的URL](#deploy) 並確定 [工作人員的test套件資料夾名稱](#test)。

的 `limits` 和 `require-adobe-auth` 是按每個工作程式分散配置的。 在這個工人里， `512 MB` 當代碼檢查（可能）大的二進位影像資料時分配儲存器。 另一個 `limits` 將刪除以使用預設值。

## 開發元資料工作程式{#metadata-worker}

在路徑上的Asset compute項目中建立新的元資料工作程式JavaScript檔案 [為新工作程式定義的manifest.yml](#manifest)。 `/actions/metadata-colors/index.js`

### 安裝npm模組

安裝額外的npm模組([@adobe/asset compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)。 [獲取影像顏色](https://www.npmjs.com/package/get-image-colors), [顏色名稱](https://www.npmjs.com/package/color-namer))中使用的。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元資料工作程式碼

這個工人看起來很像 [生成格式副本的工作程式](../develop/worker.md)，主要區別是XMP它向 `rendition.path` 才能被保存回AEM去。


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

## 在本地運行元資料工作程式{#development-tool}

工作代碼完成後，可以使用本地Asset compute開發工具執行。

因為我們的Asset compute項目包含兩個工人(前一個 [圓形格式](../develop/worker.md) 和 `metadata-colors` 工人), [asset compute開發工具](../develop/development-tool.md) 配置檔案定義列出兩個工作程式的執行配置檔案。 第二個輪廓定義指向新輪廓 `metadata-colors` 工人。

![XML元資料格式副本](./assets/metadata/metadata-rendition.png)

1. 從Asset compute項目的根
1. 執行 `aio app run` 啟動Asset compute開發工具
1. 在 __選擇檔案……__ 下拉，選擇 [樣本影像](../assets/samples/sample-file.jpg) 處理
1. 在第二個配置檔案定義配置中，它指向 `metadata-colors` 工作人員，更新 `"name": "rendition.xml"` 因為此工作人員生XMP成(XML)格式副本。 （可選）添加 `colorsFamily` 參數（支援的值） `basic`。 `hex`。 `html`。 `ntc`。 `pantone`。 `roygbiv`)。

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

1. 點擊 __運行__ 等待XML格式副本生成
   + 由於配置檔案定義中列出了兩個工作程式，因此兩個格式副本都將生成。 （可選）指向 [圓格式副本工作人員](../develop/worker.md) 可刪除，以避免從開發工具中執行。
1. 的 __格式副本__ 節預覽生成的格式副本。 點擊 `rendition.xml` 下載，並在VS代碼（或您最喜愛的XML/文本編輯器）中開啟它進行審閱。

## Test工作人員{#test}

可以使用 [與二進位格式副本相同的Asset compute測試框架](../test-debug/test.md)。 唯一的區別是 `rendition.xxx` test中的檔案必須是預期的(XMPXML)格式副本。

1. 在Asset compute項目中建立以下結構：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用 [示例檔案](../assets/samples/sample-file.jpg) 就像test案 `file.jpg`。
3. 將以下JSON添加到 `params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   注意 `"fmt": "xml"` 要求test套件生成 `.xml` 基於文本的格式副本。

4. 在 `rendition.xml` 的子菜單。 這可以通過以下方式獲得：
   + 通過開發工具運行test輸入檔案並保存（已驗證）XML格式副本。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 執行 `aio app test` 執行所有Asset compute套件。

### 將工作人員部署到Adobe I/O Runtime{#deploy}

要從AEM Assets調用此新元資料工作程式，必須使用以下命令將其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![aio應用程式部署](./assets/metadata/aio-app-deploy.png)

請注意，這將部署項目中的所有工作程式。 查看 [未刪除的部署說明](../deploy/runtime.md) 有關如何部署到舞台和生產工作區的資訊。

### 與處理配AEM置檔案整合{#processing-profile}

通過建立新AEM或修改調用此已部署工作程式的現有自定義處理配置檔案服務來從中調用工作程式。

![處理配置檔案](./assets/metadata/processing-profile.png)

1. 登錄AEM到as a Cloud Service作者服務 __管AEM理員__
1. 導航到 __「工具」>「資產」>「處理配置檔案」__
1. __建立__ 新的，或 __編輯__ 和現有的處理配置檔案
1. 點擊 __自定義__ 頁籤，然後點擊 __添加新__
1. 定義新服務
   + __建立元資料格式副本__:切換到活動
   + __終結點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 這是在 [部署](#deploy) 或使用命令 `aio app get-url`。 根據as a Cloud Service環境確保URL點在正確的工AEM作區。
   + __服務參數__
      + 點擊 __添加參數__
         + 金鑰: `colorFamily`
         + 值: `pantone`
            + 支援的值： `basic`。 `hex`。 `html`。 `ntc`。 `pantone`。 `roygbiv`
   + __Mime 類型__
      + __包括：__ `image/jpeg`。 `image/png`。 `image/gif`。 `image/svg`
         + 這些是第三方npm模組支援的唯一用於導出顏色的MIME類型。
      + __不包括：__ `Leave blank`
1. 點擊 __保存__ 右上角
1. 如果尚未將處理配置檔案應用到AEM Assets資料夾，請執行此操作

### 更新元資料架構{#metadata-schema}

要查看顏色元資料，請將映像的元資料架構上的兩個新欄位映射到工作人員填充的新元資料資料屬性。

![中繼資料結構](./assets/metadata/metadata-schema.png)

1. 在AEM Author服務中，導航到 __「工具」>「資產」>「元資料架構」__
1. 導航到 __預設__ 選擇和編輯 __影像__ 並添加只讀表單域，以顯示生成的顏色元資料
1. 添加 __單行文本__
   + __欄位標籤__: `Colors Family`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __規則>欄位>禁用編輯__:已選中
1. 添加 __多值文本__
   + __欄位標籤__: `Colors`
   + __映射至屬性__: `./jcr:content/metadata/wknd:colors`
1. 點擊 __保存__ 右上角

## 處理資產

![資產詳細內容](./assets/metadata/asset-details.png)

1. 在AEM Author服務中，導航到 __資產>檔案__
1. 導航到資料夾或子資料夾，處理配置檔案將應用到
1. 將新影像(JPEG、PNG、GIF或SVG)上載到資料夾，或使用已更新的影像重新處理現有影像 [處理配置檔案](#processing-profile)
1. 處理完成後，選擇資產，然後點擊 __屬性__ 在頂部操作欄中顯示其元資料
1. 查看 `Colors Family` 和 `Colors` [元資料欄位](#metadata-schema) 從自定義Asset compute元資料工作程式回寫的元資料。

將顏色元資料寫入資產的元資料中， `[dam:Asset]/jcr:content/metadata` 資源，此元資料通過搜索使用這些術語編製索引，提高了資產發現能力，如果這樣，甚至可以將其寫回資產的二進位檔案 __DAM元資料寫回__ 工作流被調用。

### 元資料格式副本在AEM Assets

![AEM Assets元資料格式副本檔案](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元XMP資料工作程式生成的實際檔案也作為離散格式副本儲存在資產上。 通常不使用此檔案，而是使用資產元資料節點的應用值，但工作程式的原始XML輸出在中可AEM用。

## Github上的元資料顏色工作代碼

決賽 `metadata-colors/index.js` 在Github上提供，網址為：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

決賽 `test/asset-compute/metadata-colors` test套房位於Github，網址為：

+ [aem指南 — wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
