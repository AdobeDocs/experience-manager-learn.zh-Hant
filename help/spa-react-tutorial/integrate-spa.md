---
title: 整合SPA | AEM SPA編輯器快速入門與回應
description: 瞭解在React中撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 瞭解如何使用現代前端工具（例如webpack dev server），針對AEM JSON模型API快速開發SPA。
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 0%

---


# 整合SPA {#integrate-spa}

瞭解在React中撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 瞭解如何使用現代前端工具（例如webpack dev server），針對AEM JSON模型API快速開發SPA。

## 目標

1. 瞭解SPA專案如何與AEM與用戶端程式庫整合。
2. 瞭解如何使用網頁套件開發伺服器進行專屬的前端開發。
3. 探索針對AEM JSON模型 **API** 進行開 **** 發時，使用proxy和靜態模擬檔案

## 您將建立的

本章將向SPA添加一 `Header` 個簡單元件。 在建立此靜態元件的過程 `Header` 中，將採用幾種AEM SPA開發方法。

![AEM中的新標題](./assets/integrate-spa/final-header-component.png)

*擴展SPA以添加靜態組`Header`件*

## 必備條件

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. 使用Maven將程式碼庫部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，請新增 `classic` 描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ，或切換至分支，在本機檢出程式碼 `React/integrate-spa-solution`。

## 整合方法 {#integration-approach}

AEM專案中已建立兩個模組： `ui.apps` 和 `ui.frontend`。

