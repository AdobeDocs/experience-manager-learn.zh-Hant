---
title: 新增導覽和路由 | AEM SPA編輯器快速入門與回應
description: 透過SPA編輯器SDK對應至AEM頁面，瞭解如何支援SPA中的多個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 0%

---


# 新增導覽和路由 {#navigation-routing}

透過SPA編輯器SDK對應至AEM頁面，瞭解如何支援SPA中的多個檢視。 動態導航是使用React Router實現的，並添加到現有的Header元件中。

## 目標

1. 瞭解使用SPA編輯器時可用的SPA模型路由選項。
2. 瞭解如何使 [用React Router](https://reacttraining.com/react-router/) ，在SPA的不同視圖之間導航。
3. 實作由AEM頁面階層驅動的動態導覽。

## 您將建立的

本章將向現有元件添加導航 `Header` 菜單。 導覽功能表將由AEM頁面階層所驅動，並將運用「導覽核心元件」提供的 [JSON模型](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)。

![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 必備條件

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

2. 使用Maven將程式碼庫部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，請新增 `classic` 描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 為傳統的 [WKND參考站點安裝完成的軟體包](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKND參考網 [站提供的影像](https://github.com/adobe/aem-guides-wknd/releases/latest) ，將重新用於WKND SPA。 您可使用 [AEM的Package Manager來安裝套件](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ，或切換至分支，在本機檢出程式碼 `React/navigation-routing-solution`。

## 檢查頁首更新 {#inspect-header}

在前幾章中，元 `Header` 件是以純React元件的形式加入，透過 `App.js`。 在本章中，元件 `Header` 已被刪除，將通過模板編輯器 [添加](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)。 這可讓使用者在AEM中設定導 `Header` 覽功能表。

>[!NOTE]
>
> 已對程式碼庫進行數項CSS和JavaScript更新，以開始本章節。 為了集中討論核心概念，並 **非所有** 、程式碼的變更。 您可以在這裡檢視完整 [變更](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)。

1. 在您選擇的IDE中，開啟本章的SPA啟動程式項目。
2. 在模組 `ui.frontend` 下方，檢查檔案 `Header.js` 位置： `ui.frontend/src/components/Header/Header.js`.

   已進行數項更新，包括新增 `HeaderEditConfig` 和 `MapTo` 以讓元件對應至AEM元件 `wknd-spa-react/components/header`。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

3. 在模 `ui.apps` 塊中檢查AEM元件的元件定 `Header` 義： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   AEM元 `Header` 件將透過屬性繼承 [Navigation Core Component的所有](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)`sling:resourceSuperType` 功能。

## 將頁首新增至範本 {#add-header-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
2. 導覽至 **SPA頁面範本**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. 選取最外層的「根版 **面配置容器」** ，然後按一下 **「原則** 」圖示。 請小 **心不要選取** 「版面容器 **」(Layout Container** )解除鎖定以進行製作。

   ![選取根配置容器原則圖示](assets/navigation-routing/root-layout-container-policy.png)

4. 建立名為 **SPA結構的新策略**:

   ![SPA結構政策](assets/navigation-routing/spa-policy-update.png)

   在「允 **許的元件** >一般 **」(Allowed Components** ) **>選取「** 版面容器」(Layout Container)元件。

   在「 **允許的元件** > **WKND SPA REACT - STRUCTURE** >選取標題元 **件** :

   ![選擇標題元件](assets/navigation-routing/select-header-component.png)

   在「 **允許的元件** > **WKND SPA REACT —— 內容** >選取影 **像** 和文字 **** 元件」下。 您應選取總共4個元件。

   Click **Done** to save the changes.

5. 重新整理頁面，並在未鎖定的 **版面容器上方新增** 標題元件 ****:

   ![將標題元件新增至範本](./assets/navigation-routing/add-header-component.gif)

6. 選擇標 **頭元件** ，然後按一下其 **** 策略表徵圖以編輯策略。
7. 使用 **WKND SPA標題的策略標題** ，創 **建新策略**。

   在「屬 **性**:

   * 將「導 **覽根** 」設為 `/content/wknd-spa-react/us/en`。
   * 將「排 **除根層級** 」設 **為1**。
   * Uncheck **Collect all child pages**.
   * 將「導 **覽結構深度** 」設 **為3**。

   ![配置標題策略](assets/navigation-routing/header-policy.png)

   這會收集深處的導覽2層級 `/content/wknd-spa-react/us/en`。

8. 儲存變更後，您應將填入的變 `Header` 更視為範本的一部分：

   ![填入的標題元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接著，在AEM中建立其他頁面，做為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型階層結構。

1. 導覽至 **Sites** Console: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 選擇 **WKND SPA React首頁** ，然後單 **擊建立****>**&#x200B;頁：

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

2. 在「模 **板** 」下 **選擇「SPA頁面」**。 在「 **屬性** 」下， **為Title** - **1輸入Page 1，並****** 將page-1作為名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一 **下「建立** 」，然後在對話方塊快顯視窗中，按一下「 **開啟** 」以在AEM SPA編輯器中開啟頁面。

3. 將新的 **Text** 元件新增至主 **版面容器**。 編輯元件並輸入文本： **第1頁** (使用RTE和 **H1** 元素)（您必須進入全屏模式才能更改段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

4. 返回AEM Sites主控台並重複上述步驟，建立名為 **Page 2** 的第二頁，作為 **Page 1的同級**。
5. 最後，請建立第三頁( **第3頁** )，但 **以第** 2頁 **的子**&#x200B;頁身分。 完成網站階層後，網站階層應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新標籤中，開啟AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 首次載入SPA時，會要求此JSON內容。 外部結構如下所示：

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

   在下 `:children` 方，您應該會看到每個已建立頁面的項目。 所有頁面的內容都在此初始JSON請求中。 一旦實施導航路由，SPA的後續視圖將被快速載入，因為內容已經可在客戶端使用。

   在初始JSON請求中載 **入SPA的ALL** ，並不明智，因為這會減緩初始頁面載入的速度。 接下來，讓我們來瞭解如何收集頁面的階層深度。

7. 導覽至 **SPA根模板** ，網址為： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   按一下「 **頁面屬性」功能表** > **頁面原則**:

   ![開啟SPA根目錄的頁面原則](assets/navigation-routing/open-page-policy.png)

8. **SPA根範本** ，有額外的「階層 **** 結構」索引標籤來控制所收集的JSON內容。 「結 **構深度** 」可決定網站階層中根目錄下收集子頁面的深 **度**。 您也可以使用「結 **構模式」欄位** ，根據規則運算式篩選掉其他頁面。

   將「結構 **深度** 」更新 **為2**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一 **下「完成** 」以儲存對原則所做的變更。

9. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

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

   請注意， **Page 3** path已移除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 從初始JSON模型。

   稍後，我們將觀察AEM SPA Editor SDK如何動態載入其他內容。

## 實作導覽

接著，實作導覽功能表作為的一部分 `Header`。 我們可以直接在中加入程式碼， `Header.js` 但更好的做法是避免使用大型元件。 相反地，我們將實作 `Navigation` SPA元件，稍後可能會重新使用。

1. 檢視AEM元件公開的JSON，網址 `Header` 為 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

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

   AEM頁面的階層性質是以JSON為模型，可用來填入導覽功能表。 請記得， `Header` 元件繼承了導覽核心元件的所有功能 [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) ，透過JSON公開的內容會自動對應至React prop。

2. 開啟新的終端窗口並導航到SPA `ui.frontend` 項目的資料夾。 使用 **命令啟動webpack-dev-server**`npm start`。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

3. 開啟新的瀏覽器標籤並導覽至 [http://localhost:3000/](http://localhost:3000/)。

   應 **將webpack-dev-server** 設定為從AEM()的本機例項代理JSON模型`ui.frontend/.env.development`。 這可讓我們直接針對先前練習中在AEM中建立的內容進行程式碼編寫。 請確定您已在相同的瀏覽工作階段中驗證為AEM。

   ![菜單切換工作](./assets/navigation-routing/nav-toggle-static.gif)

   目前 `Header` 的功能表切換功能已經實作。 接著，實施導覽功能表。

4. 返回您選擇的IDE，並開啟 `Header.js` at `ui.frontend/src/components/Header/Header.js`。
5. 更新 `homeLink()` 方法以移除硬式編碼字串，並使用AEM元件傳入的動態prop:

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

   上述程式碼將根據元件所設定的根導覽項目填入URL。 `homeLink()` 用於在方法中填入標 `logo()` 志，並用於確定是否應在中顯示背面按鈕 `backButton()`。

   保存更改 `Header.js`。

6. 在頂部添加一行，以 `Header.js` 將元件導入 `Navigation` 到其他導入的下方：

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

7. 下一步更新 `get navigation()` 方法以實例化 `Navigation` 元件：

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   如前所述，我們將實施元件中的大 `Header` 部分邏輯，而不是在元件中實施導 `Navigation` 航。  prop包含建 `Header` 立功能表所需的JSON結構，我們會傳遞所有prop。
8. 在中開啟 `Navigation.js` 檔案 `ui.frontend/src/components/Navigation/Navigation.js`。
9. 實作方 `renderGroupNav(children)` 法：

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

   此方法會取用導覽項目的陣列， `children`並建立未排序的清單。 然後，它會重複陣列，並將項目傳遞至 `renderNavItem`，接下來將實施。

10. 實作 `renderNavItem`:

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

   此方法會轉譯清單項目，其中CSS類別會根據屬性 `level` 和 `active`。 然後，方法會 `renderLink` 呼叫以建立錨點標籤。 由於內 `Navigation` 容是分層的，因此使用遞歸策略來調 `renderGroupNav` 用當前項的子項。

11. 實作方 `renderLink` 法：

   在檔案頂部為 [Link](https://reacttraining.com/react-router/web/api/Link) 元件（React路由器的一部分）添加導入方法，同時導入其他元件：

   ```js
   import {Link} from "react-router-dom";
   ```

   接下來完成該方法的 `renderLink` 實現：

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   請注意，使用的不是一般錨點標 `<a>`簽，而 [是Link](https://reacttraining.com/react-router/web/api/Link) 元件。 這可確保不會觸發完整頁面的重新整理，而是運用AEM SPA編輯器JS SDK提供的React路由器。

12. 將更改保 `Navigation.js` 存並返回 **webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![完成的頁首導覽](assets/navigation-routing/completed-header.png)

   按一下功能表切換以開啟導覽，您應該會看到填入的導覽連結。 您應該可以導覽至SPA的不同檢視。

## 檢查SPA路由

現在已實作導覽，請在AEM中檢查路由。

1. 在IDE中，開啟文 `index.js` 件 `ui.frontend/src/index.js`。

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

   請注意， `App` React Router的組 `Router` 件包 [裝了它](https://reacttraining.com/react-router/)。 由 `ModelManager`AEM SPA編輯器JS SDK提供的動態路由會根據JSON模型API新增至AEM頁面。

2. 開啟終端機、導覽至專案的根目錄，並使用您的Maven技巧將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 導覽至AEM中的SPA首頁： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) ，並開啟您瀏覽器的開發人員工具。 以下螢幕擷取自Google Chrome瀏覽器。

   重新整理頁面，您應該會看到XHR要求， `/content/wknd-spa-react/us/en.model.json`即SPA根目錄。 請注意，根據本教學課程前面所做的SPA根範本的階層深度設定，僅包含三個子頁面。 這不包括第 **3頁**。

   ![初始JSON請求- SPA根目錄](assets/navigation-routing/initial-json-request.png)

4. 開啟開發人員工具後，使用導 `Header` 覽導覽至第 **3頁**:

   ![第3頁導覽](assets/navigation-routing/page-three-navigation.png)

   請注意，新的XHR要求是： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理員瞭解 **Page 3** JSON內容不可用，並自動觸發其他XHR請求。

5. 使用元件的各種導覽連結，繼續導覽SPA `Header` 。 請注意，未提出其他XHR要求，且未進行完整頁面重新整理。 這可讓使用者快速取得SPA，並減少不必要的回AEM要求。

   ![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

6. 直接導覽至： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 請注意，瀏覽器的「上一步」按鈕仍能繼續運作。

## 恭喜！ {#congratulations}

恭喜您，您瞭解如何透過SPA編輯器SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航已使用React Router實現，並添加到元件 `Header` 中。

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ，或切換至分支，在本機檢出程式碼 `React/navigation-routing-solution`。
