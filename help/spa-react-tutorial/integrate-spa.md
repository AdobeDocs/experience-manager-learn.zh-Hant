---
title: 整合SPA |編輯與回應快AEM速入SPA門
description: 瞭解在React中編寫的單頁應用程SPA式()原始碼如何與Adobe Experience Manager(AEM)專案整合。 瞭解如何使用現代前端工具（例如webpack dev server），針對JSON模型APISPA快速AEM開發。
sub-product: Sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2109'
ht-degree: 0%

---


# 整合SPA{#integrate-spa}

瞭解在React中編寫的單頁應用程SPA式()原始碼如何與Adobe Experience Manager(AEM)專案整合。 瞭解如何使用現代前端工具（例如webpack dev server），針對JSON模型APISPA快速AEM開發。

## 目標

1. 瞭解專SPA案如何與用戶AEM端程式庫整合。
2. 瞭解如何使用網頁套件開發伺服器進行專屬的前端開發。
3. 探索針對AEMJSON模型API開發使用&#x200B;**proxy**&#x200B;和static **mock**&#x200B;檔案的方式

## 您將建立的

本章將向中添加一個簡單的`Header`元件SPA。 在構建此靜態`Header`元件的過程中，將採用幾AEM種SPA開發方法。

![新頁首AEM於](./assets/integrate-spa/final-header-component.png)

*擴展SPA為添加靜態組 `Header` 件*

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. 使用Maven將程式碼庫部署AEM至本機例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`React/integrate-spa-solution`，在本機檢出程式碼。

## 整合方法{#integration-approach}

項目中建立了兩個模AEM塊：`ui.apps`和`ui.frontend`。

