---
title: 添加導航和路由 |開始使用AEM SPA編輯器和Angular
description: 了解如何使用AEM頁面和SPA Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的Header元件。
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2713'
ht-degree: 0%

---

# 添加導航和路由 {#navigation-routing}

了解如何使用AEM頁面和SPA Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的Header元件。

## 目標

1. 了解使用SPA編輯器時可用的SPA模型路由選項。
2. 了解如何使用[Angular路由](https://angular.io/guide/router)在SPA的不同檢視之間導覽。
3. 實作由AEM頁面階層驅動的動態導覽。

## 您將建置的

本章將導航菜單添加到現有`Header`元件。 導覽功能表由AEM頁面階層驅動，並使用[導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝傳統[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)的已完成包。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像將在WKND SPA上重新使用。 可使用[AEM套件管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`Angular/navigation-routing-solution`在本機檢出程式碼。

## Inspect HeaderComponent更新 {#inspect-header}

在前幾章中，`HeaderComponent`元件是通過`app.component.html`包含的純Angular元件。 在本章中，將從應用程式中移除`HeaderComponent`元件，並透過[範本編輯器](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)新增元件。 這可讓使用者從AEM內設定`HeaderComponent`的導覽功能表。

>[!NOTE]
>
> 已對程式碼基底進行數次CSS和JavaScript更新，以啟動本章節。 若要著重於核心概念，不討論程式碼變更的&#x200B;**all**。 您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start)查看完整更改。[

1. 在您選擇的IDE中，開啟本章的SPA入門項目。
2. 在`ui.frontend`模組下方，檢查檔案`header.component.ts` ，網址為：`ui.frontend/src/app/components/header/header.component.ts`。

   已進行多項更新，包括添加`HeaderEditConfig`和`MapTo`以使元件映射到AEM元件`wknd-spa-angular/components/header`。

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

   記下`items`的`@Input()`注釋。 `items` 將包含從AEM傳入的導覽物件陣列。

3. 在`ui.apps`模組中，檢查AEM `Header`元件的元件定義：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header`元件將透過`sling:resourceSuperType`屬性繼承[導航核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)的所有功能。

## 將HeaderComponent新增至SPA範本 {#add-header-template}

1. 開啟瀏覽器並登入AEM, [http://localhost:4502/](http://localhost:4502/)。 應已部署起始代碼庫。
2. 導覽至&#x200B;**[!UICONTROL SPA頁面範本]**:[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)。
3. 選擇最外層&#x200B;**[!UICONTROL 根佈局容器]**，然後按一下其&#x200B;**[!UICONTROL 策略]**&#x200B;表徵圖。 請留意&#x200B;**不**&#x200B;以選取&#x200B;**[!UICONTROL 版面容器]**&#x200B;未鎖定以供編寫。

   ![選擇根佈局容器策略表徵圖](assets/navigation-routing/root-layout-container-policy.png)

4. 複製當前策略並建立名為&#x200B;**[!UICONTROL SPA Structure]**&#x200B;的新策略：

   ![SPA結構原則](assets/map-components/spa-policy-update.png)

   在「**[!UICONTROL 允許的元件]**」>「**[!UICONTROL 一般]**」下，選擇「**[!UICONTROL 佈局容器]**」元件。

   在&#x200B;**[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPAANGULAR- STRUCTURE]**&#x200B;下，選擇&#x200B;**[!UICONTROL 標題]**&#x200B;元件：

   ![選擇標題元件](assets/map-components/select-header-component.png)

   在「**[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPAANGULAR — 內容]**」下，選擇&#x200B;**[!UICONTROL Image]**&#x200B;和&#x200B;**[!UICONTROL Text]**&#x200B;元件。 您應選取總計4個元件。

   按一下&#x200B;**[!UICONTROL Done]**&#x200B;以儲存變更。

5. **** 重新整理頁面。在取消鎖定的&#x200B;**[!UICONTROL 配置容器]**&#x200B;上方新增&#x200B;**[!UICONTROL 標題]**&#x200B;元件：

   ![將標題元件添加到模板](./assets/navigation-routing/add-header-component.gif)

6. 選擇&#x200B;**[!UICONTROL Header]**&#x200B;元件，然後按一下其&#x200B;**Policy**&#x200B;表徵圖以編輯策略。

   ![按一下標題策略](assets/navigation-routing/header-policy-icon.png)

7. 使用&#x200B;**[!UICONTROL 策略標題]** **&quot;WKND SPA標題&quot;**&#x200B;建立新策略。

   在&#x200B;**[!UICONTROL Properties]**&#x200B;下：

   * 將&#x200B;**[!UICONTROL 導航根]**&#x200B;設定為`/content/wknd-spa-angular/us/en`。
   * 將&#x200B;**[!UICONTROL 排除根級別]**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**[!UICONTROL 收集所有子頁]**。
   * 將&#x200B;**[!UICONTROL 導航結構深度]**&#x200B;設定為&#x200B;**3**。

   ![配置標頭策略](assets/navigation-routing/header-policy.png)

   這將收集`/content/wknd-spa-angular/us/en`下方深處的導覽2層。

8. 儲存變更後，範本中應該會顯示已填入的`Header`:

   ![填充的標題元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，作為SPA中的不同檢視。 我們也會檢查AEM提供的JSON模型的階層結構。

1. 導覽至&#x200B;**Sites**&#x200B;主控台：[http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home)。 選擇&#x200B;**WKND SPAAngular首頁**，然後按一下&#x200B;**[!UICONTROL 建立]** > **[!UICONTROL 頁面]**:

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

2. 在&#x200B;**[!UICONTROL Template]**&#x200B;下，選擇&#x200B;**[!UICONTROL SPA Page]**。 在&#x200B;**[!UICONTROL Properties]**&#x200B;下，以&#x200B;**[!UICONTROL Title]**&#x200B;和&#x200B;**&quot;page-1&quot;**&#x200B;為名稱輸入&#x200B;**&quot;Page 1&quot;**。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下&#x200B;**[!UICONTROL 建立]**，然後在對話方塊快顯視窗中，按一下&#x200B;**[!UICONTROL 開啟]**&#x200B;以在AEM SPA編輯器中開啟頁面。

3. 將新的&#x200B;**[!UICONTROL Text]**&#x200B;元件添加到主&#x200B;**[!UICONTROL 佈局容器]**&#x200B;中。 編輯元件並輸入文本：**&quot;Page 1&quot;**&#x200B;使用RTE和&#x200B;**H1**&#x200B;元素（您必須進入全螢幕模式才能變更段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

4. 返回AEM Sites主控台並重複上述步驟，建立名為&#x200B;**&quot;Page 2&quot;**&#x200B;的第二個頁面，作為&#x200B;**Page 1**&#x200B;的同層級。 將內容新增至&#x200B;**頁面2**，以便輕鬆識別。
5. 最後，建立第三個頁面&#x200B;**&quot;Page 3&quot;**，但作為&#x200B;**Page 2**&#x200B;的&#x200B;**子**。 完成網站階層後，應如下所示：

   ![網站階層範例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新索引標籤中，開啟AEM提供的JSON模型API:[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 首次載入SPA時，會要求此JSON內容。 外部結構如下所示：

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

   在`:children`下，應該會看到每個已建立頁面的項目。 所有頁面的內容都位於此初始JSON要求中。 一旦實作導覽路由，後續的SPA檢視就會快速載入，因為內容已可在用戶端使用。

   在初始JSON要求中載入SPA內容的&#x200B;**ALL**&#x200B;並不明智，因為這會減緩初始頁面載入速度。 接下來，讓我們查看頁面階層深度的收集方式。

7. 導覽至&#x200B;**SPA根**&#x200B;範本：[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html)。

   按一下&#x200B;**[!UICONTROL 頁面屬性功能表]** > **[!UICONTROL 頁面原則]**:

   ![開啟SPA根的頁面原則](assets/navigation-routing/open-page-policy.png)

8. **SPA根**&#x200B;範本有額外的&#x200B;**[!UICONTROL 階層結構]**&#x200B;標籤，可控制收集的JSON內容。 **[!UICONTROL 「結構深度」]**&#x200B;決定了網站階層中的深度，以收集&#x200B;**根**&#x200B;下方的子頁面。 您也可以使用&#x200B;**[!UICONTROL 結構模式]**&#x200B;欄位，根據規則運算式篩除其他頁面。

   將&#x200B;**[!UICONTROL 結構深度]**&#x200B;更新為&#x200B;**&quot;2&quot;**:

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下&#x200B;**[!UICONTROL Done]**&#x200B;以保存對策略的更改。

9. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

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

   注意&#x200B;**Page 3**&#x200B;路徑已移除：`/content/wknd-spa-angular/us/en/home/page-2/page-3`。

   稍後，我們將觀察AEM SPA Editor SDK如何動態載入其他內容。

## 實作導覽

接下來，使用新的`NavigationComponent`實作導覽功能表。 我們可以直接在`header.component.html`中新增程式碼，但最好避免使用大型元件。 請改為實作`NavigationComponent`，之後可能會重新使用。

1. 檢閱AEM `Header`元件公開的JSON，網址為[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   AEM頁面的階層性質是以JSON為模型，可用來填入導覽功能表。 回想一下，`Header`元件繼承[導航核心元件](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)的所有功能，通過JSON公開的內容將自動映射到Angular`@Input`批注。

2. 開啟新的終端機視窗，並導覽至SPA專案的`ui.frontend`資料夾。 使用AngularCLI工具建立新的`NavigationComponent`:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 接下來，使用新建立的`components/navigation`目錄中的AngularCLI建立名為`NavigationLink`的類：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回您選擇的IDE，並在`/src/app/components/navigation/navigation-link.ts`的`navigation-link.ts`開啟檔案。

   ![開啟navigation-link.ts檔案](assets/navigation-routing/ide-navigation-link-file.png)

5. 將以下內容填入`navigation-link.ts`:

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

   這是代表個別導覽連結的簡單類別。 在類建構函式中，我們希望`data`是從AEM傳入的JSON物件。 此類將用於`NavigationComponent`和`HeaderComponent`中，以輕鬆填充導航結構。

   未執行任何資料轉換，此類別主要建立為強烈輸入JSON模型。 請注意，`this.children`鍵入為`NavigationLink[]`，建構子會為`children`陣列中的每個項目遞回建立新的`NavigationLink`物件。 回想一下，`Header`的JSON模型是分層的。

6. 開啟檔案`navigation-link.spec.ts`。 這是`NavigationLink`類的測試檔案。 請使用下列項目更新：

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

   請注意，`const data`遵循先前檢查過單一連結的相同JSON模型。 這遠非強健的單元測試，但是應該足以測試`NavigationLink`的建構子。

7. 開啟檔案`navigation.component.ts`。 請使用下列項目更新：

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

   `NavigationComponent` 預期 `object[]` 的 `items` 名稱為來自AEM的JSON模型。此類會公開一個返回`NavigationLink`對象陣列的`get navigationLinks()`方法。

8. 開啟檔案`navigation.component.html`，然後使用以下內容進行更新：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   這會產生初始`<ul>`，並從`navigation.component.ts`呼叫`get navigationLinks()`方法。 `<ng-container>`用於呼叫名為`recursiveListTmpl`的範本，並以名為`links`的變數傳遞`navigationLinks`。

   下面添加`recursiveListTmpl`:

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

   在此實作導覽連結的其餘轉譯。 請注意，變數`link`的類型為`NavigationLink`，且該類建立的所有方法/屬性都可用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) 會取代一般屬 `href` 性。這可讓我們連結至應用程式中的特定路由，而不需重新整理整頁。

   如果當前`link`具有非空`children`陣列，則通過建立另一個`<ul>`來實現導航的遞歸部分。

9. 更新`navigation.component.spec.ts`以添加對`RouterTestingModule`的支援：

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

   需要添加`RouterTestingModule`，因為元件使用`[routerLink]`。

10. 更新`navigation.component.scss`以將一些基本樣式添加到`NavigationComponent`中：

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

現在`NavigationComponent`已實作，必須更新`HeaderComponent`以參考它。

1. 開啟終端機，並導覽至SPA專案內的`ui.frontend`資料夾。 啟動&#x200B;**Webpack開發伺服器**:

   ```shell
   $ npm start
   ```

2. 開啟瀏覽器標籤並導覽至[http://localhost:4200/](http://localhost:4200/)。

   應將&#x200B;**webpack開發伺服器**&#x200B;設定為從AEM的本機例項(`ui.frontend/proxy.conf.json`)代理JSON模型。 這可讓我們針對教學課程先前在AEM中建立的內容直接編寫程式碼。

   ![功能表切換工作](./assets/navigation-routing/nav-toggle-static.gif)

   `HeaderComponent`目前已實作功能表切換功能。 接下來，新增導覽元件。

3. 返回您選擇的IDE，在`ui.frontend/src/app/components/header/header.component.ts`處開啟檔案`header.component.ts`。
4. 更新`setHomePage()`方法以移除硬式編碼字串，並使用AEM元件傳入的動態屬性：

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

   會根據從AEM傳入的導覽JSON模型根`items[0]`，建立`NavigationLink`的新例項。 `this.route.snapshot.data.path` 返回當前Angular路徑的路徑。此值用於確定當前路由是否為&#x200B;**首頁**。 `this.homePageUrl` 可用來填入標誌上的錨點 **連結**。

5. 開啟`header.component.html`，並以對新建立的`NavigationComponent`的引用替換導航的靜態佔位符：

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 屬性會 `@Input() items` 從傳 `HeaderComponent` 遞 `NavigationComponent` 至其將建置導覽的位置。

6. 開啟`header.component.spec.ts`並為`NavigationComponent`添加聲明：

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

   由於`NavigationComponent`現在作為`HeaderComponent`的一部分使用，因此需要將其聲明為測試台的一部分。

7. 保存對任何開啟的檔案的更改並返回到&#x200B;**webpack開發伺服器**:[http://localhost:4200/](http://localhost:4200/)

   ![已完成的頁首導航](assets/navigation-routing/completed-header.png)

   按一下功能表切換，開啟導覽，您應該會看到填入的導覽連結。 您應該可以導覽至SPA的不同檢視。

## 了解SPA路由

現在導覽已實作完畢，請檢查AEM中的路由。

1. 在IDE中，在`ui.frontend/src/app`處開啟檔案`app-routing.module.ts`。

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

   `routes: Routes = [];`陣列定義到Angular元件映射的路由或導航路徑。

   `AemPageMatcher` 是自訂Angular路 [由器UrlMatcher](https://angular.io/api/router/UrlMatcher)，可比對AEM中屬於此Angular應用程式一部分之「看起來」的任何項目。

   `PageComponent` 是AEM中代表「頁面」的Angular元件，相符的路由將會叫用。將進一步檢查`PageComponent`。

   `AemPageDataResolver`，由AEM SPA Editor JS SDK提供，是自訂 [Angular路由](https://angular.io/api/router/Resolve) 器解析器，用於將路由URL(AEM中的路徑，包括.html擴充功能)轉換為AEM中的資源路徑（頁面路徑減去擴充功能）。

   例如， `AemPageDataResolver`會將路由的URL `content/wknd-spa-angular/us/en/home.html`轉換為路徑`/content/wknd-spa-angular/us/en/home`。 這可用來根據JSON模型API中的路徑解析頁面內容。

   `AemPageRouteReuseStrategy`，由AEM SPA Editor JS SDK提供，是自訂的RouteReuseStrategy，可 [](https://angular.io/api/router/RouteReuseStrategy) 防止跨路由 `PageComponent` 重複使用。否則，導覽至頁面「B」時，頁面「A」的內容可能會顯示。

2. 在`ui.frontend/src/app/components/page/`開啟檔案`page.component.ts`。

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

   需要`PageComponent`來處理從AEM擷取的JSON，並作為轉譯路由的Angular元件。

   `ActivatedRoute`，此元件由Angular路由器模組提供，其中包含的狀態指出應將哪個AEM頁面的JSON內容載入到此Angular頁面元件例項中。

   `ModelManagerService`，會根據路由取得JSON資料，並將資料對應至類別變 `path`數 `items`、  `itemsOrder`。然後，這些值將傳遞至[AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 在`ui.frontend/src/app/components/page/`開啟檔案`page.component.html`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包含 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)。變數`path`、`items`和`itemsOrder`會傳遞至`AEMPageComponent`。 接著，透過SPA編輯器JavaScript SDK提供的`AemPageComponent`將會反覆查看此資料，並根據[地圖元件教學課程](./map-components.md)中顯示的JSON資料，以動態方式實例化Angular元件。

   `PageComponent`其實只是`AEMPageComponent`的代理，也是執行大部分繁重提升作業的`AEMPageComponent`，將JSON模型正確對應至Angular元件。

## Inspect AEM中的SPA路由

1. 開啟終端，並在啟動後停止&#x200B;**Webpack開發伺服器**。 導覽至專案的根目錄，並使用您的Maven技能將專案部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular專案已啟用一些非常嚴格的連結規則。 如果Maven組建失敗，請檢查錯誤並尋找列出檔案中找到的&#x200B;**Lint錯誤。**. 修正Linter發現的任何問題，並重新執行Maven命令。

2. 導覽至AEM中的SPA首頁：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html)並開啟瀏覽器的開發人員工具。 以下螢幕擷取畫面是從Google Chrome瀏覽器擷取。

   重新整理頁面，您應該會看到`/content/wknd-spa-angular/us/en.model.json`(即SPA根)的XHR請求。 請注意，根據先前在教學課程中對SPA根範本進行的階層深度設定，僅包含三個子頁面。 這不包括&#x200B;**第3**&#x200B;頁。

   ![初始JSON要求 — SPA根](assets/navigation-routing/initial-json-request.png)

3. 開啟開發人員工具後，導覽至&#x200B;**第3**&#x200B;頁：

   ![第3頁導航](assets/navigation-routing/page-three-navigation.png)

   請注意，系統已對下列項目提出新的XHR請求：`/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR請求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager了解JSON內容不可用，並自動觸發其他XHR要求。****

4. 使用各種導覽連結繼續導覽SPA。 請注意，系統未提出其他XHR請求，且未發生完整頁面重新整理。 這可讓使用者快速取得SPA，並減少傳回AEM的不必要請求。

   ![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

5. 直接導覽至：[http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html)。 請注意，瀏覽器的返回按鈕可繼續運作。

## 恭喜！ {#congratulations}

恭喜您，您已了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航已使用Angular路由實現，並添加到`Header`元件中。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)上檢視完成的程式碼，或切換至分支`Angular/navigation-routing-solution`在本機檢出程式碼。

### 後續步驟 {#next-steps}

[建立自訂元件](custom-component.md)  — 了解如何建立要與AEM SPA編輯器搭配使用的自訂元件。了解如何開發製作對話方塊和Sling模型，以擴充JSON模型以填入自訂元件。
