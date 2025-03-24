---
title: 整合SPA | AEM SPA Editor and React快速入門
description: 瞭解如何將在React中撰寫的單頁應用程式(SPA)原始程式碼與Adobe Experience Manager (AEM)專案整合。 瞭解如何使用現代前端工具（如Webpack開發伺服器），以針對AEM JSON模型API快速開發SPA。
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
duration: 409
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 0%

---

# 整合SPA {#developer-workflow}

瞭解如何將在React中撰寫的單頁應用程式(SPA)原始程式碼與Adobe Experience Manager (AEM)專案整合。 瞭解如何使用現代前端工具（如Webpack開發伺服器），以針對AEM JSON模型API快速開發SPA。

## 目標

1. 瞭解SPA專案如何與AEM和使用者端程式庫整合。
2. 瞭解如何使用Webpack開發伺服器來進行專屬的前端開發。
3. 探索使用&#x200B;**proxy**&#x200B;和靜態&#x200B;**Mock**&#x200B;檔案來針對AEM JSON模型API進行開發。

## 您將建置的內容

在本章中，您將會對SPA進行幾項細微的變更，以瞭解如何與AEM整合。
本章會將簡單的`Header`元件新增至SPA。 在建置此&#x200B;**靜態** `Header`元件的過程中，使用了數種AEM SPA開發方法。

![在AEM中新增標題](./assets/integrate-spa/final-header-component.png)

*已延伸SPA以新增靜態`Header`元件*

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 本章是[建立專案](create-project.md)章節的延續，但是您只要遵循啟用SPA的有效AEM專案，就可以了。

## 整合方法 {#integration-approach}

已建立兩個模組作為AEM專案的一部分： `ui.apps`和`ui.frontend`。