`ui.frontend`模組是包含所有原始碼的[webpack](https://webpack.js.org/)SPA項目。 大部分的開SPA發和測試都將在webpack專案中完成。 觸發生產組建時，會使SPA用webpack建立並編譯。 編譯的對象（CSS和Javascript）會複製到`ui.apps`模組，然後部署至執行AEM階段。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*整合的高階描SPA述。*

有關前端構建版本的其他資訊，請參閱[。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)

## InspectSPA整合{#inspect-spa-integration}

接著，檢查`ui.frontend`模組，瞭解SPA[AEM Project archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)自動生成的模組。

1. 在您選擇的IDE中，開啟WKNDAEM的項目SPA。 本教程將使用[Visual Studio代碼IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND專SPA案](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展開並檢查`ui.frontend`資料夾。 開啟檔案`ui.frontend/package.json`

3. 在`dependencies`下，您應看到與`react`（包括`react-scripts`）相關的數個項目

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend`是以[建立反應應用程式](https://create-react-app.dev/)或CRA為基礎的反應應用程式。 `react-scripts`版本指示使用的CRA版本。

4. 此外，還有三個前置詞為`@adobe`的依賴項：

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上述模組構成[AEM Editor JS SDK&lt;a1/SPA>，並提供功能，讓元件對應至元SPA件AEM。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)

5. 在`package.json`檔案中，已定義數個`scripts`:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   這些是由「建立反應應用程式」製作的[可用](https://create-react-app.dev/docs/available-scripts)標準建置指令碼。

   唯一的區別是將`&& clientlib`添加到`build`指令碼。 此額外指令負責將編譯到的SPA`ui.apps`模組作為構建過程中的客戶端庫進行複製。

   npm模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)用於促進此過程。

6. Inspect檔案`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)會使用此組態檔來決定如何產生用戶端程式庫。

7. Inspect檔案`ui.frontend/pom.xml`。 此檔案將`ui.frontend`資料夾轉換為[Maven模組](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`檔案已更新為在Maven建置期間使用[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)至&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

8. Inspect檔案`index.js`在`ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` 是入口SPA。`ModelManager` 由編輯AEM器SPA JS SDK提供。它負責呼叫並將`pageModel`（JSON內容）注入應用程式。

## 添加標題元件{#header-component}

接著，將新元件新SPA增至，並將變更部署至本機例AEM項。

1. 在`ui.frontend`模組中，在`ui.frontend/src/components`下面建立一個名為`Header`的新資料夾。
2. 在`Header`資料夾下建立名為`Header.js`的檔案。

   ![標題檔案夾和檔案](assets/integrate-spa/header-folder-js.png)

3. 將`Header.js`填入以下內容：

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   以上是將輸出靜態文字字串的標準React元件。

4. 開啟檔案`ui.frontend/src/App.js`。 這是應用程式的入口點。
5. 對`App.js`進行以下更新，以包含靜態`Header`:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

6. 開啟新的終端並導覽至`ui.frontend`資料夾，然後執行`npm run build`命令：

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

7. 導覽至`ui.apps`資料夾。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`下方，您應該會看到已編譯SPA的檔案已從`ui.frontend/build`資料夾複製。

   ![ui.apps中產生的用戶端程式庫](./assets/integrate-spa/compiled-spa-uiapps.png)

8. 返回終端並導航到`ui.apps`資料夾。 執行下列Maven命令：

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   這會將`ui.apps`套件部署至本機執行的例項AEM。

9. 開啟瀏覽器標籤並導覽至[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 現在，您應該會在中看到`Header`元件的內容SPA。

   ![初始標題實作](./assets/integrate-spa/initial-header-implementation.png)

   從項目的根目錄觸發Maven構建時，會自動執行步驟6-8（即`mvn clean install -PautoInstallSinglePackage`）。 您現在應瞭解與用戶端程式庫SPA整合的AEM基本概念。 請注意，您仍可以編輯`Text`元件，並將元AEM件新增至static `Header`元件下方。

## Webpack開發伺服器- Proxy JSON API {#proxy-json}

如先前練習所示，執行建置並將用戶端資料庫同步至本機執行個AEM體需要幾分鐘的時間。 這對於最終測試是可接受的，但對於大多數開發工作來說並不SPA理想。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)可用於快速開發SPA。 由SPA產生的JSON模型驅動AEM。 在本練習中，來自執行中例項的AEMJSON內容將&#x200B;**proxied**&#x200B;放入開發伺服器。

1. 返回IDE並開啟檔案`ui.frontend/package.json`。

   查找如下行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   [建立React App](https://create-react-app.dev/docs/proxying-api-requests-in-development)提供簡易的代理API要求機制。 所有未知請求都將通過`localhost:4502`本地快速入AEM門。

2. 開啟一個終端窗口並導航到`ui.frontend`資料夾。 運行命令`npm start` :

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

3. 開啟新的瀏覽器標籤（如果尚未開啟）並導覽至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack dev server - proxy json](./assets/integrate-spa/webpack-dev-server-1.png)

   您應看到與中相同的內AEM容，但未啟用任何編寫功能。

   >[!NOTE]
   >
   > 由於安全性需求AEM，您必須在相同瀏覽器AEM中，但是使用不同的標籤登入本機例項(http://localhost:4502)。

4. 返回IDE並在`ui.frontend/src/media`建立名為`media`的新資料夾。
5. 下載下列WKND標誌並將其添加到`media`資料夾：

   ![WKND logo](./assets/integrate-spa/wknd-logo-dk.png)

6. 在`ui.frontend/src/components/Header/Header.js`開啟`Header.js`並匯入標誌：

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. 對`Header.js`進行下列更新，將標誌加入頁首：

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   將更改保存到`Header.js`。

8. 返回瀏覽器[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。 您應立即看到應用程式的變更。

   ![標誌已新增至頁首](./assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在中進行內容更新AEM，並查看內容在&#x200B;**webpack-dev-server**&#x200B;中的反映，因為我們代理內容。

9. 在終端中使用`ctrl+c`停止webpack dev server。

## Webpack開發伺服器- Mock JSON API {#mock-json}

另一個快速開發的方法是使用靜態JSON檔案做為JSON模型。 透過「模仿」JSON，我們移除對本機例項的AEM依賴。 此外，它還可讓前端開發人員更新JSON模型，以測試功能並驅動JSON API的變更，JSON API稍後將由後端開發人員實作。

模擬JSON的初始設定需要&#x200B;**本機AEM執行個體**。

1. 在瀏覽器中導覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   這是驅動應用程式AEM的JSON匯出方式。 複製JSON輸出。

2. 返回到IDE ，導航至`ui.frontend/public`並添加名為`mock-content`的新資料夾。
3. 在`ui.frontend/public/mock-content`下方建立名為`mock.model.json`的新檔案。 將&#x200B;**步驟1**&#x200B;的JSON輸出貼到此處。

   ![模擬模型Json檔案](./assets/integrate-spa/mock-model-json-created.png)

4. 在`ui.frontend/public/index.html`開啟檔案`index.html`。 更新頁面模型的AEM中繼資料屬性，以指向變數`%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   使用變數作為`cq:pagemodel_root_url`的值，可更輕鬆地在proxy和mockjson模型之間切換。

5. 開啟檔案`ui.frontend/.env.development`並進行下列更新，以注釋`REACT_APP_PAGE_MODEL_PATH`的先前值：

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 如果當前正在運行，請停止&#x200B;**webpack-dev-server**。 從終端啟動&#x200B;**webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   導覽至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)，您應該會看到與&#x200B;**proxy** SPAjson中使用的內容相同的內容。

7. 對先前建立的`mock.model.json`檔案進行小幅更改。 您應該會在&#x200B;**webpack-dev-server**&#x200B;中立即看到更新的內容。

   ![模擬模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能夠操控JSON模型並檢視即時效果，可SPA協助開發人員瞭解JSON模型API。 它還允許前端和後端開發同時進行。

您現在可以切換`env.development`檔案中的項目，以切換使用JSON內容的位置：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## 使用Sass新增樣式

React最佳實務是保持每個元件的模組化和獨立。 一般建議是避免在元件間重複使用相同的CSS類別名稱，這會使預處理器的使用功能不如前處理器強大。 此專案將針對變數等一些有用功能使用[Sass](https://sass-lang.com/)。 此專案也將大致遵循[SUIT CSS命名慣例](https://github.com/suitcss/suit/blob/master/doc/components.md)。 SUIT是BEM記號（塊元素修飾詞）的變化，用於建立一致的CSS規則。

1. 開啟終端窗口，如果啟動，則停止&#x200B;**webpack-dev-server**。 在`ui.frontend`資料夾內輸入以下命令以安裝Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet) :[

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   安裝`sass`作為對等關係：

   ```shell
   $ npm install sass --save
   ```

2. 安裝`normalize-scss`以標準化瀏覽器的樣式：

   ```shell
   $ npm install normalize-scss
   ```

3. 啟動&#x200B;**webpack-dev-server**，以便我們能夠即時看到樣式更新：

   ```shell
   $ npm start
   ```

   使用Proxy of Mock方法來處理JSON模型API。

4. 返回到IDE並在`ui.frontend/src`下建立名為`styles`的新資料夾。
5. 在`ui.frontend/src/styles`下方建立名為`_variables.scss`的新檔案，並填入下列變數：

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. 將位於`ui.frontend/src/index.css`的`index.css`檔案副檔名重新命名為&#x200B;**`index.scss`**。 將內容取代為：

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. 更新`ui.frontend/src/index.js`以包含重新命名的`index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. 在`ui.frontend/src/components/Header`下方建立名為`Header.scss`的新檔案。 在檔案中填入下列項目：

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. 更新`Header.js`以包含`Header.scss`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. 返回瀏覽器和&#x200B;**webpack-dev-server**:[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![樣式化標題- webpack dev server](./assets/integrate-spa/styled-header.png)

   您現在應該會看到已新增至`Header`元件的更新樣式。

## 將更SPA新部署至

對`Header`所做的變更目前僅透過&#x200B;**webpack-dev-server**&#x200B;顯示。 部署更新SPA的以AEM查看變更。

1. 導覽至專案的根目錄(`aem-guides-wknd-spa`)，並使用Maven將專案部署AEM至：

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您應看到已更新的`Header`並套用標誌和樣式。

   ![更新的頁首(位AEM於](./assets/integrate-spa/final-header-component.png)

   現在更新已SPA在，制AEM作功能可以繼續。

## 節點故障排除錯誤

在開發過程中，您可能會遇到下列錯誤：

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

當&#x200B;**Node.js**&#x200B;和&#x200B;**npm**&#x200B;的本機版本與[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)使用的版本不同時，就會發生這種情況。 運行命令`npm rebuild node-sass`可以臨時修正問題，或刪除`ui.frontend/node_modules`資料夾並重新安裝。

還有一些方法可以更永久地解決這個問題。

* 確保npm和Node.js的本機版本與[Maven build](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)使用的版本匹配
* 在`npm run build`步驟之前，將以下執行步驟添加到`ui.frontend/pom.xml`:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## 恭喜！{#congratulations}

恭喜您，您已更新並探SPA索與的整合AEM! 您現在知道使用&#x200B;**SPAwebpack-dev-server**&#x200B;針對AEMJSON模型API開發程式的兩種不同方法。

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`React/integrate-spa-solution`，在本機檢出程式碼。

### 後續步驟{#next-steps}

[將元SPA件對應AEM至元件](map-components.md) -瞭解如何使用Eract元件與Adobe Experience Manager(AEM)元AEM件SPA對應JS SDK。元件對應可讓使用者在編輯器中對元SPA件進行動AEM態更SPA新，類似於傳統的製作AEM。
