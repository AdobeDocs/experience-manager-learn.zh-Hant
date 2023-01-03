---
title: 整合SPA |開始使用AEM SPA Editor and React
description: 了解在React中撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 了解如何使用現代化的前端工具（例如WebPack開發伺服器），針對AEM JSON模型API快速開發SPA。
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

了解在React中撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 了解如何使用現代化的前端工具（例如WebPack開發伺服器），針對AEM JSON模型API快速開發SPA。

## 目標

1. 了解SPA專案如何與AEM與用戶端程式庫整合。
2. 了解如何使用Webpack開發伺服器進行專屬的前端開發。
3. 探索 **代理** 靜態 **模擬** 針對AEM JSON模型API進行開發的檔案。

## 您將建置的

在本章中，您將對SPA進行幾項小幅變更，以了解其與AEM的整合方式。
本章將添加一個簡單 `Header` 元件至SPA。 在建立這個 **靜態** `Header` 元件採用數種AEM SPA開發方法。

![AEM中的新標題](./assets/integrate-spa/final-header-component.png)

*擴充SPA以新增靜態 `Header` 元件*

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment). 本章是 [建立專案](create-project.md) 不過，後續所需的章節是可運作的SPA啟用AEM專案。

## 整合方法 {#integration-approach}

已在AEM專案中建立兩個模組： `ui.apps` 和 `ui.frontend`.

