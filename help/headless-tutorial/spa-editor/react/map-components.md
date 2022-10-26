---
title: 將SPA元件對應至AEM元件 |開始使用AEM SPA Editor and React
description: 了解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。 您也將學習如何使用現成可用的AEM React核心元件。
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2257'
ht-degree: 0%

---

# 將SPA元件對應至AEM元件 {#map-components}

了解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。

本章更深入探討AEM JSON模型API，以及AEM元件公開的JSON內容如何能自動插入React元件作為Prop。

## 目標

1. 了解如何將AEM元件對應至SPA元件。
1. Inspect React元件如何使用從AEM傳遞的動態屬性。
1. 了解如何立即使用 [React AEM核心元件](https://github.com/adobe/aem-react-core-wcm-components-examples).

## 您將建置的

本章檢查提供的 `Text` SPA元件已對應至AEM `Text`元件。 回應核心元件，如 `Image` SPA元件用於SPA中，並在AEM中製作。 的現成功能 **版面容器** 和 **範本編輯器** 策略還可用於建立外觀更多樣的視圖。

![章節範例最終製作](./assets/map-components/final-page.png)

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment). 本章是 [整合SPA](integrate-spa.md) 不過，後續需要的章節是啟用SPA的AEM專案。

## 映射方法

基本概念是將SPA元件對應至AEM元件。 AEM元件、執行伺服器端，將內容匯出為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行用戶端。 系統會建立SPA元件與AEM元件之間的1:1對應。

![將AEM元件對應至React元件的概觀](./assets/map-components/high-level-approach.png)

*將AEM元件對應至React元件的概觀*

