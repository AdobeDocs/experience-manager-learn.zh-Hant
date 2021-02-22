---
title: 將SPA元件對應至AEM元件 | AEM SPA編輯器與Angular快速入門
description: 瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，類似於傳統的AEM編寫。
sub-product: sites
feature: SPA編輯器
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 28b5522e094a41d81116acb923dc0390478e2308
workflow-type: tm+mt
source-wordcount: '2389'
ht-degree: 0%

---


# 將SPA元件對應至AEM元件{#map-components}

瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，類似於傳統的AEM編寫。

本章深入探討AEM JSON模型API，以及AEM元件公開的JSON內容如何自動插入Angular元件做為prop。

## 目標

1. 瞭解如何將AEM元件對應至SPA元件。
2. 瞭解&#x200B;**Container**&#x200B;元件與&#x200B;**Content**&#x200B;元件之間的差異。
3. 建立新的Angular元件，以對應至現有的AEM元件。

## 您將建立的

本章將檢查提供的`Text` SPA元件如何映射到AEM `Text`元件。 將會建立新的`Image` SPA元件，以便用於SPA並在AEM中編寫。 **版面容器**&#x200B;和&#x200B;**範本編輯器**&#x200B;原則的立即可用功能也將用來建立外觀稍有變化的檢視。

![章節範例最終編寫](./assets/map-components/final-page.png)

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. 使用Maven將程式碼庫部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution)上檢視完成的程式碼，或切換至分支`Angular/map-components-solution`，在本機檢出程式碼。

## 映射方法

基本概念是將SPA元件對應至AEM元件。 AEM元件、執行伺服器端、匯出內容做為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行用戶端。 會建立SPA元件與AEM元件之間的1:1對應。

![將AEM元件對應至角度元件的高階概述](./assets/map-components/high-level-approach.png)

*將AEM元件對應至角度元件的高階概述*

## 檢查文本元件

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)提供映射至AEM [Text元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)的`Text`元件。 這是&#x200B;**content**&#x200B;元件的範例，其中它會從AEM轉譯&#x200B;*content*。

讓我們看看元件的運作方式。

### 檢查JSON模型

1. 在跳至SPA程式碼之前，請務必瞭解AEM提供的JSON模型。 導覽至[核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)並檢視Text元件的頁面。 核心元件庫提供所有AEM核心元件的範例。
2. 選擇&#x200B;**JSON**&#x200B;標籤，以取得其中一個範例：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性：`text`、`richText`和`:type`。

   `:type` 是一個保留屬性，會列 `sling:resourceType` 出AEM元件的（或路徑）。`:type`的值是用來將AEM元件對應至SPA元件的值。

   `text` 以 `richText` 及SPA元件的其他屬性。

### 檢查Text元件

1. 開啟新的終端機，並導覽至專案內的`ui.frontend`資料夾。 然後運行`npm install`和`npm start`以啟動&#x200B;**webpack dev server**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   `ui.frontend`模組目前已設定為使用[模擬JSON模型](./integrate-spa.md#mock-json)。

