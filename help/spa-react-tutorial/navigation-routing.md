---
title: 新增導覽和路由 |編輯與回應快AEM速入SPA門
description: 瞭解使用Editor SDK對應SPA至「頁面」可支AEM援多SPA個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。
sub-product: Sites
feature: 編SPA輯器
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2117'
ht-degree: 0%

---


# 添加導航和路由{#navigation-routing}

瞭解使用Editor SDK對應SPA至「頁面」可支AEM援多SPA個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。

## 目標

1. 瞭解使SPA用編輯器時可用的模型路SPA由選項。
1. 瞭解如何使用[ React Router](https://reacttraining.com/react-router/)在不同視圖之間導航SPA。
1. 實作由頁面階層驅動的動AEM態導覽。

## 您將建立的

本章將向現有`Header`元件添加導航菜單。 導覽功能表將由頁AEM面階層驅動，並會使用[導覽核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. 使用Maven將程式碼庫部署AEM至本機例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 為傳統[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)安裝完成的軟體包。 由[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像將重新用於WKNDSPA。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安裝軟體包。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`React/navigation-routing-solution`，在本機檢出程式碼。

## Inspect標題更新{#inspect-header}

在前幾章中，`Header`元件被新增為透過`App.js`所包含的純React元件。 在本章中，`Header`元件已被刪除，將通過[模板編輯器](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)添加。 這可讓使用者從中設定`Header`的導覽功能表AEM。

>[!NOTE]
>
> 已對程式碼庫進行數項CSS和JavaScript更新，以開始本章節。 為了集中討論核心概念，不討論代碼更改的&#x200B;**all**。 您可以在[這裡](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)檢視完整變更。

1. 在您選擇的IDE中，開啟本章SPA的入門項目。
1. 在`ui.frontend`模組下，檢查檔案`Header.js` :`ui.frontend/src/components/Header/Header.js`。

   已進行了幾項更新，包括添加`HeaderEditConfig`和`MapTo`以便將元件映射到AEM`wknd-spa-react/components/header`元件。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. 在`ui.apps`模組中，檢查`Header`組AEM件的元件定義：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   &lt;a0/AEM>元件將通過`sling:resourceSuperType`屬性繼承[導航核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)的所有功能。`Header`

## 將標題新增至範本{#add-header-template}

1. 開啟瀏覽器並登AEM入[http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
1. 導覽至&#x200B;**頁SPA面範本**:[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 選擇最外層的&#x200B;**根佈局容器** ，然後按一下其&#x200B;**策略**&#x200B;表徵圖。 請務必&#x200B;**not**，以選擇&#x200B;**版面容器**&#x200B;未鎖定以進行編寫。

   ![選取根配置容器原則圖示](assets/navigation-routing/root-layout-container-policy.png)

1. 建立名為&#x200B;**SPA Structure**&#x200B;的新策略：

   ![結SPA構政策](assets/navigation-routing/spa-policy-update.png)

   在「**允許的元件** > **一般** >」下，選擇「版面容器&#x200B;**」元件。**

   在「**允許的元件** > **WKND SPA REACT - STRUCTURE** 」下，選擇&#x200B;**標題**&#x200B;元件：

   ![選擇標題元件](assets/navigation-routing/select-header-component.png)

   在「**允許的元件** > **WKND SPA REACT —— 內容**」下，選擇&#x200B;**Image**&#x200B;和&#x200B;**Text**&#x200B;元件。 您應選取總共4個元件。

   按一下&#x200B;**Done**&#x200B;保存更改。

1. 重新整理頁面，並在未鎖定的&#x200B;**版面容器**&#x200B;上方新增&#x200B;**Header**&#x200B;元件：

   ![將標題元件新增至範本](./assets/navigation-routing/add-header-component.gif)

1. 選擇&#x200B;**標題**&#x200B;元件，然後按一下其&#x200B;**策略**&#x200B;表徵圖以編輯策略。
1. 使用&#x200B;**WKND標題**&#x200B;的&#x200B;**策略標題**&#x200B;建立SPA新策略。

   在&#x200B;**屬性**&#x200B;下：

   * 將&#x200B;**導覽根**&#x200B;設為`/content/wknd-spa-react/us/en`。
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**收集所有子頁**。
   * 將&#x200B;**導覽結構深度**&#x200B;設為&#x200B;**3**。

   ![配置標題策略](assets/navigation-routing/header-policy.png)

   這會收集`/content/wknd-spa-react/us/en`下方深處的導覽2層級。

1. 儲存變更後，您應將填入的`Header`視為範本的一部分：

   ![填入的標題元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接著，在中建立AEM其他頁面，做為中的不同檢視SPA。 我們也會檢查由提供的JSON模型階層結構AEM。

1. 導覽至&#x200B;**Sites**&#x200B;主控台：[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 選擇&#x200B;**WKND SPA React Home Page** ，然後按一下&#x200B;**Create** > **Page** :

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 在&#x200B;**Template**&#x200B;下，選擇&#x200B;**SPA Page**。 在&#x200B;**Properties**&#x200B;下輸入&#x200B;**Page 1**&#x200B;作為&#x200B;**Title**&#x200B;和&#x200B;**page-1**&#x200B;的名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下&#x200B;**建立** ，然後在對話框彈出窗口中按一下&#x200B;**開啟**&#x200B;以在編輯器中開啟該AEM頁SPA。

1. 將新的&#x200B;**Text**&#x200B;元件新增至主&#x200B;**版面容器**。 編輯元件並輸入文本：**使用RTE和** H1 **元素的第1**&#x200B;頁（您必須進入全螢幕模式才能變更段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

1. 返回AEM Sites控制台並重複上述步驟，建立名為&#x200B;**Page 2**&#x200B;的第二頁，作為&#x200B;**Page 1**&#x200B;的同級。
1. 最後，建立第三頁，**第3**&#x200B;頁，但作為&#x200B;**第2**&#x200B;頁的&#x200B;**子**。 完成網站階層後，網站階層應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 在新索引標籤中，開啟下列人員提供的JSON模型APIAEM:[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 首次載入時會要求SPA此JSON內容。 外部結構如下所示：

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

   在`:children`下，您應該會看到每個已建立頁面的項目。 所有頁面的內容都在此初始JSON請求中。 一旦實施導航路由，則後續的視SPA圖將快速載入，因為內容已經可用於客戶端。

   載入初始JSON請求中的內容&#x200B;**ALL&lt;a1/SPA>並不明智，因為這會減緩初始頁面載入的速度。**&#x200B;接下來，讓我們來瞭解如何收集頁面的階層深度。

1. 導覽至&#x200B;**SPA Root**&#x200B;範本，網址為：[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)。

   按一下&#x200B;**頁面屬性菜單** > **頁面策略**:

   ![開啟根目錄的頁SPA面原則](assets/navigation-routing/open-page-policy.png)

1. **SPA Root**&#x200B;範本有額外的&#x200B;**階層結構**&#x200B;標籤，以控制所收集的JSON內容。 **結構深度**&#x200B;可決定在網站階層中收集&#x200B;**root**&#x200B;下方子頁面的深度。 您也可以使用&#x200B;**結構模式**&#x200B;欄位，根據規則運算式來篩選其他頁面。

   將&#x200B;**結構深度**&#x200B;更新為&#x200B;**2**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下&#x200B;**Done**&#x200B;保存對策略所做的更改。

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

   請注意，**第3頁**&#x200B;路徑已移除：`/content/wknd-spa-react/us/en/home/page-2/page-3`。

   稍後，我們將觀察Editor AEM SDK如何動SPA態載入其他內容。

## 實作導覽

接著，將導覽功能表作為`Header`的一部分實作。 我們可以直接在`Header.js`中新增程式碼，但更好的做法是避免使用大型元件。 相反地，我們將實作`Navigation`元SPA件，稍後可能會重新使用。

1. 檢視`Header`元AEM件在[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)公開的JSON:

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   頁面的階層AEM性質是在JSON中建立模型，可用來填入導覽功能表。 請記住，`Header`元件繼承了[導覽核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)的所有功能，透過JSON公開的內容會自動對應至React prop。

1. 開啟新的終端視窗並導覽至專案的`ui.frontend`資料SPA夾。 使用命令`npm start`啟動&#x200B;**webpack-dev-server**。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. 開啟新的瀏覽器標籤並導覽至[http://localhost:3000/](http://localhost:3000/)。

   應將&#x200B;**webpack-dev-server**&#x200B;設定為從本機例項(`ui.frontend/.env.development`)AEM代理JSON模型。 這可讓我們直接針對先前練習中建立的內AEM容編寫程式碼。 請確定您已在相同的瀏AEM覽工作階段中驗證。

   ![菜單切換工作](./assets/navigation-routing/nav-toggle-static.gif)

   `Header`目前已實作功能表切換功能。 接著，實施導覽功能表。

1. 返回您選擇的IDE，並在`ui.frontend/src/components/Header/Header.js`處開啟`Header.js`。
1. 更新`homeLink()`方法以移除硬式編碼字串，並使用元件傳入的動態AEMprop:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   上述程式碼將根據元件所設定的根導覽項目填入URL。 `homeLink()` 用於在方法中填入標 `logo()` 志，並用於確定是否應在中顯示「上一頁」按鈕 `backButton()`。

   保存對`Header.js`的更改。

1. 在`Header.js`頂部添加一行，將`Navigation`元件導入到其它導入項下：

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. 下一步更新`get navigation()`方法以實例化`Navigation`元件：

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   如前所述，我們將實作`Navigation`元件中的大部分邏輯，而不是在`Header`中實作導覽。  `Header`的prop包含建立功能表所需的JSON結構，我們會傳遞所有prop。
1. 在`ui.frontend/src/components/Navigation/Navigation.js`開啟檔案`Navigation.js`。
1. 實施`renderGroupNav(children)`方法：

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   此方法會取用導覽項目的陣列`children`，並建立未排序的清單。 然後，它會重複陣列，並將項目傳遞至`renderNavItem`，該將在下一個實施。

1. 實施`renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   此方法會轉譯清單項目，其中CSS類別會根據屬性`level`和`active`轉換。 然後，方法會呼叫`renderLink`以建立錨記。 由於`Navigation`內容是分層的，因此使用遞歸策略來調用當前項的子項的`renderGroupNav`。

1. 實施`renderLink`方法：

   將[Link](https://reacttraining.com/react-router/web/api/Link)元件（React路由器的一部分）的導入方法添加到檔案頂部，並導入其他導入：

   ```js
   import {Link} from "react-router-dom";
   ```

   接下來完成`renderLink`方法的實作：

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   請注意，使用[Link](https://reacttraining.com/react-router/web/api/Link)元件而非一般錨記`<a>`。 這可確保不會觸發整頁重新整理，而是運用Editor JS SDK提供的AEMReact路SPA由器。

1. 保存對`Navigation.js`的更改並返回至&#x200B;**webpack-dev-server** :[http://localhost:3000](http://localhost:3000)

   ![完成的頁首導覽](assets/navigation-routing/completed-header.png)

   按一下功能表切換以開啟導覽，您應該會看到填入的導覽連結。 您應該可以導覽至不同的檢視SPA。

## InspectSPA路線

現在已實施導覽，請在中檢查路由AEM。

1. 在IDE中，開啟`index.js`檔案，位於`ui.frontend/src/index.js`。

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

   請注意，`App`被包在[ React Router](https://reacttraining.com/react-router/)的`Router`元件中。 `ModelManager`由Editor JS SDK提供AEM，會根據JSON模型API將動態路AEM由新增至「頁面」。

1. 開啟終端機、導覽至專案的根目錄，並使用您的Maven技AEM能部署專案：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至SPA首頁AEM:[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)並開啟您瀏覽器的開發人員工具。 以下螢幕擷取自Google Chrome瀏覽器。

   重新整理頁面，您應該會看到`/content/wknd-spa-react/us/en.model.json`的XHR請求，此為根SPA目錄。 請注意，根據教學課程中稍早建立的根範本的階層深度設定，僅包含三SPA個子頁面。 這不包括&#x200B;**第3頁**。

   ![初始JSON請求SPA-根](assets/navigation-routing/initial-json-request.png)

1. 開啟開發人員工具後，使用`Header`導覽導覽至&#x200B;**第3頁**:

   ![第3頁導覽](assets/navigation-routing/page-three-navigation.png)

   請注意，新的XHR要求是：`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   模AEM型管理員瞭解&#x200B;**第3頁** JSON內容不可用，並自動觸發其他XHR要求。

1. 使用&lt;a0/SPA>元件的各種導航連結繼續導航。 `Header`請注意，未提出其他XHR要求，且未進行完整頁面重新整理。 如此可讓使SPA用者快速上手，並將不必要的要求減少回AEM來。

   ![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

1. 直接導覽至：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 請注意，瀏覽器的「上一步」按鈕仍能繼續運作。

## 恭喜！{#congratulations}

恭喜您，您瞭解如何透過Editor SPA SDK對應至「頁面」來支AEM援多個SPA檢視。 動態導航已使用React Router實現，並添加到`Header`元件中。

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`React/navigation-routing-solution`，在本機檢出程式碼。
