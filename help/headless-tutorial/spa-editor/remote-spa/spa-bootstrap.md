---
title: Bootstrap遠端SPA for SPA編輯器
description: 瞭解如何啟動啟動遠端SPA，以符合AEM SPA Editor。
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 390
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# Bootstrap遠端SPA for SPA編輯器

將可編輯區域新增到遠端SPA之前，必須使用AEM SPA編輯器JavaScript SDK和其他幾項設定來啟動可編輯區域。

## 安裝AEM SPA Editor JS SDK npm相依性

首先，檢閱React專案的AEM SPA npm相依性，並加以安裝。

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) ：提供從AEM擷取內容的API。
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) ：提供將AEM內容對應至SPA元件的API。
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) ：提供用於建置自訂SPA元件的API，並提供常用的實施，例如 `AEMPage` React元件。

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## 檢閱SPA環境變數

有些環境變數必須向遠端SPA公開，以便知道如何與AEM互動。

1. 開啟遠端SPA專案，位於 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` 在您的IDE中
1. 開啟檔案 `.env.development`
1. 在檔案中，請特別留意索引鍵，並視需要更新：

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![遠端SPA環境變數](./assets/spa-bootstrap/env-variables.png)

   *請記住，React中的自訂環境變數必須加上前置詞 `REACT_APP_`.*

   + `REACT_APP_HOST_URI`：遠端SPA連線之AEM服務的配置和主機。
      + 此值會隨著AEM環境（本機、開發、階段或生產）和AEM服務型別（製作與發佈）而改變
   + `REACT_APP_USE_PROXY`：這可透過告知react開發伺服器代理AEM請求(例如 `/content, /graphql, .model.json` 使用 `http-proxy-middleware` 模組。
   + `REACT_APP_AUTH_METHOD`：適用於AEM服務請求的驗證方法，選項為「service-token」、「dev-token」、「basic」或對「無驗證」使用案例保留空白
      + 與AEM Author搭配使用時需要使用
      + 可能需要搭配AEM Publish使用（如果內容受到保護）
      + 針對AEM SDK進行開發時，可透過基本驗證支援本機帳戶。 這是本教學課程中使用的方法。
      + 與AEMas a Cloud Service整合時，請使用 [存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`：AEM __使用者名稱__ SPA在擷取AEM內容時進行身分驗證。
   + `REACT_APP_BASIC_AUTH_PASS`：AEM __密碼__ SPA在擷取AEM內容時進行身分驗證。

## 整合ModelManager API

使用應用程式可用的AEM SPA npm相依性，初始化AEM `ModelManager` 在專案的 `index.js` 早於 `ReactDOM.render(...)` 叫用的是。

此 [模型管理員](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) 負責連線至AEM以擷取可編輯的內容。

1. 在IDE中開啟遠端SPA專案
1. 開啟檔案 `src/index.js`
1. 新增匯入 `ModelManager` 並在 `root.render(..)` 引動過程，

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

此 `src/index.js` 檔案應如下所示：

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 設定內部SPA Proxy

