---
title: 添加導航和路由 |開始使用AEM SPA Editor and React
description: 了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。
sub-product: Sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2115'
ht-degree: 0%

---


# 添加導航和路由{#navigation-routing}

了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。

## 目標

1. 了解使用SPA編輯器時可用的SPA模型路由選項。
1. 了解如何使用[React Router](https://reacttraining.com/react-router/)在SPA的不同視圖之間導航。
1. 實作由AEM頁面階層驅動的動態導覽。

## 您將建置的

本章將向現有`Header`元件添加導航菜單。 導覽功能表將由AEM頁面階層驅動，且會使用[導覽核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 安裝傳統[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)的已完成包。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像將在WKND SPA上重新使用。 可使用[AEM套件管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`React/navigation-routing-solution`在本機檢出程式碼。

## Inspect標題更新{#inspect-header}

在前幾章中，通過`App.js`將`Header`元件添加為純React元件。 在本章中，`Header`元件已移除，將透過[範本編輯器](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)新增。 這可讓使用者從AEM內設定`Header`的導覽功能表。

>[!NOTE]
>
> 已對程式碼基底進行數次CSS和JavaScript更新，以啟動本章節。 若要著重於核心概念，不討論程式碼變更的&#x200B;**all**。 您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)查看完整更改。[

1. 在您選擇的IDE中，開啟本章的SPA入門項目。
1. 在`ui.frontend`模組下方，檢查檔案`Header.js` ，網址為：`ui.frontend/src/components/Header/Header.js`。

   已進行多項更新，包括添加`HeaderEditConfig`和`MapTo`以使元件映射到AEM元件`wknd-spa-react/components/header`。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. 在`ui.apps`模組中，檢查AEM `Header`元件的元件定義：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   AEM `Header`元件將透過`sling:resourceSuperType`屬性繼承[導航核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)的所有功能。

## 將標題添加到模板{#add-header-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
1. 導覽至&#x200B;**SPA頁面範本**:[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 選擇最外層&#x200B;**根佈局容器**，然後按一下其&#x200B;**策略**&#x200B;表徵圖。 請留意&#x200B;**不**&#x200B;以選取&#x200B;**版面容器**&#x200B;未鎖定以供編寫。

   ![選擇根佈局容器策略表徵圖](assets/navigation-routing/root-layout-container-policy.png)

1. 建立名為&#x200B;**SPA Structure**&#x200B;的新策略：

   ![SPA結構原則](assets/navigation-routing/spa-policy-update.png)

   在「**允許的元件**」>「**一般**」下，選擇「**佈局容器**」元件。

   在&#x200B;**允許的元件** > **WKND SPA REACT - STRUCTURE**&#x200B;下，選擇&#x200B;**標題**&#x200B;元件：

   ![選擇標題元件](assets/navigation-routing/select-header-component.png)

   在「**允許的元件** > **WKND SPA REACT — 內容**」下，選擇&#x200B;**Image**&#x200B;和&#x200B;**Text**&#x200B;元件。 您應選取總計4個元件。

   按一下&#x200B;**Done**&#x200B;以儲存變更。

1. 重新整理頁面，並在取消鎖定&#x200B;**配置容器**&#x200B;的上方新增&#x200B;**Header**&#x200B;元件：

   ![將標題元件添加到模板](./assets/navigation-routing/add-header-component.gif)

1. 選擇&#x200B;**Header**&#x200B;元件，然後按一下其&#x200B;**Policy**&#x200B;表徵圖以編輯策略。
1. 使用&#x200B;**** WKND SPA標題&#x200B;**的策略標題**&#x200B;建立新策略。

   在&#x200B;**Properties**&#x200B;下：

   * 將&#x200B;**導航根**&#x200B;設定為`/content/wknd-spa-react/us/en`。
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**收集所有子頁**。
   * 將&#x200B;**導航結構深度**&#x200B;設定為&#x200B;**3**。

   ![配置標頭策略](assets/navigation-routing/header-policy.png)

   這將收集`/content/wknd-spa-react/us/en`下方深處的導覽2層。

1. 儲存變更後，範本中應該會顯示已填入的`Header`:

   ![填充的標題元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，作為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型的階層結構。

1. 導覽至&#x200B;**Sites**&#x200B;主控台：[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 選擇&#x200B;**WKND SPA React首頁**，然後按一下&#x200B;**Create** > **Page**:

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 在&#x200B;**Template**&#x200B;下，選擇&#x200B;**SPA Page**。 在&#x200B;**Properties**&#x200B;下，以&#x200B;**Title**&#x200B;和&#x200B;**page-1**&#x200B;為名稱輸入&#x200B;**Page 1**。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下&#x200B;**建立**，然後在對話方塊快顯視窗中，按一下&#x200B;**開啟**&#x200B;以在AEM SPA編輯器中開啟頁面。

1. 將新的&#x200B;**Text**&#x200B;元件添加到主&#x200B;**佈局容器**&#x200B;中。 編輯元件並輸入文本：**使用RTE和** H1 **元素的頁面1**（您必須進入全螢幕模式才能變更段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

1. 返回AEM Sites主控台，並重複上述步驟，建立名為&#x200B;**Page 2**&#x200B;的第二個頁面，作為&#x200B;**Page 1**&#x200B;的同層級。
1. 最後，建立第三個頁面，**第3**&#x200B;頁，但作為&#x200B;**第2**&#x200B;頁的&#x200B;**子**。 完成網站階層後，應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 在新索引標籤中，開啟AEM提供的JSON模型API:[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 首次載入SPA時，會要求此JSON內容。 外部結構如下所示：

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

   在`:children`下，應該會看到每個已建立頁面的項目。 所有頁面的內容都位於此初始JSON要求中。 一旦實作導覽路由，後續的SPA檢視就會快速載入，因為內容已可在用戶端使用。

   在初始JSON要求中載入SPA內容的&#x200B;**ALL**&#x200B;並不明智，因為這會減緩初始頁面載入速度。 接下來，讓我們查看頁面階層深度的收集方式。

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

   注意&#x200B;**Page 3**&#x200B;路徑已移除：`/content/wknd-spa-react/us/en/home/page-2/page-3`。

   稍後，我們將觀察AEM SPA Editor SDK如何動態載入其他內容。

## 實作導覽

接下來，將導覽功能表作為`Header`的一部分實施。 我們可以直接在`Header.js`中新增程式碼，但最佳作法是避免使用大型元件。 反之，我們將實作`Navigation` SPA元件，稍後可能會重新使用。

1. 檢閱AEM `Header`元件公開的JSON，網址為[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

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

   AEM頁面的階層性質是以JSON為模型，可用來填入導覽功能表。 回想一下，`Header`元件繼承[導覽核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)的所有功能，透過JSON公開的內容會自動對應至React prop。

1. 開啟新的終端機視窗，並導覽至SPA專案的`ui.frontend`資料夾。 使用命令`npm start`啟動&#x200B;**webpack-dev-server**。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. 開啟新的瀏覽器標籤並導覽至[http://localhost:3000/](http://localhost:3000/)。

   應將&#x200B;**webpack-dev-server**&#x200B;設定為從AEM的本機例項(`ui.frontend/.env.development`)代理JSON模型。 這可讓我們針對先前練習中在AEM中建立的內容直接編寫程式碼。 請確定您已在相同瀏覽工作階段中驗證至AEM。

   ![功能表切換工作](./assets/navigation-routing/nav-toggle-static.gif)

   `Header`目前已實作功能表切換功能。 接下來，實作導覽功能表。

1. 返回您選擇的IDE，在`ui.frontend/src/components/Header/Header.js`處開啟`Header.js`。
1. 更新`homeLink()`方法以移除硬式編碼字串，並使用AEM元件傳入的動態屬性：

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

   上述程式碼會根據元件設定的根導覽項目填入url。 `homeLink()` 可用來填入方法中的標 `logo()` 志，並用來判斷是否應在中顯示上 `backButton()`一頁按鈕。

   保存對`Header.js`的更改。

1. 在`Header.js`頂端新增一行，將`Navigation`元件匯入至其他匯入項目下方：

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. 接下來更新`get navigation()`方法以實例化`Navigation`元件：

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   如前所述，我們不會在`Header`內實作導覽，而是在`Navigation`元件中實作大部分邏輯。  `Header`的prop包含建置功能表所需的JSON結構，我們會傳遞所有prop。
1. 在`ui.frontend/src/components/Navigation/Navigation.js`開啟檔案`Navigation.js`。
1. 實作`renderGroupNav(children)`方法：

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

   此方法會取用導覽項目的陣列`children`，並建立未排序的清單。 然後，它會反覆陣列，並將項目傳遞至`renderNavItem`，以便後續實作。

1. 實作`renderNavItem`:

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

   此方法會呈現清單項目，其中CSS類基於屬性`level`和`active`。 然後，方法會呼叫`renderLink`以建立錨點標籤。 由於`Navigation`內容是分層的，因此使用遞歸策略來調用當前項的子項的`renderGroupNav`。

1. 實作`renderLink`方法：

   在檔案頂部添加React路由器的[Link](https://reacttraining.com/react-router/web/api/Link)元件的導入方法，並添加其他導入：

   ```js
   import {Link} from "react-router-dom";
   ```

   接下來完成`renderLink`方法的實施：

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   請注意，系統使用[Link](https://reacttraining.com/react-router/web/api/Link)元件，而非一般錨點標籤`<a>`。 這可確保不會觸發完整的頁面重新整理，而是會利用AEM SPA Editor JS SDK提供的React路由器。

1. 儲存對`Navigation.js`的變更並返回&#x200B;**webpack-dev-server**:[http://localhost:3000](http://localhost:3000)

   ![已完成的頁首導航](assets/navigation-routing/completed-header.png)

   按一下功能表切換，開啟導覽，您應該會看到填入的導覽連結。 您應該可以導覽至SPA的不同檢視。

## Inspect SPA路由

現在導覽已實作完畢，請檢查AEM中的路由。

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

1. 開啟終端機，導覽至專案的根目錄，並使用您的Maven技能將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至AEM中的SPA首頁：[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)並開啟瀏覽器的開發人員工具。 以下螢幕擷取畫面是從Google Chrome瀏覽器擷取。

   重新整理頁面，您應該會看到`/content/wknd-spa-react/us/en.model.json`(即SPA根)的XHR請求。 請注意，根據先前在教學課程中對SPA根範本進行的階層深度設定，僅包含三個子頁面。 這不包括&#x200B;**第3**&#x200B;頁。

   ![初始JSON要求 — SPA根](assets/navigation-routing/initial-json-request.png)

1. 開啟開發人員工具後，使用`Header`導覽導覽至&#x200B;**頁面3**:

   ![第3頁導航](assets/navigation-routing/page-three-navigation.png)

   請注意，系統已對下列項目提出新的XHR請求：`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager了解JSON內容不可用，並自動觸發其他XHR要求。****

1. 使用`Header`元件的各種導覽連結繼續導覽SPA。 請注意，系統未提出其他XHR請求，且未發生完整頁面重新整理。 這可讓使用者快速取得SPA，並減少傳回AEM的不必要請求。

   ![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

1. 直接導覽至：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 請注意，瀏覽器的返回按鈕可繼續運作。

## 恭喜！ {#congratulations}

恭喜您，您已了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航已使用React Router實現，並添加到`Header`元件中。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`React/navigation-routing-solution`在本機檢出程式碼。
