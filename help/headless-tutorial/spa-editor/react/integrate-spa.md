---
title: 集SPA成 |從編輯器AEM開始SPA並反應
description: 瞭解在React中寫入的單頁應用程式(SPA)的原始碼如何與Adobe Experience Manager()項AEM目整合。 學習使用現代前端工具（如webpack dev伺服器）快速開發SPAJSON模AEM型API。
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: c34c27955dbc084620ac4dd811ba4051ea83f447
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 0%

---

# 整合SPA {#developer-workflow}

瞭解在React中寫入的單頁應用程式(SPA)的原始碼如何與Adobe Experience Manager()項AEM目整合。 學習使用現代前端工具（如webpack dev伺服器）快速開發SPAJSON模AEM型API。

## 目標

1. 瞭解項SPA目如何與客AEM戶端庫整合。
2. 瞭解如何使用Webpack開發伺服器進行專用前端開發。
3. 探索使用 **代理** 靜態 **嘲弄** 用於針對AEMJSON模型API進行開發的檔案。

## 您將構建的

在本章中，您將對進行幾SPA個小的更改，以瞭解它是如何與整合AEM的。
本章將添加一個簡單 `Header` 元件SPA。 在構建過程中 **靜態** `Header` 採用多種AEM開SPA發方法。

![中的新標題AEM](./assets/integrate-spa/final-header-component.png)

*擴展SPA以添加靜態 `Header` 元件*

## 必備條件

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。 本章是 [建立項目](create-project.md) 但是，後續的所有需要都是啟用工作SPA的項AEM目。

## 整合方法 {#integration-approach}

作為項目的一部分，建立了兩AEM個模組： `ui.apps` 和 `ui.frontend`。