## Inspect the Text Component

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 提供 `Text` 對應至AEM的元件 [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 這是 **內容** 元件，在中呈現 *內容* 從AEM。

讓我們查看元件的運作方式。

### Inspect JSON模型

1. 在跳入SPA程式碼之前，請務必了解AEM提供的JSON模型。 導覽至 [核心元件程式庫](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 並查看文本元件的頁面。 核心元件程式庫提供所有AEM核心元件的範例。
1. 選取 **JSON** 標籤中的一個示例：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性： `text`, `richText`，和 `:type`.

   `:type` 是保留的屬性，它列出 `sling:resourceType` （或路徑）。 的值 `:type` 用來將AEM元件對應至SPA元件。

   `text` 和 `richText` 是公開給SPA元件的其他屬性。

1. 在檢視JSON輸出 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 您應該可以找到類似以下的項目：

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect the Text SPA元件

1. 在您選擇的IDE中，開啟AEM的SPA專案。 展開 `ui.frontend` 模組並開啟檔案 `Text.js` 在 `ui.frontend/src/components/Text/Text.js`.

1. 我們首先要檢查的是 `class Text` 在第40行：

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` 是標準React元件。 元件使用 `this.props.richText` 以判斷要呈現的內容將是RTF或純文字。 實際使用的「內容」來自 `this.props.text`.

   為避免潛在的XSS攻擊，RTF會透過逸出 `DOMPurify` 使用之前 [危險的SetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 來呈現內容。 回想 `richText` 和 `text` 屬性。

1. 下一個，開啟 `ui.frontend/src/components/import-components.js` 看看 `TextEditConfig` 在第86行：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述程式碼負責決定何時在AEM製作環境中呈現預留位置。 若 `isEmpty` 方法傳回 **true** 然後呈現佔位符。

1. 最後，請查看 `MapTo` ~94號線呼叫：

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` 由AEM SPA Editor JS SDK提供(`@adobe/aem-react-editable-components`)。 路徑 `wknd-spa-react/components/text` 代表 `sling:resourceType` 的AEM元件。 此路徑符合 `:type` 由先前觀察到的JSON模型公開。 `MapTo` 負責剖析JSON模型回應，並將正確值傳入 `props` 至SPA元件。

   您可以找到AEM `Text` 元件定義 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## 使用React核心元件

[AEM WCM元件 — React核心實作](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM WCM元件 — Spa編輯器 — React Core實作](https://github.com/adobe/aem-react-core-wcm-components-spa). 這些是一組可重複使用的UI元件，會對應至現成可用的AEM元件。 大部分專案都可以重新使用這些元件，作為自己實作的起點。

1. 在專案程式碼中開啟檔案 `import-components.js` at `ui.frontend/src/components`.
此檔案會匯入對應至AEM元件的所有SPA元件。 鑑於SPA Editor實作的動態性，我們必須明確參考任何系結至AEM可製作元件的SPA元件。 這可讓AEM作者選擇在應用程式中使用元件（無論其需要的位置）。
1. 下列匯入陳述式包含專案中撰寫的SPA元件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. 還有幾個 `imports` 從 `@adobe/aem-core-components-react-spa` 和 `@adobe/aem-core-components-react-base`. 這些元件將匯入React核心元件，並可在目前專案中使用。 這些元件接著會對應至專案特定的AEM元件，使用 `MapTo`，就像 `Text` 元件範例。

### 更新AEM原則

原則是AEM範本的功能，可讓開發人員和高級使用者對可使用的元件進行精細控制。 SPA程式碼中包含React核心元件，但必須先透過原則啟用這些元件，才能在應用程式中使用。

1. 從AEM開始畫面導覽至 **工具** > **範本** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. 選取並開啟 **SPA頁面** 範本。

1. 選取 **版面容器** 按一下 **原則** 表徵圖可編輯策略：

   ![配置容器策略](assets/map-components/edit-spa-page-template.png)

1. 在 **允許的元件** > **WKND SPA React — 內容** >檢查 **影像**, **Teaser**，和 **標題**.

   ![可用的更新元件](assets/map-components/update-components-available.png)

   在 **預設元件** > **新增對應** 並選擇 **影像 — WKND SPA React — 內容** 元件：

   ![設定預設元件](./assets/map-components/default-components.png)

   輸入 **mime類型** of `image/*`.

   按一下 **完成** 以保存策略更新。

1. 在 **版面容器** 按一下 **原則** 圖示 **文字** 元件。

   建立新策略，命名為 **WKND SPA文字**. 在 **外掛程式** > **格式** >選中所有框以啟用其他格式選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在 **外掛程式** > **段落樣式** >核取方塊 **啟用段落樣式**:

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下 **完成** 以保存策略更新。

### 製作內容

1. 導覽至 **首頁** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. 您現在應該可以使用其他元件 **影像**, **Teaser**，和 **標題** 頁面上。

   ![其他元件](assets/map-components/additional-components.png)

1. 您也應該可以編輯 `Text` 元件和在 **全螢幕** 模式。

   ![全螢幕RTF編輯](assets/map-components/full-screen-rte.png)

1. 您也應該可以從 **資產尋找器**:

   ![拖放影像](assets/map-components/drag-drop-image.png)

1. 體驗 **標題** 和 **Teaser** 元件。

1. 透過 [AEM Assets](http://localhost:4502/assets.html/content/dam) 或安裝標準程式碼庫的完整版 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest). 此 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 包含許多可在WKND SPA上重複使用的影像。 可使用 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect版面容器

支援 **版面容器** 由AEM SPA Editor SDK自動提供。 此 **版面容器**，如名稱所示，為 **容器** 元件。 容器元件是接受JSON結構的元件，代表 *其他* 元件，並以動態方式具現化。

讓我們進一步檢查「版面容器」。

1. 在瀏覽器中導覽至 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 回應式格線](./assets/map-components/responsive-grid-modeljson.png)

   此 **版面容器** 元件具有 `sling:resourceType` of `wcm/foundation/components/responsivegrid` 和是由SPA編輯器識別，使用 `:type` 屬性，就像 `Text` 和 `Image` 元件。

   使用重新調整元件大小的功能相同 [版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 可與SPA編輯器搭配使用。

2. 返回 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 新增其他 **影像** 並嘗試使用 **版面** 選項：

   ![使用「佈局」模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 並觀察 `columnClassNames` 作為JSON的一部分：

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名 `aem-GridColumn--default--4` 指出元件應寬4列（基於12列網格）。 有關 [可在此處找到回應式格線](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. 返回到IDE，並在 `ui.apps` 模組有在 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. 開啟檔案 `less/grid.less`.

   此檔案確定斷點(`default`, `tablet`，和 `phone`)使用 **版面容器**. 此檔案旨在根據項目規格進行定制。 當前斷點設定為 `1200px` 和 `768px`.

5. 您應該可以使用 `Text` 製作檢視的元件，如下所示：

   ![章節範例最終製作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜您，您已了解如何將SPA元件對應至AEM元件，且已使用React核心元件。 您也可以探索 **版面容器**.

### 後續步驟 {#next-steps}

[導航和路由](navigation-routing.md)  — 了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router和React Core Components實現的。

## （額外獎勵）保留配置以進行源控制 {#bonus-configs}

在許多情況下，尤其是在AEM專案開始時，最好將設定（例如範本和相關內容原則）保留到原始碼控制。 這可確保所有開發人員針對相同的內容和設定組合工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟程度，管理模板的做法就可以交給特定的一組電源用戶。

接下來的幾個步驟將使用Visual Studio Code IDE和 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 但可以使用任何工具和配置為 **提取** 或 **匯入** 來自本機AEM例項的內容。

1. 在Visual Studio代碼IDE中，確保您 **VSCode AEM同步** 透過Marketplace擴充功能安裝：

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展開 **ui.content** 模組，並導覽至 `/conf/wknd-spa-react/settings/wcm/templates`.

3. **按一下右鍵** the `templates` 資料夾和選取 **從AEM伺服器匯入**:

   ![VSCode匯入範本](./assets/map-components/import-aem-servervscode.png)

4. 重複匯入內容的步驟，但選取 **原則** 位於 `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect `filter.xml` 位於 `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   此 `filter.xml` 檔案負責標識隨軟體包一起安裝的節點的路徑。 請注意 `mode="merge"` 在指出不會修改現有內容的每個篩選器上，只會新增新內容。 由於內容作者可能更新這些路徑，因此程式碼部署必須確實更新 **not** 覆寫內容。 請參閱 [FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html) 以取得使用篩選元素的詳細資訊。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解每個模組管理的不同節點。

## （額外練習）建立自訂影像元件 {#bonus-image}

React核心元件已提供SPA影像元件。 不過，如果您想要其他實務，請建立對應至AEM的專屬React實作 [影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 此 `Image` 元件是 **內容** 元件。

### Inspect JSON

在跳入SPA程式碼之前，請檢查AEM提供的JSON模型。

1. 導覽至 [核心元件程式庫中的影像範例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   屬性 `src`, `alt`，和 `title` 用來填入SPA `Image` 元件。

   >[!NOTE]
   >
   > 有其他已公開的影像屬性(`lazyEnabled`, `widths`)，讓開發人員建立最適化和延遲載入元件。 本教學課程中建置的元件簡單，而且是 **not** 使用這些進階屬性。

### 實作影像元件

1. 接下來，建立名為 `Image` 在 `ui.frontend/src/components`.
1. 在 `Image` 資料夾建立名為的新檔案 `Image.js`.

   ![Image.js檔案](./assets/map-components/image-js-file.png)

1. 新增下列項目 `import` 報表 `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 然後新增 `ImageEditConfig` 若要決定何時在AEM中顯示預留位置：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   預留位置會顯示 `src` 屬性未設定。

1. 下一步實作 `Image` 類別：

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   上述程式碼會將 `<img>` 根據prop `src`, `alt`，和 `title` 由JSON模型傳入。

1. 新增 `MapTo` 將React元件對應至AEM元件的程式碼：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   記下字串 `wknd-spa-react/components/image` 與AEM元件在 `ui.apps` at: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. 建立名為 `Image.css` 在相同目錄中，並新增下列項目：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. 在 `Image.js` 在 `import` 陳述式：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. 開啟檔案 `ui.frontend/src/components/import-components.js` 並將引用添加到新 `Image` 元件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. 在 `import-components.js` 注釋掉React核心元件映像：

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   這可確保改用自訂影像元件。

1. 從專案的根目錄，使用Maven將SPA程式碼部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect AEM中的SPA。 頁面上的任何影像元件應可繼續運作。 Inspect已轉譯的輸出，您應該會看到自訂影像元件的標籤，而非React核心元件。

   *自訂影像元件標籤*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *React核心元件影像標籤*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   這是擴充和實作您自己元件的簡介。