`ui.frontend`模組是包含所有SPA原始碼的[webpack](https://webpack.js.org/)專案。 大部分的SPA開發和測試都在webpack專案中完成。 觸發生產組建時，會使用webpack建置及編譯SPA。 編譯的成品（CSS和JavaScript）會複製到`ui.apps`模組，然後部署至AEM執行階段。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*SPA整合的高階描述。*

有關前端組建的其他資訊可在[此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)找到。

## 檢查SPA整合 {#inspect-spa-integration}

接下來，請檢查`ui.frontend`模組以瞭解由[AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)自動產生的SPA。

1. 在您選擇的IDE中，開啟您的AEM專案。 此教學課程將使用[Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA專案](./assets/integrate-spa/vscode-ide-openproject.png)

1. 展開並檢查`ui.frontend`資料夾。 開啟檔案`ui.frontend/package.json`

1. 在`dependencies`底下，您應該會看到數個與`react`相關的專案，包括`react-scripts`

   `ui.frontend`是以[建立React應用程式](https://create-react-app.dev/)或CRA為基礎的React應用程式。 `react-scripts`版本指出使用的CRA版本。

1. 也有數個前置詞為`@adobe`的相依性：

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   上述模組構成[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)，並提供可將SPA元件對應至AEM元件的功能。

   此外也包含[AEM WCM元件 — React Core實作](https://github.com/adobe/aem-react-core-wcm-components-base)和[AEM WCM元件 — Spa編輯器 — React Core實作](https://github.com/adobe/aem-react-core-wcm-components-spa)。 這是一組可重複使用的UI元件，對應至現成可用的AEM元件。 這些模組的設計用途與樣式皆符合您專案的需求。

1. 在`package.json`檔案中，有數個`scripts`已定義：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   這些是標準組建指令碼，由Create React應用程式讓[可用](https://create-react-app.dev/docs/available-scripts)。

   唯一的差異是將`&& clientlib`加到`build`指令碼中。 這個額外的指令負責在建置期間將編譯的SPA作為使用者端程式庫複製到`ui.apps`模組中。

   npm模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)已用來協助處理這個問題。

1. 檢查檔案`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)使用此設定檔來決定如何產生使用者端程式庫。

1. 檢查檔案`ui.frontend/pom.xml`。 此檔案會將`ui.frontend`資料夾轉換為[Maven模組](https://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`檔案已更新，以便在Maven組建期間使用[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)至&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

1. 在`ui.frontend/src/index.js`檢查檔案`index.js`：

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

   `index.js`是SPA的進入點。 `ModelManager`由AEM SPA Editor JS SDK提供。 它負責呼叫`pageModel` （JSON內容）並將其插入應用程式。

1. 在`ui.frontend/src/components/import-components.js`檢查檔案`import-components.js`。 此檔案會匯入現成可用的&#x200B;**React Core Components**，並讓它們可用於專案。 我們將在下一章中檢查AEM內容與SPA元件的對應。

## 新增靜態SPA元件 {#static-spa-component}

接下來，將新元件新增至SPA，並將變更部署至本機AEM執行個體。 這是一個簡單的變更，只是為了說明SPA如何更新。

1. 在`ui.frontend`模組的`ui.frontend/src/components`下建立名為`Header`的新資料夾。
1. 在`Header`資料夾下建立名為`Header.js`的檔案。

   ![標頭資料夾和檔案](assets/create-project/header-folder-js.png)

1. 以下列專案填入`Header.js`：

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

1. 開啟檔案`ui.frontend/src/App.js`。 這是應用程式進入點。
1. 對`App.js`進行下列更新以包含靜態`Header`：

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

1. 開啟新的終端機，並導覽至`ui.frontend`資料夾並執行`npm run build`命令：

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

1. 導覽至`ui.apps`資料夾。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`下方，您應該會看到已從`ui.frontend/build`資料夾複製編譯的SPA檔案。

   ![在ui.apps中產生的使用者端資料庫](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 返回終端機並導覽至`ui.apps`資料夾。 執行下列Maven命令：

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

   這會將`ui.apps`封裝部署至AEM的本機執行個體。

1. 開啟瀏覽器索引標籤並導覽至[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您現在應該會看到`Header`元件的內容顯示在SPA中。

   ![初始標頭實作](./assets/integrate-spa/initial-header-implementation.png)

   從專案的根目錄（即`mvn clean install -PautoInstallSinglePackage`）觸發Maven組建時，會自動執行上述步驟。 您現在應該瞭解SPA與AEM使用者端程式庫之間整合的基本概念。 請注意，您仍然可以在AEM中的靜態`Header`元件下方編輯和新增`Text`元件。

## Webpack Dev Server - JSON API的Proxy {#proxy-json}

如先前練習所示，執行組建並將使用者端程式庫同步至AEM的本機執行個體需要幾分鐘的時間。 這對於最終測試來說是可接受的，但對於大多數SPA開發而言並不理想。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)可用來快速開發SPA。 SPA由AEM產生的JSON模型驅動。 在本練習中，來自AEM執行個體的JSON內容已將&#x200B;**代理至**&#x200B;開發伺服器。

1. 返回IDE並開啟檔案`ui.frontend/package.json`。

   尋找類似以下的一行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   [建立React應用程式](https://create-react-app.dev/docs/proxying-api-requests-in-development)提供簡易的機制來代理API要求。 所有未知的請求都是透過本機AEM快速入門`localhost:4502`進行代理處理。

1. 開啟終端機視窗並導覽至`ui.frontend`資料夾。 執行命令`npm start`：

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

1. 開啟新的瀏覽器索引標籤（如果尚未開啟），並導覽至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack開發伺服器 — Proxy json](./assets/integrate-spa/webpack-dev-server-1.png)

   您應該會看到與AEM相同的內容，但未啟用任何撰寫功能。

   >[!NOTE]
   >
   > 由於AEM的安全性需求，您將需要在相同瀏覽器中的不同分頁中登入本機AEM執行個體(http://localhost:4502)。

1. 返回IDE並在`src/components/Header`資料夾中建立名為`Header.css`的檔案。
1. 以下列專案填入`Header.css`：

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

1. 重新開啟`Header.js`，並將下列行加入參考`Header.css`：

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   儲存變更。

1. 導覽至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)，檢視樣式變更會自動反映出來。

1. 在`ui.frontend/src/components/Page`開啟檔案`Page.css`。 進行下列變更以修正邊框間距：

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 在[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)返回瀏覽器。 您應該會立即看到應用程式的變更反映出來。

   ![樣式已新增至標頭](assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在AEM中進行內容更新，並且看到這些更新反映在&#x200B;**webpack-dev-server**&#x200B;中，因為我們正在代理內容。

1. 停止終端機中具有`ctrl+c`的webpack開發伺服器。

## 將SPA更新部署至AEM

對`Header`所做的變更目前只能透過&#x200B;**webpack-dev-server**&#x200B;顯示。 將更新的SPA部署至AEM以檢視變更。

1. 導覽至專案的根目錄(`aem-guides-wknd-spa`)，並使用Maven將專案部署至AEM：

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您應該會看到已套用的更新`Header`和樣式。

   ![已更新AEM中的標題](assets/integrate-spa/final-header-component.png)

   現在，更新的SPA已匯入AEM，製作作業可以繼續進行。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA並探索與AEM整合！ 您現在知道如何使用&#x200B;**webpack-dev-server**，針對AEM JSON模型API開發SPA。

### 後續步驟 {#next-steps}

[將SPA元件對應至AEM元件](map-components.md) — 瞭解如何使用AEM SPA Editor JS SDK將React元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓使用者在AEM SPA Editor中對SPA元件進行動態更新，類似於傳統的AEM編寫。

## （額外優惠） Webpack Dev Server - Mock JSON API {#mock-json}

快速開發的另一種方法是使用靜態JSON檔案充當JSON模型。 透過「嘲弄」 JSON，我們移除對本機AEM執行個體的相依性。 它也能讓前端開發人員更新JSON模型，以測試功能並推動JSON API的變更，之後再由後端開發人員實作。

模擬JSON的初始設定&#x200B;**需要本機AEM執行個體**。

1. 返回IDE並導覽至`ui.frontend/public`並新增名稱為`mock-content`的新資料夾。
1. 在`ui.frontend/public/mock-content`下建立名為`mock.model.json`的新檔案。
1. 在瀏覽器中導覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   這是AEM匯出的驅動應用程式JSON。 複製JSON輸出。

1. 將上一步的JSON輸出貼到檔案`mock.model.json`中。

   ![模擬模型Json檔案](./assets/integrate-spa/mock-model-json-created.png)

1. 在`ui.frontend/public/index.html`開啟檔案`index.html`。 更新AEM頁面模型的中繼資料屬性，以指向變數`%REACT_APP_PAGE_MODEL_PATH%`：

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   使用變數做為`cq:pagemodel_root_url`的值可讓您更輕鬆地在Proxy和模擬json模型之間切換。

1. 開啟檔案`ui.frontend/.env.development`並進行下列更新，以註解`REACT_APP_PAGE_MODEL_PATH`和`REACT_APP_API_HOST`的上一個值：

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 如果目前正在執行，請停止&#x200B;**webpack-dev-server**。 從終端機啟動&#x200B;**webpack-dev-server**：

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   導覽至[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)，您應該會看到內容與&#x200B;**proxy** json中所使用內容相同的SPA。

1. 對先前建立的`mock.model.json`檔案進行小幅變更。 您應該會看到更新後的內容立即反映在&#x200B;**webpack-dev-server**&#x200B;中。

   ![模型的json更新](./assets/integrate-spa/webpack-mock-model.gif)

能夠操控JSON模型並檢視對即時SPA的影響有助於開發人員瞭解JSON模型API。 此外，前端和後端開發均可並行進行。

您現在可以透過切換`env.development`檔案中的專案來切換使用JSON內容的位置：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