的 `ui.frontend` 模組是 [網路包](https://webpack.js.org/) 包含所有原始碼的SPA項目。 大部分開SPA發和測試都在webpack項目中完成。 觸發生產生成時，使用SPAwebpack生成並編譯。 編譯的對象（CSS和Javascript）將複製到 `ui.apps` 模組，然後部署到運AEM行時。

![ui.front高級體系結構](assets/integrate-spa/ui-frontend-architecture.png)

*整合的高級描SPA述。*

有關前端構建的其他資訊可以 [此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

## Inspect整SPA合 {#inspect-spa-integration}

接下來，檢查 `ui.frontend` 模組，SPA以瞭解 [項AEM目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

1. 在您選擇的IDE中開啟AEM項目。 本教程將使用 [Visual Studio代碼IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND項SPA目](./assets/integrate-spa/vscode-ide-openproject.png)

1. 展開並檢查 `ui.frontend` 的子菜單。 開啟檔案 `ui.frontend/package.json`

1. 在 `dependencies` 你應該看到幾個 `react` 包括 `react-scripts`

   的 `ui.frontend` 是基於 [建立React應用](https://create-react-app.dev/) 或者簡稱CRA。 的 `react-scripts` 版本指示使用的CRA版本。

1. 還有幾個以前置詞的依賴項 `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   以上模組構成 [AEM編SPA輯器JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) 並提供功能，使「元件」(Components)可以映射SPA到「組AEM件」(Components)。

   還包括 [WCMAEM元件 — 反應核心實施](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [WCMAEM元件 — Spa編輯器 — React Core實施](https://github.com/adobe/aem-react-core-wcm-components-spa)。 這些是一組可重用的UI元件，它們映射到出廠設AEM備。 這些設計旨在按原樣使用，並按照項目的需要設計樣式。

1. 在 `package.json` 檔案有幾個 `scripts` 定義：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   這些是標準生成指令碼 [可用](https://create-react-app.dev/docs/available-scripts) 按鈕

   唯一的區別是 `&& clientlib` 到 `build` 的下界。 此額外指令負責將編譯後SPA的 `ui.apps` 模組，作為生成期間的客戶端庫。

   npm模組 [aem-clientlib生成器](https://github.com/wcm-io-frontend/aem-clientlib-generator) 用來促進這一過程。

1. Inspect檔案 `ui.frontend/clientlib.config.js`。 此配置檔案由 [aem-clientlib生成器](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 確定如何生成客戶端庫。

1. Inspect檔案 `ui.frontend/pom.xml`。 此檔案轉換 `ui.frontend` 資料夾 [馬文模](https://maven.apache.org/guides/mini/guide-multiple-modules.html)。 的 `pom.xml` 已更新檔案以使用 [前面的插件](https://github.com/eirslett/frontend-maven-plugin) 至 **test** 和 **構建** 在馬SPA文建造時。

1. Inspect檔案 `index.js` 在 `ui.frontend/src/index.js`:

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

   `index.js` 是入口SPA。 `ModelManager` 由編輯AEM器SPAJS SDK提供。 它負責呼叫和注入 `pageModel` （JSON內容）。

1. Inspect檔案 `import-components.js` 在 `ui.frontend/src/components/import-components.js`。 此檔案將從框外導入 **反應核心元件** 讓它們能被項目使用。 我們將在下一章中檢AEM查內SPA容到元件的映射。

## 添加靜態組SPA件 {#static-spa-component}

接下來，將新元件添加SPA到並將更改部署到本地AEM實例。 這是一個簡單的改變，只是說明如何SPA更新。

1. 在 `ui.frontend` 模組下 `ui.frontend/src/components` 建立名為 `Header`。
1. 建立名為 `Header.js` 在下面 `Header` 的子菜單。

   ![頭資料夾和檔案](assets/create-project/header-folder-js.png)

1. 填充 `Header.js` 下面列出：

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

   上面是一個標準的React元件，它將輸出靜態文本字串。

1. 開啟檔案 `ui.frontend/src/App.js`。 這是應用程式入口點。
1. 進行以下更新 `App.js` 包含靜態 `Header`:

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

1. 開啟新終端並導航到 `ui.frontend` 資料夾並運行 `npm run build` 命令：

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

1. 導航到 `ui.apps` 的子菜單。 在下面 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 您應看到已編SPA譯的檔案已從`ui.frontend/build` 的子菜單。

   ![在ui.apps中生成的客戶端庫](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 返回到終端並導航到 `ui.apps` 的子菜單。 執行以下Maven命令：

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

   這將部署 `ui.apps` 包到的本地運行實例AEM。

1. 開啟瀏覽器頁籤並導航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您現在應看到 `Header` 顯示的組SPA件。

   ![初始頭實現](./assets/integrate-spa/initial-header-implementation.png)

   當從項目的根觸發Maven生成時(即 `mvn clean install -PautoInstallSinglePackage`)。 您現在應該瞭解與客戶端庫之間集SPA成AEM的基本知識。 請注意，您仍然可以編輯和添加 `Text` 靜AEM態下 `Header` 元件。

## Webpack開發伺服器 — 代理JSON API {#proxy-json}

如前幾個練習所示，執行生成並將客戶端庫同步到本地實例需要幾AEM分鐘的時間。 這對於最終測試是可以接受的，但對於大多數開發來說並不SPA理想。

A [WebPack-Dev伺服器](https://webpack.js.org/configuration/dev-server/) 可用於快速開發SPA。 由生SPA成的JSON模型驅動AEM。 在本練習中，運行實例的JSON內AEM容是 **代理** 進入開發伺服器。

1. 返回到IDE並開啟檔案 `ui.frontend/package.json`。

   查找類似以下行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   的 [建立React應用](https://create-react-app.dev/docs/proxying-api-requests-in-development) 提供了一種簡單的代理API請求的機制。 所有未知請求都通過代理 `localhost:4502`，本地快速啟AEM動。

1. 開啟終端窗口並導航到 `ui.frontend` 的子菜單。 運行命令 `npm start`:

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

1. 開啟新瀏覽器頁籤（如果尚未開啟）並導航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack dev伺服器 — 代理json](./assets/integrate-spa/webpack-dev-server-1.png)

   您應該看到與中相同的內AEM容，但未啟用任何創作功能。

   >[!NOTE]
   >
   > 由於安全要求，AEM您需要登錄到同一瀏覽器AEM中但位於其他頁籤中的本地實例(http://localhost:4502)。

1. 返回到IDE並建立名為 `Header.css` 的 `src/components/Header` 的子菜單。
1. 填充 `Header.css` 下面列出：

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. 重新開啟 `Header.js` 並添加以下行以供參考 `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   儲存變更。

1. 導航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 來查看自動反映的樣式更改。

1. 開啟檔案 `Page.css` 在 `ui.frontend/src/components/Page`。 進行以下更改以修復填充：

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 返回到瀏覽器，位於 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。 您應立即看到對應用所做的更改。

   ![添加到頁眉的樣式](assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在中更新內容AEM，並在中查看 **WebPack-Dev伺服器**，因為我們正在代理內容。

1. 停止Webpack Dev伺服器 `ctrl+c` 在終端。

## 將更SPA新部署到AEM

對 `Header` 當前僅通過 **WebPack-Dev伺服器**。 部署更新SPA的AEM以查看更改。

1. 導航到項目的根(`aem-guides-wknd-spa`)並將項目部署AEM到Maven :

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您應看到更新的 `Header` 的子菜單。

   ![更新的標題AEM](assets/integrate-spa/final-header-component.png)

   現在已更新SPA，創AEM作可以繼續。

## 恭喜！ {#congratulations}

祝賀您，您已更新SPA並探索與的整合AEM! 您現在知道如SPA何使AEM用JSON模型API **WebPack-Dev伺服器**。

### 後續步驟 {#next-steps}

[將組SPA件映射到組AEM件](map-components.md)  — 瞭解如何使用Editor JS SDK將React元件映射AEM到Adobe Experience Manager(AEMSPA)元件。 元件映射使用戶能夠對編輯器SPA中的元件進行AEM動SPA態更新，與傳AEM統創作類似。

## （額外）Webpack開發伺服器 — 模擬JSON API {#mock-json}

另一種快速開發的方法是使用靜態JSON檔案作為JSON模型。 通過「嘲弄」JSON，我們刪除了對本地實例的依AEM賴性。 它還允許前端開發人員更新JSON模型，以便test功能並驅動對JSON API的更改，這些更改隨後將由後端開發人員實施。

模擬JSON的初始設定 **需要本地實AEM例**。

1. 返回到IDE並導航到 `ui.frontend/public` 並添加名為 `mock-content`。
1. 建立名為 `mock.model.json` 下 `ui.frontend/public/mock-content`。
1. 在瀏覽器中導航到 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   這是驅動應用程AEM序的JSON導出。 複製JSON輸出。

1. 貼上檔案上一步的JSON輸出 `mock.model.json`。

   ![模型Json檔案](./assets/integrate-spa/mock-model-json-created.png)

1. 開啟檔案 `index.html` 在 `ui.frontend/public/index.html`。 更新頁面模型的AEM元資料屬性以指向變數 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   將變數用於 `cq:pagemodel_root_url` 將使代理和mock json模型之間的切換更加容易。

1. 開啟檔案 `ui.frontend/.env.development` 並進行以下更新以注釋掉以前的值 `REACT_APP_PAGE_MODEL_PATH` 和 `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 如果當前正在運行，請停止 **WebPack-Dev伺服器**。 啟動 **WebPack-Dev伺服器** 從終端：

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   導航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 您應該看到SPA與 **代理** json。

1. 對 `mock.model.json` 檔案。 您應看到更新的內容立即反映在 **WebPack-Dev伺服器**。

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能夠操作JSON模型並查看即時效果可SPA以幫助開發人員瞭解JSON模型API。 它還允許前端和後端開發並行進行。

現在，您可以通過切換JSON內容中的條目來切換使用位置 `env.development` 檔案：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
