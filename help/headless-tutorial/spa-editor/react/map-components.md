---
title: 將SPA元件對應至AEM元件 | AEM SPA Editor and React快速入門
description: 瞭解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓使用者在SPA SPA編輯器中對AEM元件進行動態更新，類似於傳統的AEM編寫。 您還將瞭解如何使用現成的AEM React Core Components。
feature: SPA Editor
version: Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 477
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# 將SPA元件對應至AEM元件 {#map-components}

瞭解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓使用者在SPA SPA編輯器中對AEM元件進行動態更新，類似於傳統的AEM編寫。

本章更深入地探討AEM JSON模型API，以及如何將AEM元件公開的JSON內容作為prop自動插入到React元件中。

## 目標

1. 瞭解如何將AEM元件對應至SPA元件。
1. Inspect React元件如何使用從AEM傳遞的動態屬性。
1. 瞭解如何立即使用[React AEM Core Components](https://github.com/adobe/aem-react-core-wcm-components-examples)。

## 您將建置的內容

本章會檢查提供的`Text` SPA元件如何對應至AEM `Text`元件。 React核心元件(例如`Image` SPA元件)用於SPA並在AEM中編寫。 **配置容器**&#x200B;和&#x200B;**範本編輯器**&#x200B;原則的現成功能也可用來建立外觀稍有變化的檢視。

![章節範例最終製作](./assets/map-components/final-page.png)

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 本章是[整合SPA](integrate-spa.md)章節的延續，但您只要遵循啟用SPA的AEM專案即可。

## 對應方法

基本概念是對應SPA元件至AEM元件。 AEM元件，執行伺服器端，將內容匯出為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行使用者端。 SPA元件和AEM元件之間會建立1:1對應。

![將AEM元件對應到React元件的高階概觀](./assets/map-components/high-level-approach.png)

*將AEM元件對應到React元件的高階概觀*

## Inspect文字元件

[AEM專案原型](https://github.com/adobe/aem-project-archetype)提供對應至AEM [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)的`Text`元件。 這是&#x200B;**content**&#x200B;元件的範例，它從AEM轉譯&#x200B;*內容*。

讓我們瞭解元件的運作方式。

### Inspect JSON模型

1. 在跳入SPA程式碼之前，請務必瞭解AEM提供的JSON模型。 導覽至[核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html)，並檢視文字元件的頁面。 核心元件庫提供所有AEM核心元件的範例。
1. 選取其中一個範例的&#x200B;**JSON**&#x200B;標籤：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性： `text`、`richText`和`:type`。

   `:type`是保留屬性，列出AEM元件的`sling:resourceType` （或路徑）。 `:type`的值是用來將AEM元件對應到SPA元件的值。

   `text`和`richText`是公開給SPA元件的其他屬性。

1. 在[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)檢視JSON輸出。 您應該能夠找到類似以下的專案：

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect文字SPA元件

1. 在您選擇的IDE中，開啟SPA的AEM專案。 展開`ui.frontend`模組並開啟`ui.frontend/src/components/Text/Text.js`下的檔案`Text.js`。

1. 我們將檢查的第一個區域是位於~第40行的`class Text`：

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

   `Text`是標準React元件。 元件使用`this.props.richText`來決定要呈現的內容是RTF文字還是純文字。 實際使用的「內容」來自`this.props.text`。

   為避免潛在的XSS攻擊，在使用[danguallySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)轉譯內容之前，RTF會透過`DOMPurify`逸出。 在練習的前面重新叫用JSON模型中的`richText`和`text`屬性。

1. 接下來，開啟`ui.frontend/src/components/import-components.js`檢視~第86行的`TextEditConfig`：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述程式碼負責決定何時在AEM製作環境中呈現預留位置。 如果`isEmpty`方法傳回&#x200B;**true**，則會轉譯預留位置。

1. 最後檢視~第94行上的`MapTo`呼叫：

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo`由AEM SPA編輯器JS SDK (`@adobe/aem-react-editable-components`)提供。 路徑`wknd-spa-react/components/text`代表AEM元件的`sling:resourceType`。 此路徑與先前觀察到的JSON模型公開的`:type`相符。 `MapTo`負責剖析JSON模型回應，並將正確的值作為`props`傳遞給SPA元件。

   您可以在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`找到AEM `Text`元件定義。

## 使用React核心元件

[AEM WCM元件 — React Core實施](https://github.com/adobe/aem-react-core-wcm-components-base)和[AEM WCM元件 — Spa編輯器 — React Core實施](https://github.com/adobe/aem-react-core-wcm-components-spa)。 這是一組可重複使用的UI元件，對應至現成可用的AEM元件。 大部分專案都可重複使用這些元件，作為自身實施的起點。

1. 在專案程式碼中，開啟位於`ui.frontend/src/components`的檔案`import-components.js`。
此檔案會匯入所有對應至SPA元件的AEM元件。 鑑於SPA Editor實作的動態性質，我們必須明確參考任何繫結至AEM可編寫元件的SPA元件。 這可讓AEM作者選擇在應用程式中隨處使用元件。
1. 下列匯入陳述式包含寫入專案中的SPA元件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. 有幾個來自`@adobe/aem-core-components-react-spa`和`@adobe/aem-core-components-react-base`的其他`imports`。 這些匯入React Core元件，並用於目前專案。 然後使用`MapTo`將這些元件對應至專案特定的AEM元件，就像之前的`Text`元件範例一樣。

### 更新AEM原則

原則是AEM範本的一項功能，可讓開發人員和進階使用者精細控制可使用哪些元件。 React核心元件包含在SPA程式碼中，但必須先透過原則啟用，才能在應用程式中使用。

1. 從AEM開始畫面導覽至&#x200B;**工具** > **範本** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**。

1. 選取並開啟&#x200B;**SPA頁面**&#x200B;範本以進行編輯。

1. 選取&#x200B;**配置容器**，然後按一下它的&#x200B;**原則**&#x200B;圖示以編輯原則：

   ![配置容器原則](assets/map-components/edit-spa-page-template.png)

1. 在&#x200B;**允許的元件** > **WKND SPA React - Content** >檢查&#x200B;**影像**、**Teaser**&#x200B;和&#x200B;**標題**。

   ![可用的已更新元件](assets/map-components/update-components-available.png)

   在&#x200B;**預設元件** > **新增對應**&#x200B;下並選擇&#x200B;**影像 — WKND SPA React - Content**&#x200B;元件：

   ![設定預設元件](./assets/map-components/default-components.png)

   輸入`image/*`的&#x200B;**mime型別**。

   按一下&#x200B;**完成**&#x200B;以儲存原則更新。

1. 在&#x200B;**配置容器**&#x200B;中，按一下&#x200B;**文字**&#x200B;元件的&#x200B;**原則**&#x200B;圖示。

   建立名稱為&#x200B;**WKND SPA Text**&#x200B;的新原則。 在&#x200B;**外掛程式** > **格式** >核取所有方塊以啟用其他格式選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在&#x200B;**外掛程式** > **段落樣式** >下方，勾選方塊以&#x200B;**啟用段落樣式**：

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下&#x200B;**完成**&#x200B;以儲存原則更新。

### 作者內容

1. 瀏覽至&#x200B;**首頁** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

1. 您現在應該可以在頁面上使用其他元件&#x200B;**Image**、**Teaser**&#x200B;和&#x200B;**Title**。

   ![其他元件](assets/map-components/additional-components.png)

1. 您也應該能夠編輯`Text`元件，並在&#x200B;**全熒幕**&#x200B;模式中新增其他段落樣式。

   ![全熒幕RTF編輯](assets/map-components/full-screen-rte.png)

1. 您也應該能夠從&#x200B;**資產尋找器**&#x200B;拖放影像：

   ![拖放影像](assets/map-components/drag-drop-image.png)

1. 使用&#x200B;**Title**&#x200B;和&#x200B;**Teaser**&#x200B;元件進行實驗。

1. 透過[AEM Assets](http://localhost:4502/assets.html/content/dam)新增您自己的影像，或安裝標準[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)完成的程式碼基底。 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)包含許多可在WKND SPA上重複使用的影像。 可以使用[AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)來安裝封裝。

   ![封裝管理員安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect配置容器

AEM SPA編輯器SDK會自動提供對&#x200B;**配置容器**&#x200B;的支援。 名稱所指示的&#x200B;**配置容器**&#x200B;是&#x200B;**容器**&#x200B;元件。 容器元件是接受JSON結構的元件，這些結構代表&#x200B;*其他*&#x200B;個元件並動態具現化它們。

讓我們進一步檢查配置容器。

1. 在瀏覽器中導覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 回應式格線](./assets/map-components/responsive-grid-modeljson.png)

   **配置容器**&#x200B;元件有`wcm/foundation/components/responsivegrid`的`sling:resourceType`，而且可由SPA編輯器使用`:type`屬性來辨識，就像`Text`和`Image`元件一樣。

   SPA編輯器也提供使用[配置模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新調整元件大小的相同功能。

2. 返回[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 新增其他&#x200B;**影像**&#x200B;元件，然後嘗試使用&#x200B;**配置**&#x200B;選項重新調整其大小：

   ![使用版面配置模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，並觀察作為JSON一部分的`columnClassNames`：

   ![欄類別名稱](./assets/map-components/responsive-grid-classnames.png)

   類別名稱`aem-GridColumn--default--4`表示元件應以12欄格線為4欄寬。 如需[回應式格線的詳細資訊，請參閱此處](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回IDE，在`ui.apps`模組中有一個在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`定義的使用者端程式庫。 開啟檔案`less/grid.less`。

   此檔案決定&#x200B;**配置容器**&#x200B;使用的中斷點（`default`、`tablet`和`phone`）。 此檔案旨在根據專案規格自訂。 目前中斷點設定為`1200px`和`768px`。

5. 您應該能夠使用`Text`元件的回應式功能和更新的RTF原則來編寫如下的檢視：

   ![章節範例最終製作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何將SPA元件對應至AEM Components，並且您已使用React Core Components。 您也有機會探索&#x200B;**配置容器**&#x200B;的回應式功能。

### 後續步驟 {#next-steps}

[導覽及路由](navigation-routing.md) — 瞭解如何使用SPA編輯器SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導覽是使用React Router和React Core Components來實施。

## （額外優點）將組態保留至原始檔控制 {#bonus-configs}

在許多情況下，尤其是在AEM專案開始時，將設定（例如範本和相關內容原則）保留到原始檔控制中很有價值。 這可確保所有開發人員都針對相同的內容和設定集，且可確保環境之間有額外的一致性。 一旦專案達到一定的成熟度，管理範本的實務就可以交給特殊的超級使用者群組。

後續幾個步驟將使用Visual Studio Code IDE和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)進行，但可能會使用任何工具和您已設定為從AEM本機執行個體&#x200B;**提取**&#x200B;或&#x200B;**匯入**&#x200B;內容的任何IDE來執行。

1. 在Visual Studio Code IDE中，確定您已透過Marketplace擴充功能安裝&#x200B;**VSCode AEM Sync**：

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. 展開專案總管中的&#x200B;**ui.content**&#x200B;模組，並導覽至`/conf/wknd-spa-react/settings/wcm/templates`。

3. **按一下滑鼠右鍵** `templates`資料夾並選取&#x200B;**從AEM伺服器匯入**：

   ![VSCode匯入範本](./assets/map-components/import-aem-servervscode.png)

4. 重複步驟以匯入內容，但選取位於`/conf/wknd-spa-react/settings/wcm/templates/policies`的&#x200B;**原則**&#x200B;資料夾。

5. Inspect位於`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`檔案。

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

   `filter.xml`檔案負責識別隨套件安裝的節點路徑。 請注意每個篩選器上的`mode="merge"`，這表示現有內容將不會被修改，只會新增內容。 由於內容作者可能正在更新這些路徑，因此程式碼部署&#x200B;**不會**&#x200B;覆寫內容非常重要。 請參閱[FileVault檔案](https://jackrabbit.apache.org/filevault/filter.html)，以取得使用篩選元素的詳細資訊。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`與`ui.apps/src/main/content/META-INF/vault/filter.xml`，瞭解每個模組所管理的不同節點。

## （額外練習）建立自訂影像元件 {#bonus-image}

React Core元件已提供SPA影像元件。 不過，如果您需要額外的練習，請建立您自己的React實作，該實作對應到AEM [影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)。 `Image`元件是&#x200B;**content**&#x200B;元件的另一個範例。

### Inspect和JSON

在跳入SPA程式碼之前，請檢查AEM提供的JSON模型。

1. 導覽至核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html)中的[影像範例。

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   `src`、`alt`和`title`的屬性是用來填入SPA `Image`元件。

   >[!NOTE]
   >
   > 有其他公開的影像屬性(`lazyEnabled`， `widths`)可讓開發人員建立最適化且延遲載入的元件。 此教學課程中建置的元件非常簡單，**不會**&#x200B;使用這些進階屬性。

### 實作影像元件

1. 接下來，在`ui.frontend/src/components`下建立名為`Image`的新資料夾。
1. 在`Image`資料夾下方建立名為`Image.js`的新檔案。

   ![Image.js檔案](./assets/map-components/image-js-file.png)

1. 將下列`import`陳述式新增至`Image.js`：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 然後新增`ImageEditConfig`以決定何時在AEM中顯示預留位置：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未設定`src`屬性，則會顯示預留位置。

1. 下一個實作`Image`類別：

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

   上述程式碼將根據JSON模型傳入的prop `src`、`alt`和`title`呈現`<img>`。

1. 新增`MapTo`程式碼以將React元件對應至AEM元件：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   請注意，字串`wknd-spa-react/components/image`對應至`ui.apps`中AEM元件的位置： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

1. 在相同目錄中建立名為`Image.css`的新檔案，並新增下列專案：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. 在`Image.js`中，在`import`陳述式下方的頂端加入檔案參考：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. 開啟檔案`ui.frontend/src/components/import-components.js`並新增新`Image`元件的參考：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. 在`import-components.js`中註解React核心元件影像：

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   這將確保改用我們的自訂影像元件。

1. 從專案的根使用Maven將SPA程式碼部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在AEM中Inspect SPA。 頁面上的任何影像元件都應繼續運作。 Inspect呈現的輸出結果，您應該會看到自訂影像元件的標籤，而不是React核心元件。

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

   這是擴充及實作您自己的元件的絕佳簡介。