建立可編輯的SPA時，最好設定 [SPA中的內部Proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)，這設定為將適當的要求路由傳送至AEM。 這是透過使用完成的 [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm模組，已由基本WKND GraphQL應用程式安裝。

1. 在IDE中開啟遠端SPA專案
1. 開啟檔案於 `src/proxy/setupProxy.spa-editor.auth.basic.js`
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

   此 `setupProxy.spa-editor.auth.basic.js` 檔案應如下所示：

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   此Proxy設定主要執行兩項作業：

   1. 代理對SPA提出的特定請求(`http://localhost:3000`)到AEM `http://localhost:4502`
      + 它只會代理其路徑符合模式的請求，這些模式表示這些請求應該由AEM提供服務（如中的定義） `toAEM(path, req)`.
      + 它會將SPA路徑重寫至其對應的AEM頁面，如中所定義 `pathRewriteToAEM(path, req)`
   1. 它會將CORS標頭新增至所有請求，以允許存取AEM內容（如所定義） `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + 如果未新增此專案，在SPA中載入AEM內容時會發生CORS錯誤。

1. 開啟檔案 `src/setupProxy.js`
1. 檢閱指向 `setupProxy.spa-editor.auth.basic` proxy設定檔：

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

注意，任何變更 `src/setupProxy.js` 或其引用的檔案需要重新啟動SPA。

## 靜態SPA資源

靜態SPA資源（例如WKND標誌和載入圖形）需要更新其src URL，以強制從遠端SPA主機載入。 如果保持為相對，在SPA編輯器中載入SPA以進行編寫時，這些URL預設為使用AEM主機而不是SPA，這會產生404個請求，如下圖所示。

![中斷的靜態資源](./assets/spa-bootstrap/broken-static-resource.png)

若要解決此問題，請讓遠端SPA託管的靜態資源使用包含遠端SPA來源的絕對路徑。

1. 在IDE中開啟SPA專案
1. 開啟SPA環境變數檔案 `src/.env.development` 並為SPA公用URI新增變數：

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _部署至AEMas a Cloud Service時，您需要 `.env` 檔案。_

1. 開啟檔案 `src/App.js`
1. 從SPA環境變數匯入SPA公用URI

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. 為WKND標誌加上前置詞 `<img src=.../>` 替換為 `REACT_APP_PUBLIC_URI` 以強制針對SPA解決問題。

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. 在中載入影像時執行相同操作 `src/components/Loading.js`

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

1. 對於 __兩個執行個體__ 中的「上一步」按鈕 `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

此 `App.js`， `Loading.js`、和 `AdventureDetails.js` 檔案應如下所示：

![靜態資源](./assets/spa-bootstrap/static-resources.png)

## AEM回應式格線

若要為SPA中的可編輯區域支援SPA編輯器的版面模式，我們必須將AEM回應式格線CSS整合至SPA。 別擔心 — 此格線系統僅適用於可編輯的容器，而您可以使用您選擇的格線系統來驅動SPA其餘部分的版面。

將AEM回應式格線SCSS檔案新增至SPA。

1. 在IDE中開啟SPA專案
1. 下載下列兩個檔案並複製到中 `src/styles`
   + [_格線.scss](./assets/spa-bootstrap/_grid.scss)
      + AEM回應式格線SCSS產生器
   + [_格線 — init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + 叫用 `_grid.scss` 使用SPA特定的中斷點（案頭和行動瀏覽器）和欄(12)。
1. 開啟 `src/App.scss` 並匯入 `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

此 `_grid.scss` 和 `_grid-init.scss` 檔案應如下所示：

![AEM回應式格線SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

現在，SPA包含支援新增至AEM容器之元件的AEM佈局模式所需的CSS。

## 公用程式類別

將下列公用程式類別複製到您的React應用程式專案中。

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) 至 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [editorplaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) 至 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) 至 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withstandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) 至 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![遠端SPA公用程式類別](./assets/spa-bootstrap/utility-classes.png)

## 啟動SPA

現在SPA已啟動以與AEM整合，接著執行SPA並檢視外觀！

1. 在命令列上，導覽至SPA專案的根目錄
1. 使用一般命令啟動SPA （如果尚未完成）

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. 瀏覽SPA於 [http://localhost:3000](http://localhost:3000). 應該一切正常！

![在http://localhost:3000上執行的SPA](./assets/spa-bootstrap/localhost-3000.png)

## 在SPA SPA編輯器中開啟AEM

當SPA執行於時 [http://localhost:3000](http://localhost:3000)，接著使用AEM SPA Editor將其開啟。 在SPA中尚無法編輯任何專案，這只會驗證AEM中的SPA。

1. 登入AEM Author
1. 瀏覽至 __網站> WKND應用程式>我們>英文__
1. 選取 __WKND應用程式首頁__ 然後點選 __編輯__，則會出現SPA。

   ![編輯WKND應用程式首頁](./assets/spa-bootstrap/edit-home.png)

1. 切換至 __預覽__ 使用右上方的模式切換器
1. 在SPA周圍按一下

   ![在http://localhost:3000上執行的SPA](./assets/spa-bootstrap/spa-editor.png)

## 恭喜！

您已啟動遠端SPA，使其與AEM SPA Editor相容！ 您現在知道如何：

+ 將AEM SPA編輯器JS SDK npm相依性新增至SPA專案
+ 設定您的SPA環境變數
+ 將ModelManager API與SPA整合
+ 為SPA設定內部Proxy，以便將適當的內容要求路由傳送至AEM
+ 解決在SPA編輯器內容中解析靜態SPA資源的問題
+ 新增AEM回應式格線CSS以支援AEM可編輯容器中的版面配置

## 後續步驟

我們與AEM SPA Editor的相容性已達底線，現在可以開始匯入可編輯區域了。 我們先來看一下如何放置 [固定可編輯元件](./spa-fixed-component.md) 在SPA中。
