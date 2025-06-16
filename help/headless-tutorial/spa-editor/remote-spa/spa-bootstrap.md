---
title: 適用於SPA編輯器的遠端SPA Bootstrap
description: 瞭解如何啟動啟動遠端SPA，以符合AEM SPA Editor相容性。
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# 適用於SPA編輯器的遠端SPA Bootstrap

{{spa-editor-deprecation}}

在將可編輯區域新增至遠端SPA之前，必須先使用AEM SPA Editor JavaScript SDK和其他幾項設定來啟動可編輯區域。

## 安裝AEM SPA Editor JS SDK npm相依性

首先，檢閱AEM的React專案SPA npm相依性，並安裝這些相依性。

* [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) ：提供從AEM擷取內容的API。
* [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) ：提供將AEM內容對應至SPA元件的API。
* [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) ：提供用於建置自訂SPA元件的API，並提供常用實施，例如`AEMPage` React元件。

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## 檢閱SPA環境變數

數個環境變數必須向遠端SPA公開，以便瞭解如何與AEM互動。

* 在IDE中的`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app`開啟遠端SPA專案
* 開啟檔案`.env.development`
* 在檔案中，請特別留意索引鍵，並視需要更新：

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![遠端SPA環境變數](./assets/spa-bootstrap/env-variables.png)

  *請記住，React中的自訂環境變數必須加上前置詞`REACT_APP_`。*

   * `REACT_APP_HOST_URI`：遠端SPA所連線之AEM服務的配置與主機。
      * 此值會隨著AEM環境（本機、開發、階段或生產）和AEM服務型別（製作與發佈）而改變
   * `REACT_APP_USE_PROXY`：這可透過告知react開發伺服器使用`http-proxy-middleware`模組代理AEM請求（例如`/content, /graphql, .model.json`）來避免開發期間的CORS問題。
   * `REACT_APP_AUTH_METHOD`： AEM服務請求的驗證方法，選項為「service-token」、「dev-token」、「basic」或對no-auth使用案例保留空白
      * 與AEM Author搭配使用時需要使用
      * 可能需要搭配AEM Publish使用（如果內容受到保護）
      * 針對AEM SDK進行開發時，可透過基本驗證支援本機帳戶。 這是本教學課程中使用的方法。
      * 與AEM as a Cloud Service整合時，請使用[存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   * `REACT_APP_BASIC_AUTH_USER`： SPA要驗證的AEM __使用者名稱__，擷取AEM內容。
   * `REACT_APP_BASIC_AUTH_PASS`： SPA的AEM __密碼__&#x200B;在擷取AEM內容時用來進行驗證。

## 整合ModelManager API

使用應用程式可用的AEM SPA npm相依性，在叫用`ReactDOM.render(...)`之前，在專案的`index.js`中初始化AEM的`ModelManager`。

[ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts)負責連線至AEM以擷取可編輯的內容。

1. 在IDE中開啟遠端SPA專案
1. 開啟檔案`src/index.js`
1. 新增匯入`ModelManager`，並在`root.render(..)`引動之前將其初始化，

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

`src/index.js`檔案應該如下所示：

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 設定內部SPA代理

建立可編輯的SPA時，最好在SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)中設定[內部Proxy，將其設定為將適當的要求路由傳送至AEM。 這是使用[http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm模組完成的，此模組已由基礎WKND GraphQL應用程式安裝。

1. 在IDE中開啟遠端SPA專案
1. 在`src/proxy/setupProxy.spa-editor.auth.basic.js`開啟檔案
1. 使用以下程式碼更新檔案：

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   `setupProxy.spa-editor.auth.basic.js`檔案應該如下所示：

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   此Proxy設定主要執行兩項作業：

   1. 代理對AEM `http://localhost:4502`的SPA (`http://localhost:3000`)提出的特定要求
      * 它只會代理其路徑符合模式的請求，這些模式指出它們應該由AEM提供服務，如`toAEM(path, req)`中所定義。
      * 它會將SPA路徑重寫至相對應的AEM頁面，如`pathRewriteToAEM(path, req)`中所定義
   1. 它會將CORS標頭新增至所有請求，以允許存取`res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`所定義的AEM內容
      * 如果未新增此專案，在SPA中載入AEM內容時就會發生CORS錯誤。

1. 開啟檔案`src/setupProxy.js`
1. 檢閱指向`setupProxy.spa-editor.auth.basic` Proxy設定檔案的行：

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

請注意，對`src/setupProxy.js`或其參考檔案所做的任何變更都需要重新啟動SPA。

## 靜態SPA資源

靜態SPA資源（例如WKND標誌和載入圖形）需要更新其src URL，以強制從遠端SPA主機載入。 如果保持相對位置，在SPA Editor中載入SPA進行製作時，這些URL預設會使用AEM的主機而非SPA，因此產生404個請求，如下圖所示。

![中斷的靜態資源](./assets/spa-bootstrap/broken-static-resource.png)

若要解決此問題，請讓遠端SPA託管的靜態資源使用包含遠端SPA來源的絕對路徑。

1. 在IDE中開啟SPA專案
1. 開啟SPA的環境變數檔案`src/.env.development`，並為SPA的公用URI新增變數：

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _部署至AEM as a Cloud Service時，您必須對對應的`.env`檔案執行相同的操作。_

1. 開啟檔案`src/App.js`
1. 從SPA的環境變數匯入SPA的公用URI

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. 在WKND標誌`<img src=.../>`前面加上`REACT_APP_PUBLIC_URI`，以強制對SPA進行解析。

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. 在`src/components/Loading.js`中載入影像時執行相同操作

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. 針對`src/components/AdventureDetails.js`中上一頁按鈕的&#x200B;__兩個執行個體__

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

`App.js`、`Loading.js`和`AdventureDetails.js`檔案應該如下所示：

![靜態資源](./assets/spa-bootstrap/static-resources.png)

## AEM回應式格線

若要為SPA中的可編輯區域支援SPA編輯器的配置模式，我們必須將AEM的回應式格線CSS整合至SPA。 別擔心 — 此格線系統僅適用於可編輯的容器，而您可以使用您選擇的格線系統來驅動SPA其餘部分的版面。

將AEM回應式格線SCSS檔案新增至SPA。

1. 在IDE中開啟SPA專案
1. 將下列兩個檔案下載並複製到`src/styles`
   * [_格線.scss](./assets/spa-bootstrap/_grid.scss)
      * AEM回應式格線SCSS產生器
   * [_格線 — init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * 使用SPA的特定中斷點（桌上型電腦和行動裝置）和資料行(12)叫用`_grid.scss`。
1. 開啟`src/App.scss`並匯入`./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

`_grid.scss`和`_grid-init.scss`檔案應該如下所示：

![AEM回應式格線SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

現在，SPA包含必要的CSS，以便針對新增至AEM容器的元件支援AEM的佈局模式。

## 公用程式類別

將下列公用程式類別複製到您的React應用程式專案中。

* [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js)到`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
* [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js)至`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
* [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js)至`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
* [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js)至`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![遠端SPA公用程式類別](./assets/spa-bootstrap/utility-classes.png)

## 啟動SPA

現在SPA已啟動以與AEM整合，接下來讓我們執行SPA並檢視外觀！

1. 在命令列上，導覽至SPA專案的根目錄
1. 使用一般命令啟動SPA （如果尚未完成）

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. 在[http://localhost:3000](http://localhost:3000)上瀏覽SPA。 應該一切正常！

在http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)上執行的![SPA

## 在AEM SPA編輯器中開啟SPA

在[http://localhost:3000](http://localhost:3000)上執行SPA時，讓我們使用AEM SPA Editor來開啟它。 SPA中還沒有可編輯的專案，這只會驗證AEM中的SPA。

1. 登入AEM Author
1. 導覽至&#x200B;__網站> WKND應用程式>我們> en__
1. 選取&#x200B;__WKND應用程式首頁__&#x200B;並點選&#x200B;__編輯__，SPA就會顯示。

   ![編輯WKND應用程式首頁](./assets/spa-bootstrap/edit-home.png)

1. 使用右上角的模式切換器切換至&#x200B;__預覽__
1. 在SPA周圍按一下

   在http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)上執行的![SPA

## 恭喜！

您已啟動遠端SPA，使其與AEM SPA Editor相容！ 您現在知道如何：

* 將AEM SPA Editor JS SDK npm相依性新增至SPA專案
* 設定SPA的環境變數
* 將ModelManager API與SPA整合
* 為SPA設定內部Proxy，以便將適當的內容請求路由至AEM
* 解決在SPA編輯器內容中解析靜態SPA資源的問題
* 新增AEM的回應式格線CSS以支援AEM可編輯容器的版面配置

## 後續步驟

我們與AEM SPA Editor的相容性已達底線，我們可以開始引進可編輯區域了。 我們先來看如何在SPA中放置[固定可編輯元件](./spa-fixed-component.md)。
