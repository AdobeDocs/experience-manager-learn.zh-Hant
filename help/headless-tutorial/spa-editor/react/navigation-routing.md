---
title: 添加導航和路由 |開始使用AEM SPA Editor and React
description: 了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router和React Core Components實現的。
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# 添加導航和路由 {#navigation-routing}

了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router和React Core Components實現的。

## 目標

1. 了解使用SPA編輯器時可用的SPA模型路由選項。
1. 了解如何使用 [React路由器](https://reacttraining.com/react-router/) 來導覽SPA的不同檢視。
1. 使用AEM React核心元件來實作由AEM頁面階層驅動的動態導覽。

## 您將建置的

本章將新增導覽至AEM中的SPA。 導覽功能表由AEM頁面階層驅動，且將使用 [導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![新增導覽](assets/navigation-routing/navigation-added.png)

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment). 本章是 [對應元件](map-components.md) 不過，您只需要部署至本機AEM例項的SPA啟用AEM專案，便能順利完成。

## 將導覽新增至範本 {#add-navigation-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/). 應已部署起始代碼庫。
1. 導覽至 **SPA頁面範本**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 選取最外側 **根佈局容器** 按一下 **原則** 表徵圖。 小心 **not** ，選擇 **版面容器** 取消鎖定以製作。

   ![選擇根佈局容器策略表徵圖](assets/navigation-routing/root-layout-container-policy.png)

1. 建立新策略，命名為 **SPA結構**:

   ![SPA結構原則](assets/navigation-routing/spa-policy-update.png)

   在 **允許的元件** > **一般** >選取 **版面容器** 元件。

   在 **允許的元件** > **WKND SPA REACT — 結構** >選取 **導覽** 元件：

   ![選擇導航元件](assets/navigation-routing/select-navigation-component.png)

   在 **允許的元件** > **WKND SPA REACT — 內容** >選取 **影像** 和 **文字** 元件。 您應選取總計4個元件。

   按一下 **完成** 以儲存變更。

1. 重新整理頁面，然後新增 **導覽** 元件上方 **版面容器**:

   ![將導航元件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 選取 **導覽** 元件並按一下 **原則** 表徵圖可編輯策略。
1. 使用 **策略標題** of **SPA導覽**.

   在 **屬性**:

   * 設定 **導覽根目錄** to `/content/wknd-spa-react/us/en`.
   * 設定 **排除根層級** to **1**.
   * 取消選中 **收集所有子頁**.
   * 設定 **導覽結構深度** to **3**.

   ![配置導航策略](assets/navigation-routing/navigation-policy.png)

   這會收集下方深處的導覽2層 `/content/wknd-spa-react/us/en`.

1. 儲存變更後，應會看到填入 `Navigation` 作為範本的一部分：

   ![填入的導覽元件](assets/navigation-routing/populated-navigation.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，作為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型的階層結構。

1. 導覽至 **網站** 主控台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 選取 **WKND SPA React首頁** 按一下 **建立** > **頁面**:

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 在 **範本** 選取 **SPA頁面**. 在 **屬性** 輸入 **第1頁** 針對 **標題** 和 **page-1** 作為名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下 **建立** 在對話方塊快顯視窗中，按一下 **開啟** 以在AEM SPA編輯器中開啟頁面。

1. 新增 **文字** 元件至主 **版面容器**. 編輯元件並輸入文本： **第1頁** 使用RTE和 **H2** 元素。

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

1. 返回AEM Sites主控台，並重複上述步驟，建立名為 **第2頁** 作為 **第1頁**.
1. 最後建立第三個頁面， **第3頁** 但作為 **子項** of **第2頁**. 完成網站階層後，應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 導覽元件現在可用來導覽至SPA的不同區域。

   ![導航和路由](assets/navigation-routing/navigation-working.gif)

1. 在AEM編輯器外部開啟頁面： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 使用 **導覽** 元件，導覽至應用程式的不同檢視。

1. 在您導覽時，使用瀏覽器的開發人員工具來檢查網路請求。 以下螢幕擷取畫面是從Google Chrome瀏覽器擷取。

   ![觀察網路請求](assets/navigation-routing/inspect-network-requests.png)

   請注意，在初始頁面載入後，後續導覽不會導致完整頁面重新整理，且當返回至先前造訪的頁面時，網路流量會最小化。

## 階層頁面JSON模型 {#hierarchy-page-json-model}

接下來，檢查可帶來SPA多檢視體驗的JSON模型。

1. 在新索引標籤中，開啟AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 將瀏覽器擴充功能用於 [JSON格式](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   首次載入SPA時，會要求此JSON內容。 外部結構如下所示：

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   在 `:children` 您應該會看到已建立每個頁面的項目。 所有頁面的內容都位於此初始JSON要求中。 使用導覽路由，SPA的後續檢視會快速載入，因為內容已在用戶端提供。

   載入不明智 **全部** 的SPA內容，因為這會減緩初始頁面載入速度。 接下來，我們將查看頁面階層深度的收集方式。

1. 導覽至 **SPA根** 範本： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   按一下 **頁面屬性功能表** > **頁面原則**:

   ![開啟SPA根的頁面原則](assets/navigation-routing/open-page-policy.png)

1. 此 **SPA根** 範本有額外的 **階層結構** 標籤來控制收集的JSON內容。 此 **結構深度** 決定網站階層中收集下方子頁面的深度 **根**. 您也可以使用 **結構模式** 欄位，根據規則運算式篩選其他頁面。

   更新 **結構深度** to **2**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下 **完成** 以保存對策略的更改。

1. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   請注意， **第3頁** 路徑已移除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 從初始JSON模型。 這是因為 **第3頁** 位於階層中的第3層，我們已更新原則，僅包含最大深度為第2層的內容。

1. 重新開啟SPA首頁： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 並開啟瀏覽器的開發人員工具。

   重新整理頁面，您應該會看到 `/content/wknd-spa-react/us/en.model.json`，即SPA根。 請注意，根據先前在教學課程中對SPA根範本進行的階層深度設定，僅包含三個子頁面。 這不包括 **第3頁**.

   ![初始JSON要求 — SPA根](assets/navigation-routing/initial-json-request.png)

1. 開啟開發人員工具後，請使用 `Navigation` 元件直接導覽至 **第3頁**:

   請注意，系統已對下列項目提出新的XHR請求： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理員了解 **第3頁** JSON內容無法使用，並自動觸發其他XHR要求。

1. 直接導覽至： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 另請注意，瀏覽器的返回按鈕可繼續運作。

## Inspect React Routing  {#react-routing}

導航和路由的實現方式為 [React路由器](https://reactrouter.com/). React Router是React應用程式的導航元件集合。 [AEM React核心元件](https://github.com/adobe/aem-react-core-wcm-components-base) 使用React Router的功能來實施 **導覽** 元件。

接下來，檢查React Router與SPA的整合情況，並使用React Router進行實驗 [連結](https://reactrouter.com/web/api/Link) 元件。

1. 在IDE中開啟檔案 `index.js` at `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
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
   ```

   請注意， `App` 包裝在 `Router` 元件 [React路由器](https://reacttraining.com/react-router/). 此 `ModelManager`，由AEM SPA Editor JS SDK提供，會根據JSON模型API將動態路由新增至AEM頁面。

1. 開啟檔案 `Page.js` at `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   此 `Page` SPA元件使用 `MapTo` 函式映射 **頁面** 在AEM中轉換為對應的SPA元件。 此 `withRoute` 公用程式可協助您根據 `cqPath` 屬性。

1. 開啟 `Header.js` 元件於 `ui.frontend/src/components/Header/Header.js`.
1. 更新 `Header` 來包住 `<h1>` 標籤 [連結](https://reactrouter.com/web/api/Link) 前往首頁：

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   而非使用預設值 `<a>` 我們使用的錨記 `<Link>` 由React路由器提供。 只要 `to=` 指向有效的路由，SPA將切換到該路由， **not** 執行完整頁面重新整理。 在此，我們只需硬式編碼首頁的連結，以說明的使用 `Link`.

1. 在更新測試 `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由於我們使用的是React Router的靜態元件， `App.js` 我們需要更新單元測試來解釋。

1. 開啟終端機，導覽至專案的根目錄，並使用您的Maven技能將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至AEM中SPA的其中一個頁面： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   而非使用 `Navigation` 要導覽的元件，請在 `Header`.

   ![標題連結](assets/navigation-routing/header-link.png)

   請注意，完整的頁面重新整理為 **not** 已觸發，且SPA路由正常運作。

1. （可選）嘗試 `Header.js` 使用標準 `<a>` 錨點標籤：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   這有助於說明SPA路由與一般網頁連結之間的差異。

## 恭喜！ {#congratulations}

恭喜您，您已了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導覽已使用React Router實作，並新增至 `Header` 元件。
