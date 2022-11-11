---
title: Bootstrap遠端SPA for SPA編輯器
description: 了解如何引導遠端SPA for AEM SPA編輯器相容性。
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1200'
ht-degree: 0%

---

# Bootstrap遠端SPA for SPA編輯器

可編輯區域可新增至遠端SPA之前，必須先以AEM SPA編輯器JavaScript SDK及一些其他設定引導。

## 安裝AEM SPA Editor JS SDK npm相依性

首先，檢閱React專案的AEM SPA npm相依性並加以安裝。

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) :提供從AEM擷取內容的API。
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) :提供將AEM內容對應至SPA元件的API。
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) :提供用於建置自訂SPA元件的API，並提供常用實作，例如 `AEMPage` 反應元件。

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## 檢閱SPA環境變數

數個環境變數必須公開給遠端SPA，以便其了解如何與AEM互動。

1. 在開啟遠端SPA專案 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` 在IDE中
1. 開啟檔案 `.env.development`
1. 在檔案中，請特別注意索引鍵，並視需要更新：

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![遠端SPA環境變數](./assets/spa-bootstrap/env-variables.png)

   *請記住，React中的自訂環境變數必須加上前置詞 `REACT_APP_`.*

   + `REACT_APP_HOST_URI`:遠端SPA所連線之AEM服務的配置和主機。
      + 如果AEM環境（本機、開發、預備或生產）和AEM服務類型（製作與發佈），此值會隨之變更
   + `REACT_APP_USE_PROXY`:這可借由向react開發伺服器告知proxy AEM請求(例如 `/content, /graphql, .model.json` 使用 `http-proxy-middleware` 模組。
   + `REACT_APP_AUTH_METHOD`:AEM已呈現請求的驗證方法、選項為「service-token」、「dev-token」、「basic」，或留空供無驗證使用案例使用
      + 與AEM作者搭配使用必要
      + 與AEM Publish搭配使用可能需要（如果內容受到保護）
      + 針對AEM SDK進行開發可支援透過基本驗證的本機帳戶。 這是本教學課程中使用的方法。
      + 與AEMas a Cloud Service整合時，請使用 [存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`:AEM __用戶名__ ，以在擷取AEM內容時進行驗證。
   + `REACT_APP_BASIC_AUTH_PASS`:AEM __密碼__ ，以在擷取AEM內容時進行驗證。

## 整合ModelManager API

使用應用程式可用的AEM SPA npm相依性，初始化AEM `ModelManager` 在項目的 `index.js` befor `ReactDOM.render(...)` 叫用的是。

此 [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) 負責連線至AEM以擷取可編輯的內容。

1. 在IDE中開啟遠程SPA項目
1. 開啟檔案 `src/index.js`
1. 新增匯入 `ModelManager` 並在之前初始化 `root.render(..)` 調用，

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

此 `src/index.js` 檔案看起來應該像這樣：

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 設定內部SPA代理

