---
title: 添加導航和路由 |從編輯器AEM開始SPA並反應
description: 瞭解使用編輯器SPASDK映射到頁AEM面可以支SPA持多個視圖。 動態導航是使用React Router和React Core元件實現的。
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

瞭解使用編輯器SPASDK映射到頁AEM面可以支SPA持多個視圖。 動態導航是使用React Router和React Core元件實現的。

## 目標

1. 瞭解使SPA用編輯器時可用的模型路SPA由選項。
1. 學習使用 [反應路由器](https://reacttraining.com/react-router/) 在不同視圖之間導SPA航。
1. 使用AEMReact Core Components實現由頁面層次驅動的動AEM態導航。

## 您將構建的

本章將將導航添加到SPA中AEM。 導航菜單由頁AEM面層次結構驅動，並將利用 [導航核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)。

![已添加導航](assets/navigation-routing/navigation-added.png)

## 必備條件

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。 本章是 [映射元件](map-components.md) 但是，本章要遵循所有需要的，都是SPA已啟AEM用的項目部署到本AEM地實例。

## 將導航添加到模板 {#add-navigation-template}

1. 開啟瀏覽器並登錄AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署啟動代碼庫。
1. 導航到 **頁SPA面模板**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 選擇最外層 **根佈局容器** 點擊 **策略** 表徵圖 小心 **不** 的 **佈局容器** 未鎖定以創作。

   ![選擇根佈局容器策略表徵圖](assets/navigation-routing/root-layout-container-policy.png)

1. 建立名為 **結SPA構**:

   ![結SPA構策略](assets/navigation-routing/spa-policy-update.png)

   下 **允許的元件** > **常規** >選擇 **佈局容器** 元件。

   下 **允許的元件** > **WKND反SPA應 — 結構** >選擇 **導航** 元件：

   ![選擇導航元件](assets/navigation-routing/select-navigation-component.png)

   下 **允許的元件** > **WKND反SPA應 — 內容** >選擇 **影像** 和 **文本** 元件。 您應選擇4個總元件。

   按一下「**完成**」以儲存變更。

1. 刷新頁面，然後添加 **導航** 未鎖定的元件上方 **佈局容器**:

   ![將導航元件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 選擇 **導航** 元件，按一下 **策略** 表徵圖以編輯策略。
1. 使用 **策略標題** 共 **導SPA航**。

   在 **屬性**:

   * 設定 **導航根** 至 `/content/wknd-spa-react/us/en`。
   * 設定 **排除根級別** 至 **1**。
   * 取消選中 **收集所有子頁**。
   * 設定 **導航結構深度** 至 **3**。

   ![配置導航策略](assets/navigation-routing/navigation-policy.png)

   這將收集深處的導航2層 `/content/wknd-spa-react/us/en`。

1. 保存更改後，您應看到已填充的 `Navigation` 作為模板的一部分：

   ![填充的導航元件](assets/navigation-routing/populated-navigation.png)

## 建立子頁

接下來，在中創AEM建將作為中不同視圖的附SPA加頁。 我們還將檢查提供的JSON模型的層次結構AEM。

1. 導航到 **站點** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 選擇 **WKND反SPA應首頁** 按一下 **建立** > **頁面**:

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 下 **模板** 選擇 **頁SPA面**。 下 **屬性** 輸入 **第1頁** 為 **標題** 和 **第1頁** 的下界。

   ![輸入初始頁屬性](assets/navigation-routing/initial-page-properties.png)

   按一下 **建立** 並在對話框彈出窗口中按一下 **開啟** 開啟AEM頁SPA面。

1. 添加新 **文本** 主元件 **佈局容器**。 編輯元件並輸入文本： **第1頁** 使用RTE和 **H2** 的子菜單。

   ![示例內容第1頁](assets/navigation-routing/page-1-sample-content.png)

   可以隨意添加其他內容，如影像。

1. 返回到AEM Sites控制台並重複上述步驟，建立名為 **第2頁** 作為 **第1頁**。
1. 最後建立第三頁， **第3頁** 但作為 **孩子** 共 **第2頁**。 完成後，站點層次結構應如下所示：

   ![示例站點層次結構](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 現在，導航元件可用於導航到的不同區SPA域。

   ![導航和路由](assets/navigation-routing/navigation-working.gif)

1. 在編輯器外部打AEM開頁面： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)。 使用 **導航** 元件，導航到應用的不同視圖。

1. 在導航時，使用瀏覽器的開發人員工具檢查網路請求。 下面的螢幕截圖是從GoogleChrome瀏覽器捕獲的。

   ![觀察網路請求](assets/navigation-routing/inspect-network-requests.png)

   請注意，在載入初始頁面後，後續導航不會導致全頁刷新，並且當返回到以前訪問過的頁面時，網路通信量將最小化。

## 層次結構頁JSON模型 {#hierarchy-page-json-model}

接下來，檢查驅動多視圖體驗的JSON模SPA型。

1. 在新頁籤中，開啟以下提供的JSON模型APIAEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 使用瀏覽器擴展可能有助於 [格式化JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)。

   首次載入時請求SPA此JSON內容。 外部結構如下所示：

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

   下 `:children` 您應看到建立的每個頁面的條目。 所有頁的內容都在此初始JSON請求中。 通過導航路由，快速加SPA載該內容的後續視圖，因為該內容已經可用於客戶端。

   裝載不明智 **全部** 初始JSON請SPA求中的內容，因為這會降低初始頁載入速度。 接下來，讓我們看看如何收集頁面的層次結構深度。

1. 導航到 **根SPA** 模板： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)。

   按一下 **頁面屬性菜單** > **頁面策略**:

   ![開啟根的頁面策SPA略](assets/navigation-routing/open-page-policy.png)

1. 的 **根SPA** 模板有額外的 **層次結構** 頁籤，以控制收集的JSON內容。 的 **結構深度** 確定在站點層次結構中收集子頁的深度 **根**。 您還可以使用 **結構模式** 欄位，用於根據規則運算式篩選其他頁。

   更新 **結構深度** 至 **2**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下 **完成** 的子菜單。

1. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

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

   請注意 **第3頁** 路徑已刪除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 從初始JSON模型。 這是因為 **第3頁** 位於層次中的第3級，我們更新了策略，以僅包含位於第2級的最大深度的內容。

1. 重新開啟主SPA頁： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 並開啟瀏覽器的開發人員工具。

   刷新頁面，您應看到XHR請求 `/content/wknd-spa-react/us/en.model.json`，即根SPA。 請注意，根據本教程前面所做的「根」模板的層次深度配置，SPA只包括三個子頁。 這不包括 **第3頁**。

   ![初始JSON請求 — 根SPA](assets/navigation-routing/initial-json-request.png)

1. 開啟開發人員工具後，使用 `Navigation` 元件直接導航到 **第3頁**:

   請注意，新的XHR請求是： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   模AEM型經理明白 **第3頁** JSON內容不可用，並自動觸發附加XHR請求。

1. 通過直接導航到： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 另請注意瀏覽器的後退按鈕繼續工作。

## Inspect反應路由  {#react-routing}

導航和路由通過 [反應路由器](https://reactrouter.com/)。 React Router是React應用程式的導航元件的集合。 [反AEM應核心元件](https://github.com/adobe/aem-react-core-wcm-components-base) 使用React Router的功能來實現 **導航** 元件。

接下來，檢查React Router如何與整合，並SPA使用React Router的 [連結](https://reactrouter.com/web/api/Link) 元件。

1. 在IDE中開啟檔案 `index.js` 在 `ui.frontend/src/index.js`。

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

   請注意 `App` 包裹在 `Router` 元件 [反應路由器](https://reacttraining.com/react-router/)。 的 `ModelManager`，由編AEM輯器SPAJS SDK提供，根據JSON模型API將動態路AEM由添加到頁面。

1. 開啟檔案 `Page.js` 在 `ui.frontend/src/components/Page/Page.js`

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

   的 `Page` 組SPA件使用 `MapTo` 函式映射 **頁面** 的AEM子SPA菜單。 的 `withRoute` 實用程式可幫助根據SPA相應AEM的子頁動態路由 `cqPath` 屬性。

1. 開啟 `Header.js` 元件 `ui.frontend/src/components/Header/Header.js`。
1. 更新 `Header` 來包裝 `<h1>` 標籤 [連結](https://reactrouter.com/web/api/Link) 至首頁：

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

   而不是使用預設 `<a>` 我們使用的錨標籤 `<Link>` 由React Router提供。 只要 `to=` 指向有效路由，SPA將切換到該路由 **不** 執行全頁刷新。 在這裡，我們只需對指向首頁的連結進行硬編碼，以說明 `Link`。

1. 更新test的位置 `App.test.js` 在 `ui.frontend/src/App.test.js`。

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由於我們使用的是在中引用的靜態元件中使用React Router的功能 `App.js` 我們需要更新設備test，以計算。

1. 開啟終端，導航到項目的根，然後部署項目以使用AEMMaven技能：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導航到中的SPA一頁AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   而不是使用 `Navigation` 要導航的元件，請使用 `Header`。

   ![標題連結](assets/navigation-routing/header-link.png)

   觀察整個頁面刷新 **不** 觸發，路SPA由工作。

1. （可選） `Header.js` 使用標準 `<a>` 錨點標籤：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   這有助於說明路由和常規網SPA頁連結之間的區別。

## 恭喜！ {#congratulations}

祝賀您，您已經瞭解了通過SPAEditor SDK映射到頁面可以支AEM持中的多SPA個視圖。 已使用React Router實現動態導航，並將其添加到 `Header` 元件。