此 `ui.frontend` 模組是 [webpack](https://webpack.js.org/) 包含所有SPA原始碼的專案。 大部分的SPA開發和測試都是在Webpack專案中完成。 觸發生產組建時，會使用webpack建置及編譯SPA。 編譯的成品（CSS和Javascript）會複製到 `ui.apps` 模組，然後部署至AEM執行階段。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*對SPA整合的高階描述。*

有關前端版本編號的其他資訊可以是 [此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA整合 {#inspect-spa-integration}

接下來，檢查 `ui.frontend` 模組，了解由 [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. 在您選擇的IDE中，開啟AEM專案。 本教學課程將使用 [Visual Studio代碼IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA專案](./assets/integrate-spa/vscode-ide-openproject.png)

1. 展開並檢查 `ui.frontend` 檔案夾。 開啟檔案 `ui.frontend/package.json`

1. 在 `dependencies` 您應會看到數個與 `react` 包括 `react-scripts`

   此 `ui.frontend` 是根據 [建立React應用程式](https://create-react-app.dev/) 或CRA。 此 `react-scripts` 版本會指出使用的CRA版本。

1. 還有數個以為首碼的相依性 `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   上述模組構成 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) 並提供功能，以便將SPA元件對應至AEM元件。

   也包括 [AEM WCM元件 — React核心實作](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM WCM元件 — Spa編輯器 — React Core實作](https://github.com/adobe/aem-react-core-wcm-components-spa). 這些是一組可重複使用的UI元件，會對應至現成可用的AEM元件。 這些設計可依原樣使用，並設定符合專案需求的樣式。

1. 在 `package.json` 檔案有數個 `scripts` 已定義：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   這些是製作的標準建置指令碼 [可用](https://create-react-app.dev/docs/available-scripts) 建立React應用程式。

   唯一的差異是 `&& clientlib` 到 `build` 指令碼。 此額外指示負責將編譯的SPA複製至 `ui.apps` 模組做為建置期間的用戶端程式庫。

   npm模組 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 可方便執行。

1. Inspect檔案 `ui.frontend/clientlib.config.js`. 此配置檔案由 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 以決定如何產生用戶端程式庫。

1. Inspect檔案 `ui.frontend/pom.xml`. 此檔案會轉換 `ui.frontend` 資料夾 [Maven模組](https://maven.apache.org/guides/mini/guide-multiple-modules.html). 此 `pom.xml` 檔案已更新，以使用 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) to **測試** 和 **建置** SPA。

1. Inspect檔案 `index.js` at `ui.frontend/src/index.js`:

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

   `index.js` 是SPA的入口。 `ModelManager` 由AEM SPA Editor JS SDK提供。 它負責呼叫和注入 `pageModel` （JSON內容）放入應用程式中。

1. Inspect檔案 `import-components.js` at `ui.frontend/src/components/import-components.js`. 此檔案會立即匯入 **React核心元件** 並讓項目可供使用。 我們將在下一章中檢查AEM內容與SPA元件的對應。

## 新增靜態SPA元件 {#static-spa-component}

接著，將新元件新增至SPA，並將變更部署至本機AEM例項。 這是一項簡單的變更，只是為了說明SPA的更新方式。

1. 在 `ui.frontend` 模組下方 `ui.frontend/src/components` 建立名為 `Header`.
1. 建立名為 `Header.js` 在下面 `Header` 檔案夾。

   ![標題資料夾和檔案](assets/create-project/header-folder-js.png)

1. 填入 `Header.js` 並搭配下列項目：

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

1. 開啟檔案 `ui.frontend/src/App.js`. 這是應用程式入口點。
1. 對 `App.js` 納入靜態 `Header`:

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

1. 開啟新終端機並導覽至 `ui.frontend` 資料夾和執行 `npm run build` 命令：

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

1. 導覽至 `ui.apps` 檔案夾。 下方 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 您應該會看到已編譯的SPA檔案已從`ui.frontend/build` 檔案夾。

   ![在ui.apps中產生的用戶端程式庫](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 返回終端並導覽至 `ui.apps` 檔案夾。 執行以下Maven命令：

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

   這會部署 `ui.apps` 套件至本機執行中的AEM例項。

1. 開啟瀏覽器標籤並導覽至 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 您現在應該會看到 `Header` 元件。

   ![初始標題實作](./assets/integrate-spa/initial-header-implementation.png)

   從專案根目錄觸發Maven組建時，上述步驟會自動執行(即 `mvn clean install -PautoInstallSinglePackage`)。 您現在應了解SPA和AEM用戶端程式庫之間整合的基本知識。 請注意，您仍可以編輯和新增 `Text` AEM中靜態下方的元件 `Header` 元件。

## Webpack開發伺服器 — 代理JSON API {#proxy-json}

如先前的練習所示，執行組建，並將用戶端程式庫同步至AEM的本機例項需要幾分鐘的時間。 這是最終測試可接受的選項，但不適用於大部分SPA開發。

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 可用來快速開發SPA。 SPA是由AEM產生的JSON模型驅動。 在本練習中，來自執行中AEM例項的JSON內容為 **代理** 進入開發伺服器。

1. 返回到IDE並開啟檔案 `ui.frontend/package.json`.

   尋找如下所示的行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   此 [建立React應用程式](https://create-react-app.dev/docs/proxying-api-requests-in-development) 提供簡單的代理API請求機制。 所有未知請求都會透過 `localhost:4502`，即本機AEM快速入門。

1. 開啟終端機視窗並導覽至 `ui.frontend` 檔案夾。 運行命令 `npm start`:

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

1. 開啟新的瀏覽器標籤（如果尚未開啟）並導覽至 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack開發伺服器 — 代理json](./assets/integrate-spa/webpack-dev-server-1.png)

   您應該會看到與AEM相同的內容，但未啟用任何製作功能。

   >[!NOTE]
   >
   > 由於AEM的安全性需求，您需要在相同的瀏覽器中，但位在不同的索引標籤中，登入本機AEM例項(http://localhost:4502)。

1. 返回到IDE並建立名為的檔案 `Header.css` 在 `src/components/Header` 檔案夾。
1. 填入 `Header.css` 並搭配下列項目：

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

1. 重新開啟 `Header.js` 並新增下列行以供參考 `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   儲存變更。

1. 導覽至 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) ，以查看自動反映的樣式更改。

1. 開啟檔案 `Page.css` at `ui.frontend/src/components/Page`. 進行下列變更以修正邊框間距：

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 返回瀏覽器時間： [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). 您應會立即看到應用程式的變更反映在內。

   ![新增至標題的樣式](assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在AEM中更新內容，並查看 **webpack-dev-server**，因為我們代理內容。

1. 使用停止Webpack開發伺服器 `ctrl+c` 在終端機。

## 將SPA更新部署至AEM

對 `Header` 目前僅可透過 **webpack-dev-server**. 將更新的SPA部署至AEM以查看變更。

1. 導覽至專案的根目錄(`aem-guides-wknd-spa`)，並使用Maven將專案部署至AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 您應會看到更新 `Header` 和樣式。

   ![更新AEM中的標題](assets/integrate-spa/final-header-component.png)

   現在更新的SPA已在AEM中，製作即可繼續。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA並探索與AEM的整合！ 您現在知道如何使用，針對AEM JSON模型API開發SPA **webpack-dev-server**.

### 後續步驟 {#next-steps}

[將SPA元件對應至AEM元件](map-components.md)  — 了解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。

## （額外獎勵）Webpack開發伺服器 — 模擬JSON API {#mock-json}

另一種快速開發的方法是使用靜態JSON檔案作為JSON模型。 借由「模擬」JSON，我們移除了對本機AEM例項的相依性。 此外，前端開發人員也能更新JSON模型，以測試功能，並推動對JSON API的變更，JSON API稍後將由後端開發人員實作。

模擬JSON的初始設定可 **需要本機AEM例項**.

1. 返回IDE並導航到 `ui.frontend/public` 並添加名為 `mock-content`.
1. 建立名為 `mock.model.json` 在 `ui.frontend/public/mock-content`.
1. 在瀏覽器中導覽至 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   這是由AEM匯出的JSON，此JSON會驅動應用程式。 複製JSON輸出。

1. 將上一步的JSON輸出貼到檔案中 `mock.model.json`.

   ![模擬模型Json檔案](./assets/integrate-spa/mock-model-json-created.png)

1. 開啟檔案 `index.html` at `ui.frontend/public/index.html`. 更新AEM頁面模型的中繼資料屬性以指向變數 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   將變數用於 `cq:pagemodel_root_url` 可讓您在proxy和模擬json模型之間切換。

1. 開啟檔案 `ui.frontend/.env.development` 並進行下列更新，以註解的先前值 `REACT_APP_PAGE_MODEL_PATH` 和 `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 如果目前執行中，請停止 **webpack-dev-server**. 啟動 **webpack-dev-server** 從終端：

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   導覽至 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 而且您應會看到SPA中使用的相同內容 **代理** json。

1. 對 `mock.model.json` 檔案。 您應會看到更新的內容立即反映在 **webpack-dev-server**.

   ![模擬模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能夠操控JSON模型並查看即時SPA上的效果，可協助開發人員了解JSON模型API。 它還允許前端和後端開發並行進行。

您現在可以切換使用JSON內容的位置，方法是切換 `env.development` 檔案：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
