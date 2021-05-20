---
title: 將SPA元件對應至AEM元件 |開始使用AEM SPA Editor and React
description: 了解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。
sub-product: Sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 1%

---


# 將SPA元件對應至AEM元件{#map-components}

了解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。

本章更深入探討AEM JSON模型API，以及AEM元件公開的JSON內容如何能自動插入React元件作為Prop。

## 目標

1. 了解如何將AEM元件對應至SPA元件。
2. 了解&#x200B;**Container**&#x200B;元件與&#x200B;**Content**&#x200B;元件之間的差異。
3. 建立新的React元件，將其對應至現有AEM元件。

## 您將建置的

本章將檢查提供的`Text` SPA元件如何對應至AEM `Text`元件。 將建立新的`Image` SPA元件，可在SPA中使用並在AEM中製作。 **佈局容器**&#x200B;和&#x200B;**模板編輯器**&#x200B;策略的現成功能也將用於建立外觀更多樣的視圖。

![章節範例最終製作](./assets/map-components/final-page.png)

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上檢視完成的程式碼，或切換至分支`React/map-components-solution`在本機檢出程式碼。

## 映射方法

基本概念是將SPA元件對應至AEM元件。 AEM元件、執行伺服器端，將內容匯出為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行用戶端。 系統會建立SPA元件與AEM元件之間的1:1對應。

![將AEM元件對應至React元件的概觀](./assets/map-components/high-level-approach.png)

*將AEM元件對應至React元件的概觀*

## Inspect the Text Component

