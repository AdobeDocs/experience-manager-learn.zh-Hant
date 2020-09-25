---
title: 新增導覽和路由 | AEM SPA編輯器與Angular快速入門
description: 瞭解如何使用AEM Pages和SPA Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的Header元件。
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 0%

---


# 新增導覽和路由 {#navigation-routing}

瞭解如何使用AEM Pages和SPA Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的Header元件。

## 目標

1. 瞭解使用SPA編輯器時可用的SPA模型路由選項。
2. 瞭解如何使 [用Angular路由](https://angular.io/guide/router) ，在SPA的不同視圖之間導航。
3. 實作由AEM頁面階層驅動的動態導覽。

## 您將建立的

本章將導航菜單添加到現有組 `Header` 件。 導覽功能表由AEM頁面階層驅動，並使用導覽核心元件提供的 [JSON模型](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)。

![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 必備條件

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
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

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ，或切換至分支，在本機檢出程式碼 `Angular/navigation-routing-solution`。

## 檢查標題元件更新 {#inspect-header}

在前幾章中，元 `HeaderComponent` 件已新增為包含於的純Angular元件 `app.component.html`。 在本章中，元 `HeaderComponent` 件會從應用程式中移除，並會透過範本編輯 [器新增](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)。 這可讓使用者在AEM中設定導 `HeaderComponent` 覽功能表。

>[!NOTE]
>
> 已對程式碼庫進行數項CSS和JavaScript更新，以開始本章節。 為了集中討論核心概念，並 **非所有** 、程式碼的變更。 您可以在這裡檢視完整 [變更](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start)。

1. 在您選擇的IDE中，開啟本章的SPA啟動程式項目。
2. 在模組 `ui.frontend` 下方，檢查檔案 `header.component.ts` 位置： `ui.frontend/src/app/components/header/header.component.ts`.

   已進行數項更新，包括新增 `HeaderEditConfig` 和 `MapTo` 以讓元件對應至AEM元件 `wknd-spa-angular/components/header`。

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   請注意 `@Input()` 的注釋 `items`。 `items` 將包含從AEM傳入的導覽物件陣列。

3. 在模 `ui.apps` 塊中檢查AEM元件的元件定 `Header` 義： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM元 `Header` 件將透過屬性繼承 [Navigation Core Component的所有](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)`sling:resourceSuperType` 功能。

## 將HeaderComponent添加到SPA模板 {#add-header-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
2. 導覽至 **[!UICONTROL SPA頁面範本]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 選取最外層的「根版 **[!UICONTROL 面配置容器」]** ，然後按一下 **[!UICONTROL 「原則]** 」圖示。 請小 **心不要選取** 「版面容器 **[!UICONTROL 」(Layout Container]** )解除鎖定以進行製作。

   ![選取根配置容器原則圖示](assets/navigation-routing/root-layout-container-policy.png)

4. 複製當前策略並建立名為 **[!UICONTROL SPA結構的新策略]**:

   ![SPA結構政策](assets/map-components/spa-policy-update.png)

   在「允 **[!UICONTROL 許的元件]** >一般 **[!UICONTROL 」(Allowed Components]** ) **[!UICONTROL >選取「]** 版面容器」(Layout Container)元件。

   在「 **[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPA角度——結構]** 」(Allowed Components **[!UICONTROL )下，選擇]** 標題元件：

   ![選擇標題元件](assets/map-components/select-header-component.png)

   在「 **[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPA ANGULAR（WKND SPA角度）」下方，選擇「Image]** （影像）」和「 ******** TextAllowed（允許的文本）」元件。 您應選取總共4個元件。

   Click **[!UICONTROL Done]** to save the changes.

5. **重新整理頁面** 。 將Header元 **[!UICONTROL 件新增至]** un-locked **[!UICONTROL Layout Container（未鎖定的版面容器）上]**&#x200B;方：

   ![將標題元件新增至範本](./assets/navigation-routing/add-header-component.gif)

6. 選擇標 **[!UICONTROL 頭元件]** ，然後按一下其 **** 策略表徵圖以編輯策略。

   ![按一下頁首原則](assets/navigation-routing/header-policy-icon.png)

7. 使用「WKND SPA標 **[!UICONTROL 題」的]****「策略標題」建立新策略**。

   在「屬 **[!UICONTROL 性]**:

   * 將「導 **[!UICONTROL 覽根]** 」設為 `/content/wknd-spa-angular/us/en`。
   * 將「排 **[!UICONTROL 除根層級]** 」設 **為1**。
   * 取消選 **[!UICONTROL 中「收集所有子頁面」]**。
   * 將「導 **[!UICONTROL 覽結構深度]** 」設 **為3**。

   ![配置標題策略](assets/navigation-routing/header-policy.png)

   這會收集深處的導覽2層級 `/content/wknd-spa-angular/us/en`。

8. 儲存變更後，您應將填入的變 `Header` 更視為範本的一部分：

   ![填入的標題元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接著，在AEM中建立其他頁面，做為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型階層結構。

1. 導覽至 **Sites** Console: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). 選擇 **WKND SPA Angular首頁** ，然後按一下「 **[!UICONTROL 建立]** 」 **[!UICONTROL >]**「頁：

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

2. 在「模 **[!UICONTROL 板]** 」下 **[!UICONTROL 選擇「SPA頁面」]**。 在「 **[!UICONTROL 屬性]** 」 **下，輸入「Page 1」作為Title** , **[!UICONTROL 並輸]****** 入「page-1」作為名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一 **[!UICONTROL 下「建立]** 」，然後在對話方塊快顯視窗中，按一下「 **[!UICONTROL 開啟]** 」以在AEM SPA編輯器中開啟頁面。

3. 將新的 **[!UICONTROL Text]** 元件新增至主 **[!UICONTROL 版面容器]**。 編輯元件並輸入文本： **使用** RTE和 **H1** 元素的&quot;Page 1&quot;（您必須進入全螢幕模式才能變更段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

4. 返回AEM Sites主控台並重複上述步驟，建立名為「Page 2」的第二頁， **作為****Page 1的同級頁面**。 將內容新 **增至第2** 頁，以便輕鬆識別。
5. 最後，請建立第三 **頁「第3頁** 」，但 **以第** 2 **頁的子頁**。 完成網站階層後，網站階層應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新標籤中，開啟AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 首次載入SPA時，會要求此JSON內容。 外部結構如下所示：

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   在下 `:children` 方，您應該會看到每個已建立頁面的項目。 所有頁面的內容都在此初始JSON請求中。 一旦實施導航路由，SPA的後續視圖將被快速載入，因為內容已經可在客戶端使用。

   在初始JSON請求中載 **入SPA的ALL** ，並不明智，因為這會減緩初始頁面載入的速度。 接下來，讓我們來瞭解如何收集頁面的階層深度。

7. 導覽至 **SPA根模板** ，網址為： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   按一下「 **[!UICONTROL 頁面屬性」功能表]** > **[!UICONTROL 頁面原則]**:

   ![開啟SPA根目錄的頁面原則](assets/navigation-routing/open-page-policy.png)

8. **SPA根範本** ，有額外的「階層 **** 結構」索引標籤來控制所收集的JSON內容。 「結 **[!UICONTROL 構深度]** 」可決定網站階層中根目錄下收集子頁面的深 **度**。 您也可以使用「結 **[!UICONTROL 構模式」欄位]** ，根據規則運算式篩選掉其他頁面。

   將「結 **[!UICONTROL 構深度]** 」更 **新為「2」**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一 **[!UICONTROL 下「完成]** 」以儲存對原則所做的變更。

9. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   請注意， **Page 3** path已移除： `/content/wknd-spa-angular/us/en/home/page-2/page-3` 從初始JSON模型。

   稍後，我們將觀察AEM SPA Editor SDK如何動態載入其他內容。

## 實作導覽

接著，使用新功能來實作導覽功能表 `NavigationComponent`。 我們可以直接在中加入程式碼， `header.component.html` 但更好的做法是避免使用大型元件。 相反，實作可 `NavigationComponent` 能會在稍後重新使用。

1. 檢視AEM元件公開的JSON，網址 `Header` 為 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   AEM頁面的階層性質是以JSON為模型，可用來填入導覽功能表。 請記得， `Header` 元件繼承了 [Navigation Core Component的所有功能，透過JSON公開的內容將自動映射至Angular注](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)`@Input` 釋。

2. 開啟新的終端窗口並導航到SPA `ui.frontend` 項目的資料夾。 使用Angular CLI `NavigationComponent` 工具建立新工具：

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 然後，在新建立的 `NavigationLink` 目錄中使用Angular CLI建立一個 `components/navigation` 類：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回您選擇的IDE，並在中開啟 `navigation-link.ts` 檔案 `/src/app/components/navigation/navigation-link.ts`。

   ![開啟navigation-link.ts檔案](assets/navigation-routing/ide-navigation-link-file.png)

5. 填 `navigation-link.ts` 入下列：

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   這是一個簡單的類別，可代表個別導覽連結。 在類別建構函式中，我 `data` 們預期是從AEM傳入的JSON物件。 此類將用於和中，以 `NavigationComponent` 便輕 `HeaderComponent` 松填入導航結構。

   未執行任何資料轉換，此類別的建立主要是為了強烈輸入JSON模型。 請注意， `this.children` 已鍵入 `NavigationLink[]` 為，且建構函式會遞歸地 `NavigationLink` 為陣列中的每個項目建立新物 `children` 件。 請回想一下的JSON模型是 `Header` 階層式。

6. 開啟檔案 `navigation-link.spec.ts`。 這是類的測試 `NavigationLink` 檔案。 使用下列方式更新：

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   請注意， `const data` 遵循先前針對單一連結所檢查的相同JSON模型。 這遠非強穩的單元測試，但是測試的建構子應該足夠 `NavigationLink`。

7. 開啟檔案 `navigation.component.ts`。 使用下列方式更新：

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` 預計 `object[]` AEM的 `items` JSON模型命名為。 此類會顯示一個返回 `get navigationLinks()` 對象陣列的單一 `NavigationLink` 方法。

8. 開啟檔案 `navigation.component.html` 並使用下列功能更新：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   這會產生初始 `<ul>` 值，並從 `get navigationLinks()` 呼叫方法 `navigation.component.ts`。 用 `<ng-container>` 來呼叫名為的範本，並將其 `recursiveListTmpl` 傳遞為 `navigationLinks` 名為的變數 `links`。

   新增下 `recursiveListTmpl` 一個：

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   這裡會實作導覽連結的其餘演算。 請注意，此變 `link` 數為類型， `NavigationLink` 且該類別所建立的所有方法／屬性皆可使用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) is used而非normal `href` attribute. 這可讓我們連結至應用程式中的特定路由，而不需重新整理整頁。

   如果當前具有非空陣列，則通過創 `<ul>` 建另一個 `link` 來實現導航的遞歸部 `children` 分。

9. 更新 `navigation.component.spec.ts` 以新增對以下項目的支 `RouterTestingModule`援：

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   由於組 `RouterTestingModule` 件使用，因此需要添加 `[routerLink]`。

10. 更新 `navigation.component.scss` 以新增一些基本樣式至 `NavigationComponent`:

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## 更新標題元件

現在已實 `NavigationComponent` 施，必須更新 `HeaderComponent` 以參考它。

1. 開啟終端並導覽至SPA `ui.frontend` 專案中的資料夾。 啟動 **webpack dev server**:

   ```shell
   $ npm start
   ```

2. 開啟瀏覽器標籤並導覽至 [http://localhost:4200/](http://localhost:4200/)。

   應 **將webpack dev server** (webpack dev server`ui.frontend/proxy.conf.json`)設定為從AEM()的本機例項代理JSON模型。 這可讓我們直接針對教學課程中先前在AEM中建立的內容編碼。

   ![菜單切換工作](./assets/navigation-routing/nav-toggle-static.gif)

   目前 `HeaderComponent` 的功能表切換功能已經實作。 接著，新增導覽元件。

3. 返回您選擇的IDE，並在中開啟文 `header.component.ts` 件 `ui.frontend/src/app/components/header/header.component.ts`。
4. 更新 `setHomePage()` 方法以移除硬式編碼字串，並使用AEM元件傳入的動態prop:

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   會根據從AEM `NavigationLink` 傳入的導 `items[0]`覽JSON模型根，建立新的例項。 `this.route.snapshot.data.path` 返回當前「角度」(Angular)路由的路徑。 此值用於確定當前路由是否為 **首頁**。 `this.homePageUrl` 用於填入標誌上的錨點 **連結**。

5. 開啟 `header.component.html` 導覽的靜態預留位置，並取代為新建立的參考 `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 屬性會 `@Input() items` 從 `HeaderComponent` 傳遞 `NavigationComponent` 至它要建立導覽的位置。

6. 打 `header.component.spec.ts` 開並添加聲明 `NavigationComponent`:

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   由於現在 `NavigationComponent` 已將它用作測試 `HeaderComponent` 台的一部分，因此需要聲明它。

7. 將更改保存到任何開啟的檔案並返回 **webpack開發伺服器**: [http://localhost:4200/](http://localhost:4200/)

   ![完成的頁首導覽](assets/navigation-routing/completed-header.png)

   按一下功能表切換以開啟導覽，您應該會看到填入的導覽連結。 您應該可以導覽至SPA的不同檢視。

## 瞭解SPA路由

現在已實作導覽，請在AEM中檢查路由。

1. 在IDE中，開啟文 `app-routing.module.ts` 件 `ui.frontend/src/app`。

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   陣 `routes: Routes = [];` 列定義指向Angular元件映射的路由或導航路徑。

   `AemPageMatcher` 是自訂的Angular路由器 [UrlMatcher](https://angular.io/api/router/UrlMatcher)，它會比對AEM中屬於此Angular應用程式的「外觀」頁面。

   `PageComponent` 是代表AEM中頁面的Angular Component，且相符的路由將會呼叫。 將 `PageComponent` 進一步檢查。

   `AemPageDataResolver`，由AEM SPA Editor JS SDK提供的是自訂的 [Angular Router Resolver](https://angular.io/api/router/Resolve) ，用來將路由URL（AEM中的路徑，包括the.html擴充功能）轉換為AEM中的資源路徑，而AEM中的頁面路徑則不是擴充功能。

   例如，將 `AemPageDataResolver` 路由的URL轉 `content/wknd-spa-angular/us/en/home.html` 換為路徑 `/content/wknd-spa-angular/us/en/home`。 這可用來根據JSON模型API中的路徑解析頁面內容。

   `AemPageRouteReuseStrategy`，由AEM SPA Editor JS SDK提供的自訂 [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) ，可防止跨路由 `PageComponent` 重複使用。 否則，頁面&quot;A&quot;的內容在導覽至頁面&quot;B&quot;時可能會顯示。

2. 在中開啟 `page.component.ts` 檔案 `ui.frontend/src/app/components/page/`。

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   必 `PageComponent` 須處理從AEM擷取的JSON，並用作Angular元件來轉換路由。

   `ActivatedRoute`，由Angular Router模組提供，包含狀態，指出應將哪個AEM Page的JSON內容載入此Angular Page元件例項。

   `ModelManagerService`，則會根據路由取得JSON資料，並將資料對應至類別變 `path`數 `items`、 `itemsOrder`。 然後，這些項目會傳遞至 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 開啟檔案位 `page.component.html` 置 `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包含 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)。 變數 `path`、 `items`和 `itemsOrder` 會傳遞至 `AEMPageComponent`。 然 `AemPageComponent`後，透過SPA編輯器JavaScript SDK提供的Angular元件將會重複此資料，並根據 [Map Components教學課程中的JSON資料動態執行個體化Angular元件](./map-components.md)。

   它 `PageComponent` 其實只是代理程式碼， `AEMPageComponent` 而它負責 `AEMPageComponent` 大部分的繁重工作，以正確將JSON模型對應至Angular元件。

## 在AEM中檢查SPA路由

1. 開啟終端並停止 **webpack dev server** （如果啟動）。 導覽至專案的根目錄，並使用您的Maven技巧將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Angular專案啟用了一些非常嚴格的行距規則。 如果Maven組建失敗，請檢查錯誤並查找列出的 **檔案中找到的Lint錯誤。**. 修正Linter發現的任何問題，並重新執行Maven命令。

2. 導覽至AEM中的SPA首頁： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) ，並開啟您瀏覽器的開發人員工具。 以下螢幕擷取自Google Chrome瀏覽器。

   重新整理頁面，您應該會看到XHR要求， `/content/wknd-spa-angular/us/en.model.json`即SPA根目錄。 請注意，根據本教學課程前面所做的SPA根範本的階層深度設定，僅包含三個子頁面。 這不包括第 **3頁**。

   ![初始JSON請求- SPA根目錄](assets/navigation-routing/initial-json-request.png)

3. 開啟開發人員工具後，導覽至 **第3頁**:

   ![第3頁導覽](assets/navigation-routing/page-three-navigation.png)

   請注意，新的XHR要求是： `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理員瞭解 **Page 3** JSON內容不可用，並自動觸發其他XHR請求。

4. 使用各種導覽連結繼續導覽SPA。 請注意，未提出其他XHR要求，且未進行完整頁面重新整理。 這可讓使用者快速取得SPA，並減少不必要的回AEM要求。

   ![實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

5. 直接導覽至： [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). 請注意，瀏覽器的「上一步」按鈕仍能繼續運作。

## 恭喜！ {#congratulations}

恭喜您，您瞭解如何透過SPA編輯器SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航已使用Angular路由實現，並添加到元件 `Header` 中。

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ，或切換至分支，在本機檢出程式碼 `Angular/navigation-routing-solution`。

### 後續步驟 {#next-steps}

[建立自訂元件](custom-component.md) -瞭解如何建立要搭配AEM SPA編輯器使用的自訂元件。 瞭解如何開發作者對話方塊和Sling Models以擴充JSON模型以填入自訂元件。