建立可編輯的SPA時，最好設定 [SPA中的內部Proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)，此變數會設定為將適當的請求路由至AEM。 這需使用 [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm模組，基本WKND GraphQL應用程式已安裝此模組。

1. 在IDE中開啟遠程SPA項目
1. 在開啟檔案 `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. 使用下列程式碼更新檔案：

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

   此 `setupProxy.spa-editor.auth.basic.js` 檔案看起來應該像這樣：

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   此代理設定執行兩項主要動作：

   1. Proxy對SPA提出的特定請求(`http://localhost:3000`)到AEM `http://localhost:4502`
      + 只有Proxy請求的路徑符合指示AEM應提供這些服務的模式，如 `toAEM(path, req)`.
      + 如中所定義，會將SPA路徑重新寫入對應的AEM頁面 `pathRewriteToAEM(path, req)`
   1. 它會將CORS標題新增至所有請求，以允許存取AEM內容(定義為 `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + 如果未新增，則在SPA中載入AEM內容時會發生CORS錯誤。

1. 開啟檔案 `src/setupProxy.js`
1. 查看指向 `setupProxy.spa-editor.auth.basic` 代理配置檔案：

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

請注意， `src/setupProxy.js` 或者引用的檔案需要重新啟動SPA。

## 靜態SPA資源

靜態SPA資源（例如WKND標誌和載入圖形）需要更新其src URL，以強制從遠端SPA主機載入。 若相對左，當SPA在SPA編輯器中載入以進行編寫時，這些URL預設會使用AEM主機，而非SPA，因此會產生404個要求，如下圖所示。

![中斷的靜態資源](./assets/spa-bootstrap/broken-static-resource.png)

若要解決此問題，請讓遠端SPA托管的靜態資源使用包含遠端SPA來源的絕對路徑。

1. 在IDE中開啟SPA專案
1. 開啟SPA環境變數檔案 `src/.env.development` 並為SPA公用URI新增變數：

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _部署至AEM as a Cloud Service時，您需要對應 `.env` 檔案。_

1. 開啟檔案 `src/App.js`
1. 從SPA環境變數匯入SPA公用URI

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. 為WKND徽標加上前置詞 `<img src=.../>` with `REACT_APP_PUBLIC_URI` 來強制對SPA。

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. 在中載入影像時也請執行相同操作 `src/components/Loading.js`

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

1. 對於 __兩個例項__ 中的後按鈕 `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

此 `App.js`, `Loading.js`，和 `AdventureDetails.js` 檔案應該如下：

![靜態資源](./assets/spa-bootstrap/static-resources.png)

## AEM回應式格線

若要支援SPA編輯器的版面模式，以便在SPA中處理可編輯區域，我們必須將AEM Responsive Grid CSS整合至SPA。 別擔心 — 此網格系統僅適用於可編輯的容器，並且您可以使用所選的網格系統來驅動其餘SPA的佈局。

將AEM回應式格線SCSS檔案新增至SPA。

1. 在IDE中開啟SPA專案
1. 下載以下兩個檔案並複製到 `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEM回應式格線SCSS產生器
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + 調用 `_grid.scss` 使用SPA的特定中斷點（案頭和行動裝置）和欄(12)。
1. 開啟 `src/App.scss` 和匯入 `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

此 `_grid.scss` 和 `_grid-init.scss` 檔案應該如下：

![AEM回應式格線SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

現在，SPA包含新增至AEM容器之元件支援AEM版面模式所需的CSS。

## 實用程式類

將下列公用程式類別中的內容複製到您的React應用程式專案。

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![遠程SPA實用程式類](./assets/spa-bootstrap/utility-classes.png)

## 啟動SPA

現在SPA正在啟動以便與AEM整合，讓我們執行SPA並查看看看看起來是什麼樣子！

1. 在命令列中，導覽至SPA專案的根目錄
1. 使用一般命令啟動SPA（如果尚未執行）

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. 在上瀏覽SPA [http://localhost:3000](http://localhost:3000). 一切都應該好看！

![SPA在http://localhost:3000上執行](./assets/spa-bootstrap/localhost-3000.png)

## 在AEM SPA編輯器中開啟SPA

當SPA執行時 [http://localhost:3000](http://localhost:3000)，請使用AEM SPA編輯器開啟它。 SPA中尚無可編輯的項目，這只會驗證AEM中的SPA。

1. 登入AEM作者
1. 導覽至 __網站> WKND應用程式>我們>結束__
1. 選取 __WKND應用首頁__ 點選 __編輯__，則會顯示SPA。

   ![編輯WKND應用首頁](./assets/spa-bootstrap/edit-home.png)

1. 切換至 __預覽__ 使用右上角的模式切換器
1. 按一下週圍的SPA

   ![SPA在http://localhost:3000上執行](./assets/spa-bootstrap/spa-editor.png)

## 恭喜！

您已引導遠端SPA以相容AEM SPA編輯器！ 您現在知道如何：

+ 將AEM SPA Editor JS SDK npm相依性新增至SPA專案
+ 設定SPA環境變數
+ 將ModelManager API與SPA整合
+ 設定SPA的內部代理，以便將適當的內容要求路由至AEM
+ 解決在SPA Editor中解決靜態SPA資源的問題
+ 新增AEM Responsive Grid CSS以支援AEM可編輯容器中的版面配置

## 後續步驟

現在，我們已取得與AEM SPA Editor相容性的基準，可以開始介紹可編輯的區域。 我們先看看如何 [固定可編輯元件](./spa-fixed-component.md) 在SPA中。