2. 您應會看到新的瀏覽器視窗開啟至[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![具有模擬內容的Webpack開發伺服器](assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 展開`ui.frontend`模組並開啟`ui.frontend/src/app/components/text/text.component.ts`下的&#x200B;**text.component.ts**&#x200B;檔案：

   ![Text.js Angular元件原始碼](assets/map-components/vscode-ide-text-js.png)

4. 第一個要檢查的區域是位於~35行的`class TextComponent`:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) decorator可用來宣告透過已映射JSON物件（先前已檢閱）設定值的欄位。

   `@HostBinding('innerHtml') get content()` 是一種方法，它會從的值中公開編寫的文字內容 `this.text`如果內容是富文本（由`this.richText`標誌確定），則會繞過Angular的內置安全性。 Angular的[DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer)用於「拖曳」原始HTML並防止跨網站指令碼弱點。 該方法使用[@HostBinding](https://angular.io/api/core/HostBinding) decorator綁定到`innerHtml`屬性。

5. 接下來檢查~line 24處的`TextEditConfig`:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上述程式碼負責決定何時在AEM作者環境中呈現預留位置。 如果`isEmpty`方法返回&#x200B;**true**，則會呈現預留位置。

6. 最後，請看~53行的`MapTo`呼叫：

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **AEM SPA Editor JS SDK(** `@adobe/cq-angular-editable-components`)提供的MapTois。路徑`wknd-spa-angular/components/text`代表AEM元件的`sling:resourceType`。 此路徑與先前觀察到的JSON模型公開的`:type`相符。 **MapTo** 剖析JSON模型回應，並將正確值傳 `@Input()` 遞至SPA元件的變數。

   您可以在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`找到AEM `Text`元件定義。

7. 在`ui.frontend/src/mocks/json/en.model.json`修改&#x200B;**en.model.json**&#x200B;檔案，以進行實驗。

   在~line 62更新第一個`Text`值，以使用&#x200B;**`H1`**&#x200B;和&#x200B;**`u`**&#x200B;標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   返回瀏覽器，查看&#x200B;**webpack dev server**&#x200B;所提供的效果：

   ![更新的文字模型](assets/map-components/updated-text-model.png)

   嘗試在&#x200B;**true** / **false**&#x200B;之間切換`richText`屬性，以檢視演算邏輯的實際運作。

8. 在`ui.frontend/src/app/components/text/text.component.html`檢查&#x200B;**text.component.html**。

   此檔案為空，因為元件的全部內容將由`innerHTML`屬性設定。

9. 在`ui.frontend/src/app/app.module.ts`檢查&#x200B;**app.module.ts**。

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   **TextComponent**&#x200B;未明確包含，而是透過AEM SPA編輯器JS SDK提供的&#x200B;**AEMResponsiveGridComponent**&#x200B;動態加入。 因此，必須列在&#x200B;**app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components)陣列中。

## 建立影像元件

接著，建立映射至AEM [Image元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)的`Image`Angular元件。 `Image`元件是&#x200B;**content**&#x200B;元件的另一個範例。

### 檢查JSON

在跳至SPA程式碼之前，請先檢查AEM提供的JSON模型。

1. 導覽至核心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)中的[影像範例。

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   `src`、`alt`和`title`的屬性將用於填充SPA `Image`元件。

   >[!NOTE]
   >
   > 還有其他顯示的影像屬性(`lazyEnabled`、`widths`)可讓開發人員建立可調式和延遲載入元件。 本教學課程中建立的元件將很簡單，而且&#x200B;**not**&#x200B;會使用這些進階屬性。

2. 返回IDE並在`ui.frontend/src/mocks/json/en.model.json`處開啟`en.model.json`。 由於這是專案的新元件，因此我們需要「模擬」影像JSON。

   在~line 70中，為`image`模型新增JSON項目（別忘記第二個`text_386303036`後面的尾隨逗號`,`），並更新`:itemsOrder`陣列。

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   專案包含`/mock-content/adobestock-140634652.jpeg`的範例影像，將與&#x200B;**webpack dev server**&#x200B;搭配使用。

   您可以在這裡檢視完整的[en.model.json](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json)。

3. 新增要由元件顯示的圖庫像片。

   在`ui.frontend/src/mocks`下方建立名為&#x200B;**images**&#x200B;的新資料夾。 下載[adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg)，並將它放置在新建立的&#x200B;**images**&#x200B;檔案夾中。 如有需要，請自由使用您自己的影像。

### 實作影像元件

1. 如果啟動，請停止&#x200B;**webpack dev server**。
2. 通過從`ui.frontend`資料夾內運行Angular CLI `ng generate component`命令來建立新的映像元件：

   ```shell
   $ ng generate component components/image
   ```

3. 在IDE中，在`ui.frontend/src/app/components/image/image.component.ts`處開啟&#x200B;**image.component.ts** ，然後按如下方式更新：

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` 是設定，以決定是否在AEM中根據屬性是否已填入來呈現作者 `src` 預留位置。

   `@Input()` of  `src`、 `alt`和 `title` 是從JSON API映射的屬性。

   `hasImage()` 是一種方法，可決定是否應呈現影像。

   `MapTo` 將SPA元件對應至位於的AEM元件 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`。

4. 開啟&#x200B;**image.component.html**，並依下列方式更新：

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   如果`hasImage`傳回&#x200B;**true**，則會轉換`<img>`元素。

5. 開啟&#x200B;**image.component.scss**，並依下列方式更新：

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > `:host-context`規則是&#x200B;**critical**，讓AEM SPA編輯器預留位置正確運作。 所有要在AEM頁面編輯器中編寫的SPA元件至少需要此規則。

6. 開啟`app.module.ts` ，將`ImageComponent`添加到`entryComponents`陣列：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   與`TextComponent`一樣，`ImageComponent`是動態載入的，且必須包含在`entryComponents`陣列中。

7. 啟動&#x200B;**webpack dev server**&#x200B;以查看`ImageComponent`演算。

   ```shell
   $ npm run start:mock
   ```

   ![已新增影像至模擬](assets/map-components/image-added-mock.png)

   *已添加到SPA的映像*

   >[!NOTE]
   >
   > **獎金挑戰**:實作新方法，將值顯示為影 `title` 像下方的標題。

## AEM中的更新政策

`ImageComponent`元件僅在&#x200B;**webpack dev server**&#x200B;中顯示。 接著，將更新的SPA部署至AEM並更新範本原則。

1. 停止&#x200B;**webpack dev server**&#x200B;和專案的&#x200B;**root**，使用您的Maven技巧將變更部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從「AEM開始」畫面導覽至「工具」(**[!UICONTROL Tools)]**>>「範本」(Templates)]**>>「WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**。**[!UICONTROL **[

   選擇並編輯&#x200B;**SPA頁**:

   ![編輯SPA頁面範本](assets/map-components/edit-spa-page-template.png)

3. 選擇&#x200B;**配置容器**，然後按一下其&#x200B;**policy**&#x200B;表徵圖以編輯策略：

   ![配置容器原則](./assets/map-components/layout-container-policy.png)

4. 在「**允許的元件** > **WKND SPA角度——內容**」下，檢查&#x200B;**Image**&#x200B;元件：

   ![已選取影像元件](assets/map-components/check-image-component.png)

   在「**預設元件** > **添加映射**」下，選擇&#x200B;**影像- WKND SPA角度——內容**&#x200B;元件：

   ![設定預設元件](assets/map-components/default-components.png)

   輸入&#x200B;**mime類型**&#x200B;的`image/*`。

   按一下&#x200B;**Done**&#x200B;保存策略更新。

5. 在&#x200B;**版面容器**&#x200B;中，按一下&#x200B;**Text**&#x200B;元件的&#x200B;**policy**&#x200B;圖示：

   ![文本元件策略表徵圖](./assets/map-components/edit-text-policy.png)

   建立名為&#x200B;**WKND SPA文本**&#x200B;的新策略。 在「**Plugins** > **Formatting** >」下，選中所有框以啟用其他格式設定選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在「**Plugins** > **段落樣式** >」下，選中該框以「啟用段落樣式&#x200B;**」:**

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下&#x200B;**Done**&#x200B;保存策略更新。

6. 導覽至&#x200B;**Homepage** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。

   您也可以編輯`Text`元件，並在&#x200B;**全螢幕**&#x200B;模式中新增其他段落樣式。

   ![全螢幕豐富式文字編輯](assets/map-components/full-screen-rte.png)

7. 您也可以從&#x200B;**Asset finder**&#x200B;拖放影像：

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過[AEM Assets](http://localhost:4502/assets.html/content/dam)新增您自己的影像，或安裝標準[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)的完成程式碼庫。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)包含許多可在WKND SPA上重新使用的影像。 可使用[AEM的Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## 檢查版面容器

AEM SPA編輯器SDK會自動提供對&#x200B;**版面容器**&#x200B;的支援。 **版面容器**&#x200B;是&#x200B;**容器**&#x200B;元件，如名稱所示。 容器元件是接受JSON結構的元件，其代表&#x200B;*other*&#x200B;元件並動態執行個體化它們。

讓我們進一步檢查「版面容器」。

1. 在IDE中，在`ui.frontend/src/app/components/responsive-grid`處開啟&#x200B;**responsive-grid.component.ts**:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent`是以AEM SPA Editor SDK的一部份實作，並透過`import-components`包含在專案中。

2. 在瀏覽器中瀏覽至[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON模型API —— 自適應格線](./assets/map-components/responsive-grid-modeljson.png)

   **版面容器**&#x200B;元件具有`wcm/foundation/components/responsivegrid`的`sling:resourceType`，並由SPA編輯器使用`:type`屬性加以識別，就像`Text`和`Image`元件一樣。

   SPA編輯器提供使用[配置模式](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新調整元件大小的相同功能。

3. 返回[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 新增其他&#x200B;**Image**&#x200B;元件，並嘗試使用&#x200B;**Layout**&#x200B;選項重新調整其大小：

   ![使用版面模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

4. 重新開啟JSON模型[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)，並觀察`columnClassNames`作為JSON的一部分：

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名`aem-GridColumn--default--4`表示元件應基於12列網格為4列寬。 如需[回應式格線的詳細資訊，請參閱此處](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

5. 返回到IDE，在`ui.apps`模組中，`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`中定義了一個客戶端庫。 開啟檔案`less/grid.less`。

   此檔案會決定&#x200B;**版面配置容器**&#x200B;使用的中斷點（`default`、`tablet`和`phone`）。 此檔案旨在根據專案規格自訂。 目前，中斷點設為`1200px`和`650px`。

6. 您應該能夠使用`Text`元件的回應功能和更新的富格文字原則來製作如下檢視：

   ![章節範例最終編寫](assets/map-components/final-page.png)

## 恭喜！{#congratulations}

恭喜您，您已學習如何將SPA元件對應至AEM元件，並實作新的`Image`元件。 您也可以探索&#x200B;**版面容器**&#x200B;的回應功能。

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution)上檢視完成的程式碼，或切換至分支`Angular/map-components-solution`，在本機檢出程式碼。

### 後續步驟{#next-steps}

[導覽和路由](navigation-routing.md) -瞭解如何透過SPA編輯器SDK對應至AEM頁面，以支援SPA中的多個檢視。動態導航是使用Angular Router實現的，並添加到現有的Header元件中。

## 附加——將配置保留到源控制{#bonus}

在許多情況下，尤其是在AEM專案開始時，將設定（例如範本和相關內容原則）保留至來源控制非常有用。 這可確保所有開發人員針對相同的內容和組態進行工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟度，管理模板的做法就可以交給一組特殊的超級用戶。

接下來的幾個步驟將使用Visual Studio代碼IDE和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)進行，但可能使用您已設定為從AEM的本機例項匯入&#x200B;**pull**&#x200B;或&#x200B;**內容的任何工具和任何IDE進行。**

1. 在Visual Studio代碼IDE中，請確定您已透過Marketplace擴充功能安裝&#x200B;**VSCode AEM Sync**:

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展開「項目瀏覽器」中的&#x200B;**ui.content**&#x200B;模組，並導航至`/conf/wknd-spa-angular/settings/wcm/templates`。

3. **Right+** 按一下 `templates` 資料夾，然 **後選取「從AEM Server匯入」**:

   ![VSCode匯入範本](assets/map-components/import-aem-servervscode.png)

4. 重複這些步驟以導入內容，但選擇位於`/conf/wknd-spa-angular/settings/wcm/policies`的&#x200B;**policys**&#x200B;資料夾。

5. 檢查位於`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`檔案。

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   `filter.xml`檔案負責標識將隨軟體包一起安裝的節點的路徑。 請注意，每個篩選器上的`mode="merge"`都表示不會修改現有內容，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此代碼部署&#x200B;**not**&#x200B;覆寫內容非常重要。 有關使用篩選器元素的詳細資訊，請參閱[FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html)。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以瞭解每個模組管理的不同節點。
