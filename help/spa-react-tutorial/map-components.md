---
title: 將元SPA件對應至元AEM件 |編輯與回應快AEM速入SPA門
description: 瞭解如何使用Editor JS AEM SDK將React元件對應AEM至Adobe Experience Manager()元SPA件。 元件對應可讓使用者在編輯器中對元SPA件進行動AEM態更SPA新，類似於傳統的製作AEM。
sub-product: Sites
feature: 編SPA輯器
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2264'
ht-degree: 1%

---


# 將元SPA件對AEM應至元件{#map-components}

瞭解如何使用Editor JS AEM SDK將React元件對應AEM至Adobe Experience Manager()元SPA件。 元件對應可讓使用者在編輯器中對元SPA件進行動AEM態更SPA新，類似於傳統的製作AEM。

本章深入探討AEMJSON模型API，以及如何將元件公開的JSON內容自動插入AEMRearced元件中做為prop。

## 目標

1. 瞭解如何將元AEM件對應至SPA元件。
2. 瞭解&#x200B;**Container**&#x200B;元件與&#x200B;**Content**&#x200B;元件之間的差異。
3. 建立新的React元件，以對應至現有元AEM件。

## 您將建立的

本章將檢查提供的`Text`元件SPA如何映射到AEM`Text`元件。 將建立一個新的&lt;a0/SPA>元件，可用於SPA並在中編AEM寫。 `Image`**版面容器**&#x200B;和&#x200B;**範本編輯器**&#x200B;原則的立即可用功能也將用來建立外觀稍有變化的檢視。

![章節範例最終編寫](./assets/map-components/final-page.png)

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. 使用Maven將程式碼庫部署AEM至本機例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上檢視完成的程式碼，或切換至分支`React/map-components-solution`，在本機檢出程式碼。

## 映射方法

基本概念是將元件對SPA應至元AEM件。 元AEM件、執行伺服器端、匯出內容做為JSON模型API的一部分。 JSON內容會由在瀏覽器SPA中執行用戶端的使用者使用。 將建立元件和組SPA件之間AEM的1:1映射。

![將元件對應至反應元件AEM的高階概述](./assets/map-components/high-level-approach.png)

*將元件對應至反應元件AEM的高階概述*

## Inspect文本元件

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)提供映射至[AEMText元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)的`Text`元件。 這是&#x200B;**content**&#x200B;元件的範例，其中從中轉譯&#x200B;*content* AEM。

讓我們看看元件的運作方式。

### InspectJSON模型

1. 在跳至程式SPA碼之前，請務必瞭解提供的JSON模AEM型。 導覽至[核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)並檢視Text元件的頁面。 核心元件庫提供所有核心元件AEM的範例。
2. 選擇&#x200B;**JSON**&#x200B;標籤，以取得其中一個範例：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性：`text`、`richText`和`:type`。

   `:type` 是一個保留屬性，列 `sling:resourceType` 出元件的(或路AEM徑)。`:type`的值是用於將元件映射到組AEM件的SPA值。

   `text` 以 `richText` 及將公開給元件的其他屬SPA性。

### Inspect文字元件

1. 開啟新的終端機，並導覽至專案內的`ui.frontend`資料夾。 然後運行`npm install`和`npm start`以啟動&#x200B;**webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   `ui.frontend`模組目前已設定為使用[模擬JSON模型](./integrate-spa.md#mock-json)。