模 `ui.frontend` 塊是包含 [所有SPA原始碼的Web Pack](https://webpack.js.org/) 項目。 大部分的SPA開發和測試都將在webpack專案中完成。 觸發生產組建時，會使用webpack建立並編譯SPA。 已編譯的物件（CSS和Javascript）會複製到模 `ui.apps` 組中，然後部署至AEM執行時期。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*SPA整合的高階描述。*

有關前端構建版本的其他資訊，請 [到此處](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

## 檢查SPA整合 {#inspect-spa-integration}

接下來，請檢 `ui.frontend` 查模組以瞭解由 [AEM Project原型自動產生的SPA](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

1. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 本教學課程將使用 [Visual Studio代碼IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA專案](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展開並檢查文 `ui.frontend` 件夾。 開啟檔案 `ui.frontend/package.json`

3. 在下 `dependencies``react` 面，您應看到數個與 `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   簡 `ui.frontend` 稱為以「建立反應應用程 [](https://create-react-app.dev/) 式」或CRA為基礎的React應用程式。 版 `react-scripts` 本指示使用的CRA版本。

4. 還有三個前置詞的相關性 `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上述模組構成 [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) ，並提供功能，讓SPA元件對應至AEM元件。

5. 在檔案 `package.json` 中定義了數 `scripts` 個：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   這些是Create React App提供的 [標準](https://create-react-app.dev/docs/available-scripts) 「建立指令碼」。

   唯一的區別是，在指令 `&& clientlib` 碼中加 `build` 入。 此額外指令負責在構建過程中將編譯的SPA `ui.apps` 作為客戶端庫複製到模組中。

   npm模 [塊aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 可用來協助此作業。

6. 檢查檔案 `ui.frontend/clientlib.config.js`。 此設定檔案由 [aem-clientlib-generator使用](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) ，以決定如何產生用戶端程式庫。

7. 檢查檔案 `ui.frontend/pom.xml`。 此檔案將檔案 `ui.frontend` 夾轉換為 [Maven模組](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 檔 `pom.xml` 案已更新，在Maven建置 [期間使用Frontend-maven](https://github.com/eirslett/frontend-maven-plugin) -plugin **來測** 試和建 **立** SPA。

8. 請在以下位置檢 `index.js` 查檔案 `ui.frontend/src/index.js`:

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

   `index.js` 是SPA的入口。 `ModelManager` 由AEM SPA Editor JS SDK提供。 它負責呼叫應用程式 `pageModel` 並將（JSON內容）注入。

## 新增標題元件 {#header-component}

接著，將新元件新增至SPA，並將變更部署至本機AEM例項。

1. 在模組 `ui.frontend` 中，建立 `ui.frontend/src/components` 一個名為的新資料夾 `Header`。
2. 建立資料夾下 `Header.js` 面的 `Header` 檔案。

   ![標題檔案夾和檔案](assets/integrate-spa/header-folder-js.png)

3. 填 `Header.js` 入下列：

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

4. 開啟檔案 `ui.frontend/src/App.js`。 這是應用程式的入口點。
5. 進行下列更新以 `App.js` 包含靜態 `Header`:

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

6. 開啟新的終端並導覽至資料 `ui.frontend` 夾並執行命 `npm run build` 令：

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

7. 導覽至資料 `ui.apps` 夾。 您應 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 該會看到已編譯的SPA檔案已從資料夾中複製`ui.frontend/build` 。

   ![ui.apps中產生的用戶端程式庫](./assets/integrate-spa/compiled-spa-uiapps.png)

8. 返回終端並導覽至資料 `ui.apps` 夾。 執行下列Maven命令：

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

   這會將套件部 `ui.apps` 署至本機執行的AEM例項。

9. 開啟瀏覽器標籤並導覽至 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您現在應該會在SPA中看 `Header` 到元件的內容。

   ![初始標題實作](./assets/integrate-spa/initial-header-implementation.png)

   在從項目的根目錄觸發Maven構建時，會自動執行步驟6-8(即 `mvn clean install -PautoInstallSinglePackage`)。 您現在應該瞭解SPA與AEM用戶端程式庫之間整合的基本概念。 請注意，您仍可以在AEM中的靜態 `Text` 元件下方編輯和新增 `Header` 元件。

## Webpack開發伺服器- Proxy JSON API {#proxy-json}

如先前練習所示，執行建置並將用戶端資料庫同步至AEM的本機例項需要幾分鐘的時間。 這對最終測試是可接受的，但對於大多數的SPA開發來說並不理想。

可 [以使用webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 來快速開發SPA。 SPA由AEM產生的JSON模型驅動。 在本練習中，來自AEM執行中例項的JSON內容將 **轉譯** 至開發伺服器。

1. 返回IDE並開啟檔案 `ui.frontend/package.json`。

   查找如下行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   「建 [立反應應用程式](https://create-react-app.dev/docs/proxying-api-requests-in-development) 」提供輕鬆的代理API要求機制。 所有未知的請求都會透過本 `localhost:4502`機AEM快速入門。

2. 開啟終端窗口並導航到該文 `ui.frontend` 件夾。 運行命令 `npm start`:

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

3. 開啟新的瀏覽器標籤（如果尚未開啟）並導覽至 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack dev server - proxy json](./assets/integrate-spa/webpack-dev-server-1.png)

   您應該會看到與AEM相同的內容，但是沒有啟用任何編寫功能。

   >[!NOTE]
   >
   > 由於AEM的安全性需求，您將需要在相同的瀏覽器中，但是使用不同的標籤登入本機AEM例項(http://localhost:4502)。

4. 返回IDE並建立名為的新文 `media` 件夾 `ui.frontend/src/media`。
5. 下載並將下列WKND標誌新增至資 `media` 料夾：

   ![WKND logo](./assets/integrate-spa/wknd-logo-dk.png)

6. 開啟 `Header.js` 於 `ui.frontend/src/components/Header/Header.js` 並匯入標誌：

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. 進行下列更新以 `Header.js` 將標誌加入頁首：

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

   將更改保存到 `Header.js`。

8. 返回瀏覽器，網址 [為http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。 您應立即看到應用程式的變更。

   ![標誌已新增至頁首](./assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在AEM中進行內容更新，並看到 **webpack-dev-server**，因為我們代管內容。

9. 在終端中停止webpack `ctrl+c` dev伺服器。

## Webpack Dev Server - Mock JSON API {#mock-json}

另一個快速開發的方法是使用靜態JSON檔案做為JSON模型。 透過「模仿」JSON，我們移除對本機AEM例項的依賴。 此外，它還可讓前端開發人員更新JSON模型，以測試功能並驅動JSON API的變更，以便後端開發人員建置。

模擬JSON的初始設定需要 **本機AEM例項**。

1. 在瀏覽器中瀏覽至 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   這是由AEM匯出的JSON，是應用程式的驅動器。 複製JSON輸出。

2. 返回到IDE ，導航到並 `ui.frontend/public` 添加名為的新資料夾 `mock-content`。
3. 建立名為下方的新 `mock.model.json` 檔案 `ui.frontend/public/mock-content`。 從此處貼上步驟1 **的JSON輸出** 。

   ![模擬模型Json檔案](./assets/integrate-spa/mock-model-json-created.png)

4. 在中開啟 `index.html` 檔案 `ui.frontend/public/index.html`。 更新AEM頁面模型的中繼資料屬性，以指向變數 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   使用變數作為值，可 `cq:pagemodel_root_url` 讓您更輕鬆地在proxy和mockjson模型之間切換。

5. 開啟檔案 `ui.frontend/.env.development` 並進行下列更新，以注釋先前的值 `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 如果當前正在運行，請停 **止webpack-dev-server**。 從終 **端啟動webpack-dev-server** :

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   導覽至 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) ，您應該會看到SPA包含與proxy **json中使用的相同內** 容。

7. 對先前建立的檔案進 `mock.model.json` 行小幅變更。 您應該會立即在webpack-dev-server中 **看到更新內容**。

   ![模擬模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能夠操控JSON模型並查看即時SPA的效果，可協助開發人員瞭解JSON模型API。 它還允許前端和後端開發同時進行。

您現在可以切換檔案中的項目，以切換使用JSON內容的 `env.development` 位置：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## 使用Sass新增樣式

React最佳實務是保持每個元件的模組化和獨立。 一般建議是避免在元件間重複使用相同的CSS類別名稱，這會使預處理器的使用功能不如前處理器強大。 此專案將使用 [Sass](https://sass-lang.com/) ，以取得一些有用的功能，例如變數。 此專案也將大致遵循 [SUIT CSS命名慣例](https://github.com/suitcss/suit/blob/master/doc/components.md)。 SUIT是BEM記號（塊元素修飾詞）的變化，用於建立一致的CSS規則。

1. 開啟終端窗口並停止 **webpack-dev-server** （如果啟動）。 從資料夾 `ui.frontend` 內輸入以下命令以 [安裝Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   以對 `sass` 等相依性方式安裝：

   ```shell
   $ npm install sass --save
   ```

2. 安裝 `normalize-scss` 以標準化各種瀏覽器的樣式：

   ```shell
   $ npm install normalize-scss
   ```

3. 啟動 **webpack-dev-server** ，讓我們可即時檢視樣式更新：

   ```shell
   $ npm start
   ```

   使用Proxy of Mock方法來處理JSON模型API。

4. 返回到IDE，並在下面 `ui.frontend/src` 建立名為的新資料夾 `styles`。
5. 在名稱下方建立新 `ui.frontend/src/styles` 檔案， `_variables.scss` 並填入下列變數：

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

6. 將副檔名重新命名為 `index.css` ( `ui.frontend/src/index.css` 至) **`index.scss`**。 將內容取代為：

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

7. 更 `ui.frontend/src/index.js` 新以包含重新命名 `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. 建立名為下方的新 `Header.scss` 檔案 `ui.frontend/src/components/Header`。 在檔案中填入下列項目：

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

9. 更新 `Header.scss` 以包含 `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. 返回瀏覽器和 **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![樣式化標題- webpack dev server](./assets/integrate-spa/styled-header.png)

   您現在應該會看到元件中新增的更新樣 `Header` 式。

## 將SPA更新部署至AEM

對所做的變更 `Header` 目前只能透過 **webpack-dev-server看到**。 將更新的SPA部署至AEM，以檢視變更。

1. 導覽至專案的根目錄(`aem-guides-wknd-spa`)，並使用Maven將專案部署至AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您應看到已更新的 `Header` 標誌和套用樣式。

   ![AEM中的更新標題](./assets/integrate-spa/final-header-component.png)

   現在更新的SPA已在AEM中，撰寫作業可以繼續。

## 節點故障排除錯誤

在開發過程中，您可能會遇到下列錯誤：

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

當 **Node.js和** npm **(node.js和npm** )的本機版本與前端-maven-plugin使用的版本 [不同時，就會發生這種情況](https://github.com/eirslett/frontend-maven-plugin)。 執行命令 `npm rebuild node-sass` 可以暫時修正問題，或移除資 `ui.frontend/node_modules` 料夾並重新安裝。

還有一些方法可以更永久地解決這個問題。

* 確保npm和Node.js的本機版本與 [Maven組建版本使用的版本相符](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* 將下列執行步驟新增至步 `ui.frontend/pom.xml` 驟前 `npm run build` 方：

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

## 恭喜！ {#congratulations}

恭喜您，您已更新SPA並探索與AEM的整合！ 您現在知道使用webpack-dev-server針對AEM JSON模型API開發SPA的 **兩種不同方法**。

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ，或切換至分支，在本機檢出程式碼 `React/integrate-spa-solution`。

### 後續步驟 {#next-steps}

[將SPA元件對應至AEM元件](map-components.md) -瞭解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，類似於傳統的AEM編寫。