[AEM專案原型](https://github.com/adobe/aem-project-archetype)提供對應至AEM [文字元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)的`Text`元件。 這是&#x200B;**content**&#x200B;元件的範例，其會從AEM轉譯&#x200B;*content*。

讓我們查看元件的運作方式。

### Inspect JSON模型

1. 在跳入SPA程式碼之前，請務必了解AEM提供的JSON模型。 導覽至[核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)並檢視文字元件的頁面。 核心元件程式庫提供所有AEM核心元件的範例。
2. 選取其中一個範例的&#x200B;**JSON**&#x200B;標籤：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性：`text`、`richText`和`:type`。

   `:type` 是保留屬性，會列 `sling:resourceType` 出AEM元件的（或路徑）。`:type`值可用來將AEM元件對應至SPA元件。

   `text` 和 `richText` 是將公開給SPA元件的其他屬性。

### Inspect the Text元件

1. 開啟新終端機，並導覽至專案內的`ui.frontend`資料夾。 執行`npm install`，然後執行`npm start`以啟動&#x200B;**webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   `ui.frontend`模組目前已設定為使用[模擬JSON模型](./integrate-spa.md#mock-json)。

2. 您應會看到新的瀏覽器視窗開啟至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![具有模擬內容的Webpack開發伺服器](./assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 展開`ui.frontend`模組，並在`ui.frontend/src/components/Text/Text.js`下開啟檔案`Text.js`:

   ![Text.js React元件原始碼](./assets/map-components/vscode-ide-text-js.png)

4. 我們要檢查的第一個區域是~40行的`class Text` :

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

   `Text` 是標準React元件。元件使用`this.props.richText`來判斷要呈現的內容將為RTF或純文字。 使用的實際「content」來自`this.props.text`。 為避免潛在的XSS攻擊，在使用[allisalSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)來呈現內容之前，RTF會透過`DOMPurify`逸出。 在本練習前面的JSON模型中，回想一下`richText`和`text`屬性。

5. 接下來，查看`TextEditConfig`的~29行：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述程式碼負責決定何時在AEM製作環境中呈現預留位置。 如果`isEmpty`方法傳回&#x200B;**true**，則會呈現預留位置。

6. 最後，查看~line 62處的`MapTo`呼叫：

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 由AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)提供。路徑`wknd-spa-react/components/text`代表AEM元件的`sling:resourceType`。 此路徑與先前觀察到的JSON模型公開的`:type`相符。 `MapTo` 會負責剖析JSON模型回應，並將正確值傳 `props` 遞至SPA元件。

   您可以在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`找到AEM `Text`元件定義。

7. 修改`ui.frontend/public/mock-content/mock.model.json`處的`mock.model.json`檔案以進行實驗。 ~line 62會更新第一個`Text`值，以使用&#x200B;**`H1`**&#x200B;和&#x200B;**`u`**&#x200B;標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   導覽至[http://localhost:3000](http://localhost:3000)以查看效果：

   ![更新的文字模型](./assets/map-components/updated-text-model.png)

   嘗試在&#x200B;**true** / **false**&#x200B;之間切換`richText`屬性，以查看執行中的轉譯邏輯。

8. Inspect `Text.scss` at `ui.frontend/src/components/Text/Text.scss`。

   此檔案由本章的起始程式碼庫添加，並使用前一章中添加的[Sass](https://sass-lang.com/)功能。 記下從`ui.frontend/src/styles/_variables.scss`引用的變數。

## 建立影像元件

接下來，建立映射到AEM [Image元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)的`Image` React元件。 `Image`元件是&#x200B;**content**&#x200B;元件的另一個示例。

### Inspect JSON

在跳入SPA程式碼之前，請檢查AEM提供的JSON模型。

1. 導覽至核心元件程式庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)中的[影像範例。

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   `src`、`alt`和`title`的屬性將用於填入SPA `Image`元件。

   >[!NOTE]
   >
   > 有其他公開的影像屬性(`lazyEnabled`, `widths`)可讓開發人員建立最適化和延遲載入元件。 本教學課程中建置的元件將會很簡單，並且&#x200B;**not**&#x200B;會使用這些進階屬性。

2. 返回IDE，在`ui.frontend/public/mock-content/mock.model.json`處開啟`mock.model.json`。 由於這是專案的全新元件，因此我們需要「模擬」影像JSON。

   在~70行中，為`image`模型（別忘了第二個`text_23828680`後面的尾隨逗號`,`）新增JSON項目，並更新`:itemsOrder`陣列。

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   專案包含將與&#x200B;**webpack-dev-server**&#x200B;搭配使用的`/mock-content/adobestock-140634652.jpeg`範例影像。

   您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)檢視完整[mock.model.json。

### 實作影像元件

1. 接下來，在`ui.frontend/src/components`下建立名為`Image`的新資料夾。
2. 在`Image`資料夾下面建立名為`Image.js`的新檔案。

   ![Image.js檔案](./assets/map-components/image-js-file.png)

3. 將下列`import`陳述式新增至`Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 然後新增`ImageEditConfig`以判斷何時要在AEM中顯示預留位置：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未設定`src`屬性，則佔位符將顯示。

5. 下一步實作`Image`類別：

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

   上述程式碼會根據JSON模型傳入的prop `src`、`alt`和`title`轉譯`<img>`。

6. 新增`MapTo`程式碼，將React元件對應至AEM元件：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   請注意，字串`wknd-spa-react/components/image`對應於`ui.apps`中AEM元件的位置：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

7. 在相同目錄中建立名為`Image.scss`的新檔案，並添加以下內容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. 在`Image.js`中，在`import`陳述式下方的頂部添加對檔案的引用：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)檢視已完成的[Image.js。

9. 開啟檔案`ui.frontend/src/components/import-components.js`並將引用添加到新`Image`元件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. 如果尚未啟動，請啟動&#x200B;**webpack-dev-server**。 導覽至[http://localhost:3000](http://localhost:3000)，您應該會看到影像呈現：

   ![模擬中新增的影像](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **獎金挑戰**:在中實作新方 `Image.js` 法，將的值 `this.props.title` 顯示為影像下方的註解。

## 更新AEM中的原則

`Image`元件僅顯示在&#x200B;**webpack-dev-server**&#x200B;中。 接著，將更新的SPA部署至AEM並更新範本原則。

1. 停止&#x200B;**webpack-dev-server**，並從項目根目錄中，使用Maven技能將更改部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從「AEM開始」畫面，導覽至「**工具** > **範本** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**」。

   選擇並編輯&#x200B;**SPA Page**:

   ![編輯SPA頁面範本](./assets/map-components/edit-spa-page-template.png)

3. 選擇&#x200B;**佈局容器**&#x200B;並按一下其&#x200B;**policy**&#x200B;表徵圖以編輯策略：

   ![佈局容器策略](./assets/map-components/layout-container-policy.png)

4. 在「**允許的元件**」>「**WKND SPA React — 內容**」下，檢查&#x200B;**Image**&#x200B;元件：

   ![已選取影像元件](./assets/map-components/check-image-component.png)

   在「**預設元件** > **添加映射**」下，選擇&#x200B;**影像 — WKND SPA React — 內容**&#x200B;元件：

   ![設定預設元件](./assets/map-components/default-components.png)

   輸入&#x200B;**mime類型**(`image/*`)。

   按一下&#x200B;**Done**&#x200B;以保存策略更新。

5. 在&#x200B;**佈局容器**&#x200B;中，按一下&#x200B;**Text**&#x200B;元件的&#x200B;**policy**&#x200B;表徵圖：

   ![文本元件策略表徵圖](./assets/map-components/edit-text-policy.png)

   建立名為&#x200B;**WKND SPA Text**&#x200B;的新策略。 在&#x200B;**Plugins** > **Formatting**&#x200B;下，選中所有框以啟用其他格式選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在&#x200B;**Plugins** > **段落樣式** >下，選中該框以&#x200B;**啟用段落樣式**:

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下&#x200B;**Done**&#x200B;以保存策略更新。

6. 導覽至&#x200B;**Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

   您也應該可以編輯`Text`元件，並在&#x200B;**全螢幕**&#x200B;模式中新增其他段落樣式。

   ![全螢幕RTF編輯](assets/map-components/full-screen-rte.png)

7. 您也應該能夠從&#x200B;**資產尋找器**&#x200B;拖放影像：

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過[AEM Assets](http://localhost:4502/assets.html/content/dam)新增您自己的影像，或安裝標準[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)的已完成程式碼基底。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)包含許多可在WKND SPA上重新使用的影像。 可使用[AEM套件管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect版面容器

AEM SPA編輯器SDK會自動支援&#x200B;**版面容器**。 如名稱所示，**「佈局容器」**&#x200B;是&#x200B;**容器**&#x200B;元件。 容器元件是接受JSON結構的元件，這些結構代表&#x200B;*other*&#x200B;元件並以動態方式具現化。

讓我們進一步檢查「版面容器」。

1. 在瀏覽器中導覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 回應式格線](./assets/map-components/responsive-grid-modeljson.png)

   **版面容器**&#x200B;元件具有`wcm/foundation/components/responsivegrid`的`sling:resourceType`，且由SPA編輯器使用`:type`屬性來識別，就像`Text`和`Image`元件一樣。

   SPA編輯器提供使用[Layout Mode](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新調整元件大小的相同功能。

2. 返回[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 新增其他&#x200B;**Image**&#x200B;元件，並嘗試使用&#x200B;**Layout**&#x200B;選項重新調整元件大小：

   ![使用「佈局」模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，並在JSON中觀察`columnClassNames`:

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名`aem-GridColumn--default--4`指示元件應基於12列網格為4列寬。 如需[回應式格線的詳細資訊，請參閱此處](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回到IDE，在`ui.apps`模組中，有一個在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`定義的客戶端庫。 開啟檔案`less/grid.less`。

   此檔案確定&#x200B;**佈局容器**&#x200B;使用的斷點（`default`、`tablet`和`phone`）。 此檔案旨在根據項目規格進行定制。 目前斷點設定為`1200px`和`650px`。

5. 您應該可以使用`Text`元件的回應式功能和更新的RTF原則來製作如下所示的檢視：

   ![章節範例最終製作](./assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜您，您已學習如何將SPA元件對應至AEM元件，且已實作新的`Image`元件。 您也可以探索&#x200B;**版面容器**&#x200B;的回應功能。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上檢視完成的程式碼，或切換至分支`React/map-components-solution`在本機檢出程式碼。

### 後續步驟{#next-steps}

[導覽與路由](navigation-routing.md)  — 使用SPA Editor SDK對應至AEM頁面，可了解SPA中的多個檢視如何受到支援。動態導航是使用React Router實現的，並添加到現有的Header元件中。

## 額外 — 保留原始碼控制的配置 {#bonus}

在許多情況下，尤其是在AEM專案開始時，最好將設定（例如範本和相關內容原則）保留到原始碼控制。 這可確保所有開發人員針對相同的內容和設定組合工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟程度，管理模板的做法就可以交給特定的一組電源用戶。

接下來的幾個步驟將使用Visual Studio Code IDE和[VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)進行，但可能使用您已配置為&#x200B;**pull**&#x200B;或&#x200B;**從AEM的本地實例導入**&#x200B;內容的任何工具和任何IDE來執行。

1. 在Visual Studio代碼IDE中，確保已通過Marketplace擴展安裝&#x200B;**VSCode AEM同步**:

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展開「專案總管」中的&#x200B;**ui.content**&#x200B;模組，並導覽至`/conf/wknd-spa-react/settings/wcm/templates`。

3. **按一下右** 鍵資料 `templates` 夾，然後選 **取「從AEM伺服器匯入」**:

   ![VSCode匯入範本](./assets/map-components/import-aem-servervscode.png)

4. 重複匯入內容的步驟，但選取位於`/conf/wknd-spa-react/settings/wcm/templates/policies`的&#x200B;**policys**&#x200B;資料夾。

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

   `filter.xml`檔案負責標識將隨軟體包一起安裝的節點的路徑。 請注意每個篩選器上的`mode="merge"` ，指出不會修改現有內容，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此程式碼部署必須&#x200B;**不**&#x200B;覆寫內容。 有關使用篩選元素的詳細資訊，請參閱[FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html)。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每個模組所管理的不同節點。
