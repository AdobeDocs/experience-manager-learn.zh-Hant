---
title: 添加導航和路由 |開始使用AEM SPA Editor and React
description: 了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router和React Core Components實現的。
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1619'
ht-degree: 0%

---

# 添加導航和路由 {#navigation-routing}

了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router和React Core Components實現的。

## 目標

1. 了解使用SPA編輯器時可用的SPA模型路由選項。
1. 了解如何使用[React Router](https://reacttraining.com/react-router/)在SPA的不同視圖之間導航。
1. 使用AEM React核心元件來實作由AEM頁面階層驅動的動態導覽。

## 您將建置的

本章將新增導覽至AEM中的SPA。 導覽功能表將由AEM頁面階層驅動，且會使用[導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![新增導覽](assets/navigation-routing/navigation-added.png)

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。 本章是[映射元件](map-components.md)章節的繼續，但是，您只需要部署至本機AEM例項的SPA啟用AEM專案，便能順利完成。

## 將導覽新增至範本 {#add-navigation-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
1. 導覽至&#x200B;**SPA頁面範本**:[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 選擇最外層&#x200B;**根佈局容器**，然後按一下其&#x200B;**策略**&#x200B;表徵圖。 請留意&#x200B;**不**&#x200B;以選取&#x200B;**版面容器**&#x200B;未鎖定以供編寫。

   ![選擇根佈局容器策略表徵圖](assets/navigation-routing/root-layout-container-policy.png)

1. 建立名為&#x200B;**SPA Structure**&#x200B;的新策略：

   ![SPA結構原則](assets/navigation-routing/spa-policy-update.png)

   在「**允許的元件**」>「**一般**」下，選擇「**佈局容器**」元件。

   在&#x200B;**允許的元件** > **WKND SPA REACT - STRUCTURE**&#x200B;下，選擇&#x200B;**導航**&#x200B;元件：

   ![選擇導航元件](assets/navigation-routing/select-navigation-component.png)

   在「**允許的元件** > **WKND SPA REACT — 內容**」下，選擇&#x200B;**Image**&#x200B;和&#x200B;**Text**&#x200B;元件。 您應選取總計4個元件。

   按一下&#x200B;**Done**&#x200B;以儲存變更。

1. 重新整理頁面，並在取消鎖定的&#x200B;**配置容器**&#x200B;上方新增&#x200B;**Navigation**&#x200B;元件：

   ![將導航元件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 選擇&#x200B;**導航**&#x200B;元件，然後按一下其&#x200B;**策略**&#x200B;表徵圖以編輯策略。
1. 使用&#x200B;**SPA導航**&#x200B;的&#x200B;**策略標題**&#x200B;建立新策略。

   在&#x200B;**Properties**&#x200B;下：

   * 將&#x200B;**導航根**&#x200B;設定為`/content/wknd-spa-react/us/en`。
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**收集所有子頁**。
   * 將&#x200B;**導航結構深度**&#x200B;設定為&#x200B;**3**。

   ![配置導航策略](assets/navigation-routing/navigation-policy.png)

   這將收集`/content/wknd-spa-react/us/en`下方深處的導覽2層。

1. 儲存變更後，範本中應該會顯示已填入的`Navigation`:

   ![填入的導覽元件](assets/navigation-routing/populated-navigation.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，作為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型的階層結構。

1. 導覽至&#x200B;**Sites**&#x200B;主控台：[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 選擇&#x200B;**WKND SPA React首頁**，然後按一下&#x200B;**Create** > **Page**:

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 在&#x200B;**Template**&#x200B;下，選擇&#x200B;**SPA Page**。 在&#x200B;**Properties**&#x200B;下，以&#x200B;**Title**&#x200B;和&#x200B;**page-1**&#x200B;為名稱輸入&#x200B;**Page 1**。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下&#x200B;**建立**，然後在對話方塊快顯視窗中，按一下&#x200B;**開啟**&#x200B;以在AEM SPA編輯器中開啟頁面。

1. 將新的&#x200B;**Text**&#x200B;元件添加到主&#x200B;**佈局容器**&#x200B;中。 編輯元件並輸入文本：**使用RTE和** H2 **元素的第1**&#x200B;頁。

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

1. 返回AEM Sites主控台，並重複上述步驟，建立名為&#x200B;**Page 2**&#x200B;的第二個頁面，作為&#x200B;**Page 1**&#x200B;的同層級。
1. 最後，建立第三個頁面，**第3**&#x200B;頁，但作為&#x200B;**第2**&#x200B;頁的&#x200B;**子**。 完成網站階層後，應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 導覽元件現在可用來導覽至SPA的不同區域。

   ![導航和路由](assets/navigation-routing/navigation-working.gif)

1. 在AEM編輯器外部開啟頁面：[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)。 使用&#x200B;**導覽**&#x200B;元件來導覽至應用程式的不同檢視。

1. 在您導覽時，使用瀏覽器的開發人員工具來檢查網路請求。 以下螢幕擷取畫面是從Google Chrome瀏覽器擷取。

   ![觀察網路請求](assets/navigation-routing/inspect-network-requests.png)

   請注意，在初始頁面載入後，後續導覽不會導致完整頁面重新整理，且當返回至先前造訪的頁面時，網路流量會最小化。

## 階層頁面JSON模型 {#hierarchy-page-json-model}

接下來，檢查可帶來SPA多檢視體驗的JSON模型。

1. 在新索引標籤中，開啟AEM提供的JSON模型API:[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 使用瀏覽器擴充功能將[格式化為JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)可能會很有幫助。

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

   在`:children`下，應該會看到每個已建立頁面的項目。 所有頁面的內容都位於此初始JSON要求中。 使用導覽路由，後續的SPA檢視將會快速載入，因為內容已在用戶端提供。

   在初始JSON要求中載入SPA內容的&#x200B;**ALL**&#x200B;並不明智，因為這會減緩初始頁面載入速度。 接下來，我們將查看頁面階層深度的收集方式。

1. 導覽至&#x200B;**SPA根**&#x200B;範本：[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)。

   按一下&#x200B;**頁面屬性功能表** > **頁面原則**:

   ![開啟SPA根的頁面原則](assets/navigation-routing/open-page-policy.png)

1. **SPA根**&#x200B;範本有額外的&#x200B;**階層結構**&#x200B;標籤，可控制收集的JSON內容。 **「結構深度」**&#x200B;決定了網站階層中的深度，以收集&#x200B;**根**&#x200B;下方的子頁面。 您也可以使用&#x200B;**結構模式**&#x200B;欄位，根據規則運算式篩除其他頁面。

   將&#x200B;**結構深度**&#x200B;更新為&#x200B;**2**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下&#x200B;**Done**&#x200B;以保存對策略的更改。

1. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

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

   注意&#x200B;**Page 3**&#x200B;路徑已移除：`/content/wknd-spa-react/us/en/home/page-2/page-3`。 這是因為&#x200B;**頁面3**&#x200B;位於階層中的第3層，我們更新了原則，僅包含最大深度為第2層的內容。

1. 重新開啟SPA首頁：[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)並開啟瀏覽器的開發人員工具。

   重新整理頁面，您應該會看到`/content/wknd-spa-react/us/en.model.json`(即SPA根)的XHR請求。 請注意，根據先前在教學課程中對SPA根範本進行的階層深度設定，僅包含三個子頁面。 這不包括&#x200B;**第3**&#x200B;頁。

   ![初始JSON要求 — SPA根](assets/navigation-routing/initial-json-request.png)

1. 開啟開發人員工具後，使用`Navigation`元件直接導覽至&#x200B;**第3**&#x200B;頁：

   請注意，系統已對下列項目提出新的XHR請求：`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager了解JSON內容不可用，並自動觸發其他XHR要求。****

1. 直接導覽至：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 另請注意，瀏覽器的返回按鈕可繼續運作。

## Inspect React Routing  {#react-routing}

導航和路由採用[React Router](https://reactrouter.com/)實現。 React Router是React應用程式的導航元件集合。 [AEM React核心](https://github.com/adobe/aem-react-core-wcm-components-base) 元件使用React Router的功能來實 **** 施前述步驟中使用的Navigation元件。

接下來，檢查React Router與SPA的整合情況，並使用React Router的[Link](https://reactrouter.com/web/api/Link)元件進行實驗。

1. 在IDE中，在`ui.frontend/src/index.js`處開啟檔案`index.js`。

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

   注意`App`被包在[React Router](https://reacttraining.com/react-router/)的`Router`元件中。 `ModelManager`由AEM SPA Editor JS SDK提供，會根據JSON模型API將動態路由新增至AEM頁面。

1. 在`ui.frontend/src/components/Page/Page.js`開啟檔案`Page.js`

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

   `Page` SPA元件使用`MapTo`函式將AEM中的&#x200B;**Pages**&#x200B;對應至對應的SPA元件。 `withRoute`公用程式可協助根據`cqPath`屬性以動態方式將SPA路由至適當的AEM子頁面。

1. 在`ui.frontend/src/components/Header/Header.js`開啟`Header.js`元件。
1. 更新`Header`以將[Link](https://reactrouter.com/web/api/Link)中的`<h1>`標籤包裝到首頁：

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

   我們不使用React Router提供的預設`<a>`錨點標籤，而是使用`<Link>`。 只要`to=`指向有效的路由，SPA就會切換到該路由，而&#x200B;**not**&#x200B;會執行完整的頁面刷新。 在此，我們只需將首頁的連結硬式編碼，以說明`Link`的使用方式。

1. 在`ui.frontend/src/App.test.js`更新測試。`App.test.js`

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由於我們在`App.js`中引用的靜態元件內使用React Router的功能，因此需要更新單元測試以考慮該功能。

1. 開啟終端機，導覽至專案的根目錄，並使用您的Maven技能將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至AEM中SPA的其中一個頁面：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   請使用`Header`中的連結，而非使用`Navigation`元件進行導覽。

   ![標題連結](assets/navigation-routing/header-link.png)

   請注意，已觸發完整頁面重新整理為&#x200B;**not**，且SPA路由正在運作。

1. 或者，使用標準`<a>`錨點標籤來實驗`Header.js`檔案：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   這有助於說明SPA路由與一般網頁連結之間的差異。

## 恭喜！ {#congratulations}

恭喜您，您已了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航已使用React Router實現，並添加到`Header`元件中。