2. 您應會看到新的瀏覽器視窗開啟至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![具有模擬內容的Webpack開發伺服器](./assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKNDAEM的項目SPA。 展開`ui.frontend`模組並開啟`ui.frontend/src/components/Text/Text.js`下的`Text.js`檔案：

   ![Text.js React元件原始碼](./assets/map-components/vscode-ide-text-js.png)

4. 我們將檢查的第一個區域是~第40行的`class Text`:

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

   `Text` 是標準的React元件。元件使用`this.props.richText`來判斷要轉譯的內容是富格文字還是純文字。 實際使用的「內容」來自`this.props.text`。 為避免潛在的XSS攻擊，在使用[allisalSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)來轉換內容之前，會先透過`DOMPurify`逸出Rich Text。 在練習中，請前面的JSON模型中取用`richText`和`text`屬性。

5. 接下來，請看`TextEditConfig`的第29行：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述程式碼負責決定何時在作者環境中呈現預留AEM位置。 如果`isEmpty`方法返回&#x200B;**true**，則會呈現預留位置。

6. 最後，請看~line 62的`MapTo`呼叫：

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 由Editor AEM SPA JS SDK(`@adobe/aem-react-editable-components`)提供。路徑`wknd-spa-react/components/text`表示元件的&lt;a1/AEM>。 `sling:resourceType`此路徑與先前觀察到的JSON模型公開的`:type`相符。 `MapTo` 負責剖析JSON模型回應，並將正確值傳 `props` 遞至SPA元件。

   您可以在AEM`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`中找到`Text`元件定義。

7. 修改`ui.frontend/public/mock-content/mock.model.json`處的`mock.model.json`檔案以進行實驗。 在~line 62更新第一個`Text`值，以使用&#x200B;**`H1`**&#x200B;和&#x200B;**`u`**&#x200B;標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   導覽至[http://localhost:3000](http://localhost:3000)以檢視效果：

   ![更新的文字模型](./assets/map-components/updated-text-model.png)

   嘗試在&#x200B;**true** / **false**&#x200B;之間切換`richText`屬性，以檢視演算邏輯的實際運作。

8. Inspect`Text.scss`在`ui.frontend/src/components/Text/Text.scss`。

   本章的起始代碼庫添加了此檔案，並使用前一章中添加的[Sass](https://sass-lang.com/)功能。 請注意`ui.frontend/src/styles/_variables.scss`引用的變數。

## 建立影像元件

接著，建立映射至[Image元件AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)的`Image` React元件。 `Image`元件是&#x200B;**content**&#x200B;元件的另一個範例。

### InspectJSON

在跳至程式SPA碼之前，請檢查由提供的JSON模AEM型。

1. 導覽至核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)中的[影像範例。

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   `src`、`alt`和`title`的屬性將用於填充SPA`Image`元件。

   >[!NOTE]
   >
   > 還有其他顯示的影像屬性(`lazyEnabled`、`widths`)可讓開發人員建立可調式和延遲載入元件。 本教學課程中建立的元件將很簡單，而且&#x200B;**not**&#x200B;會使用這些進階屬性。

2. 返回IDE並在`ui.frontend/public/mock-content/mock.model.json`處開啟`mock.model.json`。 由於這是專案的新元件，因此我們需要「模擬」影像JSON。

   在~line 70中，為`image`模型新增JSON項目（別忘記第二個`text_23828680`後面的尾隨逗號`,`），並更新`:itemsOrder`陣列。

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

   專案包含`/mock-content/adobestock-140634652.jpeg`的範例影像，將與&#x200B;**webpack-dev-server**&#x200B;搭配使用。

   您可以在這裡檢視完整的[mock.model.json](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)。

### 實作影像元件

1. 接著，在`ui.frontend/src/components`下建立一個名為`Image`的新資料夾。
2. 在`Image`資料夾下面建立一個名為`Image.js`的新檔案。

   ![Image.js檔案](./assets/map-components/image-js-file.png)

3. 將以下`import`語句添加到`Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 然後新增`ImageEditConfig`以決定何時在以下位置顯示預留位置AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未設定`src`屬性，則預留位置將顯示。

5. 接下來實作`Image`類別：

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

   上述程式碼會根據JSON模型傳入的prop `src`、`alt`和`title`來轉換`<img>`。

6. 新增`MapTo`程式碼，將React元件對應至元AEM件：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   請注意，字串`wknd-spa-react/components/image`與`ui.apps`中元AEM件的位置相對應，網址為：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

7. 在同一目錄中建立名為`Image.scss`的新檔案並添加以下內容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. 在`Image.js`中，在`import`語句下方的頂部添加對檔案的引用：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   您可以在此處檢視已完成的[Image.js](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)。

9. 開啟檔案`ui.frontend/src/components/import-components.js`並添加對新`Image`元件的引用：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. 如果尚未啟動，請啟動&#x200B;**webpack-dev-server**。 導覽至[http://localhost:3000](http://localhost:3000)，您應該會看到影像演算：

   ![已新增影像至模擬](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **獎金挑戰**:在中實作新方 `Image.js` 法，將值顯示為 `this.props.title` 影像下方的標題。

## 更新策略AEM:

`Image`元件僅顯示在&#x200B;**webpack-dev-server**&#x200B;中。 接著，部署更新SPA至AEM並更新範本原則。

1. 停止&#x200B;**webpack-dev-server**&#x200B;並從專案的根目錄部署變更，以使用您的Maven技AEM能：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從「AEM開始」畫面導覽至「工具&#x200B;**** > **範本** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**」。

   選擇並編輯&#x200B;**SPA Page**:

   ![編輯頁SPA面範本](./assets/map-components/edit-spa-page-template.png)

3. 選擇&#x200B;**配置容器**，然後按一下其&#x200B;**policy**&#x200B;表徵圖以編輯策略：

   ![配置容器原則](./assets/map-components/layout-container-policy.png)

4. 在「**允許的元件** > **WKND SPA React - Content** >」下檢查&#x200B;**Image**&#x200B;元件：

   ![已選取影像元件](./assets/map-components/check-image-component.png)

   在「**預設元件** > **添加映射**」下，選擇&#x200B;**影像- WKND SPA React - Content**&#x200B;元件：

   ![設定預設元件](./assets/map-components/default-components.png)

   輸入&#x200B;**mime類型**&#x200B;的`image/*`。

   按一下&#x200B;**Done**&#x200B;保存策略更新。

5. 在&#x200B;**版面容器**&#x200B;中，按一下&#x200B;**Text**&#x200B;元件的&#x200B;**policy**&#x200B;圖示：

   ![文本元件策略表徵圖](./assets/map-components/edit-text-policy.png)

   建立名為&#x200B;**WKND SPA Text**&#x200B;的新策略。 在「**Plugins** > **Formatting** >」下，選中所有框以啟用其他格式設定選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在「**Plugins** > **段落樣式** >」下，選中該框以「啟用段落樣式&#x200B;**」:**

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下&#x200B;**Done**&#x200B;保存策略更新。

6. 導覽至&#x200B;**Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

   您也可以編輯`Text`元件，並在&#x200B;**全螢幕**&#x200B;模式中新增其他段落樣式。

   ![全螢幕豐富式文字編輯](assets/map-components/full-screen-rte.png)

7. 您也可以從&#x200B;**Asset finder**&#x200B;拖放影像：

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過[AEM Assets](http://localhost:4502/assets.html/content/dam)新增您自己的影像，或安裝標準[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)的完成程式碼庫。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)包含許多可在WKND上重新使用的影像SPA。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安裝軟體包。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect版面容器

**版面容器**&#x200B;的支援由編輯器SDK自動提AEM供SPA。 **版面容器**&#x200B;是&#x200B;**容器**&#x200B;元件，如名稱所示。 容器元件是接受JSON結構的元件，其代表&#x200B;*other*&#x200B;元件並動態執行個體化它們。

讓我們進一步檢查「版面容器」。

1. 在瀏覽器中瀏覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API —— 自適應格線](./assets/map-components/responsive-grid-modeljson.png)

   **版面容器**&#x200B;元件具有`wcm/foundation/components/responsivegrid`的`sling:resourceType`，並由編輯器使用`:type`屬性來識別SPA，就像`Text`和`Image`元件一樣。

   編輯器提供使用[配置模式](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新調整元件大小的相同功SPA能。

2. 返回[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 新增其他&#x200B;**Image**&#x200B;元件，並嘗試使用&#x200B;**Layout**&#x200B;選項重新調整其大小：

   ![使用版面模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，並觀察`columnClassNames`作為JSON的一部分：

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名`aem-GridColumn--default--4`表示元件應基於12列網格為4列寬。 如需[回應式格線的詳細資訊，請參閱此處](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回到IDE，在`ui.apps`模組中，`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`中定義了一個客戶端庫。 開啟檔案`less/grid.less`。

   此檔案會決定&#x200B;**版面配置容器**&#x200B;使用的中斷點（`default`、`tablet`和`phone`）。 此檔案旨在根據專案規格自訂。 目前，中斷點設為`1200px`和`650px`。

5. 您應該能夠使用`Text`元件的回應功能和更新的富格文字原則來製作如下檢視：

   ![章節範例最終編寫](./assets/map-components/final-page.png)

## 恭喜！{#congratulations}

恭喜您，您學習了如SPA何將元件對AEM應至元件，並實作了新的`Image`元件。 您也可以探索&#x200B;**版面容器**&#x200B;的回應功能。

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上檢視完成的程式碼，或切換至分支`React/map-components-solution`，在本機檢出程式碼。

### 後續步驟{#next-steps}

[導覽和路由](navigation-routing.md) -瞭解使用Editor SDK對應至頁SPA面，如何支援AEM多SPA個檢視。動態導航是使用React Router實現的，並添加到現有的Header元件中。

## 附加——將配置保留到源控制{#bonus}

在許多情況下，特別是在項目開始AEM時，將配置（如模板和相關內容策略）保留到源控制非常有用。 這可確保所有開發人員針對相同的內容和組態進行工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟度，管理模板的做法就可以交給一組特殊的超級用戶。

接下來的幾個步驟將使用Visual Studio代碼IDE和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)進行，但可能使用您已配置為從本地實例&#x200B;**pull**&#x200B;或&#x200B;**import**&#x200B;內容的任何工具和任何IDE來執行AEM。

1. 在Visual Studio代碼IDE中，確保已通過Marketplace擴展AEM安裝&#x200B;**VSCode Sync**:

   ![VSCode同AEM步](./assets/map-components/vscode-aem-sync.png)

2. 展開「項目瀏覽器」中的&#x200B;**ui.content**&#x200B;模組，並導航至`/conf/wknd-spa-react/settings/wcm/templates`。

3. **Right+按一** 下資料 `templates` 夾，然後選 **取「從伺AEM服器匯入」**:

   ![VSCode匯入範本](./assets/map-components/import-aem-servervscode.png)

4. 重複這些步驟以導入內容，但選擇位於`/conf/wknd-spa-react/settings/wcm/templates/policies`的&#x200B;**policys**&#x200B;資料夾。

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

   `filter.xml`檔案負責標識將隨軟體包一起安裝的節點的路徑。 請注意，每個篩選器上的`mode="merge"`都表示不會修改現有內容，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此代碼部署&#x200B;**not**&#x200B;覆寫內容非常重要。 有關使用篩選器元素的詳細資訊，請參閱[FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html)。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以瞭解每個模組管理的不同節點。
