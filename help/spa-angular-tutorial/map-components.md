---
title: 將SPA元件對應至AEM元件 | AEM SPA編輯器與Angular快速入門
description: 瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，類似於傳統的AEM編寫。
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 0%

---


# 將SPA元件對應至AEM元件 {#map-components}

瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，類似於傳統的AEM編寫。

本章深入探討AEM JSON模型API，以及AEM元件公開的JSON內容如何自動插入Angular元件做為prop。

## 目標

1. 瞭解如何將AEM元件對應至SPA元件。
2. 瞭解Container元件與 **Content** 元件之 **間的差** 異。
3. 建立新的Angular元件，以對應至現有的AEM元件。

## 您將建立的

本章將檢查提供的 `Text` SPA元件如何對應至AEM元 `Text`件。 將會建 `Image` 立新的SPA元件，以便用於SPA並在AEM中編寫。 版面容器和範本編輯 **器(** Layout Container **** )原則的立即可用功能也可用來建立外觀更多樣的檢視。

![章節範例最終編寫](./assets/map-components/final-page.png)

## 必備條件

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

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

   如果使用 [AEM 6.x](overview.md#compatibility) ，請新增 `classic` 描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ，或切換至分支，在本機檢出程式碼 `Angular/map-components-solution`。

## 映射方法

基本概念是將SPA元件對應至AEM元件。 AEM元件、執行伺服器端、匯出內容做為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行用戶端。 會建立SPA元件與AEM元件之間的1:1對應。

![將AEM元件對應至角度元件的高階概述](./assets/map-components/high-level-approach.png)

*將AEM元件對應至角度元件的高階概述*

## 檢查文本元件

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)`Text` 提供對應至AEM [Text元件的元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)。 這是內容元件的 **範例** ，其中會從 *AEM轉譯內* 容。

讓我們看看元件的運作方式。

### 檢查JSON模型

1. 在跳至SPA程式碼之前，請務必瞭解AEM提供的JSON模型。 導覽至核 [心元件庫](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) ，並檢視文字元件的頁面。 核心元件庫提供所有AEM核心元件的範例。
2. 選取 **其中一個範例的JSON** 標籤：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性： `text`、 `richText`和 `:type`。

   `:type` 是一個保留屬性，會列 `sling:resourceType` 出AEM元件的（或路徑）。 值是用 `:type` 來將AEM元件對應至SPA元件的值。

   `text` 以 `richText` 及SPA元件所暴露的其他屬性。

### 檢查Text元件

1. 開啟新的終端機，並導覽至專案 `ui.frontend` 內的資料夾。 執行 `npm install` 並啟 `npm start` 動Webpack開 **發伺服器**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   模 `ui.frontend` 組目前已設定為使用 [模擬JSON模型](./integrate-spa.md#mock-json)。

2. 您應會看到新的瀏覽器視窗開啟至 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![具有模擬內容的Webpack開發伺服器](assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 展開模 `ui.frontend` 塊並在以下位 **置開啟檔案text.component.ts** `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.js Angular元件原始碼](assets/map-components/vscode-ide-text-js.png)

4. 第一個要檢查的區域是 `class TextComponent` 第35行：

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

   [@Input()](https://angular.io/api/core/Input) decorator可用來宣告透過映射的JSON物件（先前已檢閱）設定值的欄位。

   `@HostBinding('innerHtml') get content()` 是一種方法，它會從的值中公開編寫的文字內容 `this.text`如果內容是富文本（由標誌確定），則 `this.richText` 會繞過Angular的內置安全性。 Angular的 [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) 可用來「拖曳」原始HTML並防止跨網站指令碼弱點。 該方法使用@HostBinding `innerHtml` decorator綁定到 [屬性](https://angular.io/api/core/HostBinding) 。

5. 接下來檢查 `TextEditConfig` 第24行：

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上述程式碼負責決定何時在AEM作者環境中呈現預留位置。 如果方 `isEmpty` 法傳 **回true** ，則會呈現預留位置。

6. 最後，請看第53 `MapTo` 行的通話：

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo由** AEM SPA Editor JS SDK(`@adobe/cq-angular-editable-components`)提供。 路徑 `wknd-spa-angular/components/text` 代表 `sling:resourceType` AEM元件。 此路徑與先前觀察到 `:type` 的JSON模型公開的路徑相符。 **MapTo** 剖析JSON模型回應，並將正確值傳 `@Input()` 遞至SPA元件的變數。

   您可在找到AEM元 `Text` 件定義 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`。

7. 修改 **en.model.json檔案** ，以進行實驗 `ui.frontend/src/mocks/json/en.model.json`。

   在~line 62更新第一個 `Text` 值，以使用 **`H1`** 和 **`u`** 標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   返回瀏覽器，查看webpack dev **server提供的效果**:

   ![更新的文字模型](assets/map-components/updated-text-model.png)

   嘗試在true `richText` / **false之間切****** 換屬性，以檢視演算邏輯的實際運作。

8. 在 **檢查text.component.html**`ui.frontend/src/app/components/text/text.component.html`。

   此檔案為空，因為該元件的整個內容將由屬性設 `innerHTML` 置。

9. 請在 **檢查app.module.ts**`ui.frontend/src/app/app.module.ts`。

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

   TextComponent **未明確包含** ，而是透過AEM SPA Editor JS SDK提供的 **AEMResponsiveGridComponent** ，動態加入。 因此，必須列在 **app.module.ts**&#39; [entryComponents陣列中](https://angular.io/guide/entry-components) 。

## 建立影像元件

接著，建立 `Image` 對應至AEM [Image元件的Angular元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/components/image.translate.html)。 該 `Image` 元件是內容元件的 **另一示例** 。

### 檢查JSON

在跳至SPA程式碼之前，請先檢查AEM提供的JSON模型。

1. 導覽至核心 [元件庫中的影像範例](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)。

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   將使 `src`用、 `alt`和 `title` 的屬性填充SPA組 `Image` 件。

   >[!NOTE]
   >
   > 還有其他顯示(`lazyEnabled`, `widths`)的影像屬性可讓開發人員建立可調式和延遲載入的元件。 本教學課程中建立的元件將很簡單， **不會** 使用這些進階屬性。

2. 返回IDE並開啟 `en.model.json` at `ui.frontend/src/mocks/json/en.model.json`。 由於這是專案的新元件，因此我們需要「模擬」影像JSON。

   在~70行新增模型的JSON `image` 項目(別忘記第二個字元後面 `,` 的尾 `text_386303036`隨逗號)並更新 `:itemsOrder` 陣列。

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

   該項目包含將與 `/mock-content/adobestock-140634652.jpeg` webpack dev server一起使用的 **示例映像**。

   您可以在這裡檢 [視完整的en.model.json](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json)。

3. 新增要由元件顯示的圖庫像片。

   在下方建立名為「影像 **」的新資** 料夾 `ui.frontend/src/mocks`。 下載 [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) ，並將它置入新建立的 **影像資料夾** 。 如有需要，請自由使用您自己的影像。

### 實作影像元件

1. 停止 **webpack dev server** （如果已啟動）。
2. 通過從資料夾內運行Angular CLI命令，創 `ng generate component` 建新的映像 `ui.frontend` 元件：

   ```shell
   $ ng generate component components/image
   ```

3. 在IDE中，開啟 **image.component.ts** , `ui.frontend/src/app/components/image/image.component.ts` 並按如下方式更新：

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

   `@Input()` of `src`、 `alt`和 `title` 是從JSON API映射的屬性。

   `hasImage()` 是一種方法，可決定是否應呈現影像。

   `MapTo` 將SPA元件對應至位於的AEM元件 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`。

4. 開啟 **image.component.html** ，並依下列方式更新：

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   如果傳回 `<img>` true，則 `hasImage` 會轉換 **元素**。

5. 開啟 **image.component.scss** ，並依下列方式更新：

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
   > 規 `:host-context` 則對於 **AEM SPA編輯器預留位置** (placeholder)而言至關重要，可正常運作。 所有要在AEM頁面編輯器中編寫的SPA元件至少需要此規則。

6. 打 `app.module.ts` 開並添加 `ImageComponent` 到陣 `entryComponents` 列：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   與 `TextComponent`類似， `ImageComponent` 動態載入，且必須包含在陣 `entryComponents` 列中。

7. 啟動 **webpack dev server** ，查看演 `ImageComponent` 算。

   ```shell
   $ npm run start:mock
   ```

   ![已新增影像至模擬](assets/map-components/image-added-mock.png)

   *已添加到SPA的映像*

   >[!NOTE]
   >
   > **獎金挑戰**:實作新方法，將值顯示為影 `title` 像下方的標題。

## AEM中的更新政策

組 `ImageComponent` 件僅顯示在 **webpack dev server中**。 接著，將更新的SPA部署至AEM並更新範本原則。

1. 停止 **webpack dev server** ，並從專案的 **** 根目錄，使用您的Maven技能將變更部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從「AEM開始」畫面導覽至「工 **[!UICONTROL 具]** >範本 **[!UICONTROL >]** WKND SPA Angular **[](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**」。

   選擇並編輯 **SPA頁**:

   ![編輯SPA頁面範本](assets/map-components/edit-spa-page-template.png)

3. 選取「 **配置容器** 」，然後按一下其 **** 原則圖示以編輯原則：

   ![配置容器原則](./assets/map-components/layout-container-policy.png)

4. 在「 **允許的元件** > **WKND SPA角度——內容** >檢查 **** 影像元件：

   ![已選取影像元件](assets/map-components/check-image-component.png)

   在「 **Default Components** > **Add mapping** (預設元件 **>添加映射)」下，選擇「** Image - WKND SPA Angular - Content（影像- WKND SPA角度——內容元件）」:

   ![設定預設元件](assets/map-components/default-components.png)

   輸入 **MIME類型**`image/*`。

   按一下 **完成** ，保存策略更新。

5. 在「版 **面容器** 」中，按一 **下Text元件的** 原則圖示 **** :

   ![文本元件策略表徵圖](./assets/map-components/edit-text-policy.png)

   建立名為 **WKND SPA Text的新策略**。 在「外 **掛程式** >格 **式設定** >選取所有方塊以啟用其他格式設定選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在「 **外掛程式** >段落樣式 **>」下方，核取「啟用段落樣式」** 方塊 ****:

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下 **完成** ，保存策略更新。

6. 導覽至首 **頁**[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。

   您也可以編輯元件，並 `Text` 在全螢幕模式中新 **增其他段落** 。

   ![全螢幕豐富式文字編輯](assets/map-components/full-screen-rte.png)

7. 您也應該能夠從資產搜尋器拖放影 **像**:

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過 [AEM Assets](http://localhost:4502/assets.html/content/dam) ，新增您自己的影像，或安裝標準 [WKND參考網站的完成程式碼庫](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKND參 [考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) ，包含許多可在WKND SPA上重新使用的影像。 您可使用 [AEM的Package Manager來安裝套件](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## 檢查版面容器

AEM SPA編輯 **器SDK會自動提供「版面容器** 」支援。 「 **版面容器**」（由名稱指示）是容器 **元件** 。 容器元件是接受JSON結構的元件，可代表其 *他元件* ，並動態執行個體化。

讓我們進一步檢查「版面容器」。

1. 在IDE中， **在以下位置開啟responsive-grid.component** .ts `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   此 `AEMResponsiveGridComponent` 軟體是以AEM SPA Editor SDK的一部份實作，並透過下列方式包含在專案中 `import-components`。

2. 在瀏覽器中導覽至 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON模型API —— 自適應格線](./assets/map-components/responsive-grid-modeljson.png)

   Layout Container **元件有一** 個， `sling:resourceType` SPA編輯器會使用屬性來識別它，就像和元 `wcm/foundation/components/responsivegrid` 件一 `:type``Text``Image` 樣。

   SPA編輯器也提供使用版面模式重新調整 [元件大小](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 的相同功能。

3. 返回 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 新增其 **他影像** ，並嘗試使用「版面配置」選項重新調整 **其大小** :

   ![使用版面模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

4. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) ，並 `columnClassNames` 觀察JSON的一部分：

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名表 `aem-GridColumn--default--4` 示元件應基於12列網格為4列寬。 如需互動式格線 [的詳細資訊，請參閱這裡](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

5. 返回IDE，在模組中 `ui.apps` 有定義於的客戶端庫 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`。 開啟檔案 `less/grid.less`。

   此檔案會決定「版面`default`容器」 `tablet`所使 `phone`用的中 **斷點**。 此檔案旨在根據專案規格自訂。 目前，中斷點設為 `1200px` 和 `650px`。

6. 您應該可以使用元件的回應功能和更新的豐富文字原 `Text` 則來製作如下的檢視：

   ![章節範例最終編寫](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜您，您已學習如何將SPA元件對應至AEM元件，並實作了新的元 `Image` 件。 您也有機會探索版面容器的互動 **功能**。

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ，或切換至分支，在本機檢出程式碼 `Angular/map-components-solution`。

### 後續步驟 {#next-steps}

[導覽和路由](navigation-routing.md) -瞭解如何透過SPA編輯器SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用Angular Router實現的，並添加到現有的Header元件中。

## 額外——保留源控制的配置 {#bonus}

在許多情況下，尤其是在AEM專案開始時，將設定（例如範本和相關內容原則）保留至來源控制非常有用。 這可確保所有開發人員針對相同的內容和組態進行工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟度，管理模板的做法就可以交給一組特殊的超級用戶。

接下來的幾個步驟將會使用Visual Studio程式碼IDE和 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ，但可能會使用您設定為從AEM的本機例項提取或匯入內容的任何工具和任何IDE ******** 。

1. 在Visual Studio代碼IDE中，請確定您已透過Marketplace擴充功能 **安裝VSCode AEM Sync** :

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展開「 **專案檔案總管」中的ui.content** 模組，並導覽至 `/conf/wknd-spa-angular/settings/wcm/templates`。

3. **以滑鼠右鍵** +按一 `templates` 下資料夾，然後選 **取「從AEM Server匯入」**:

   ![VSCode匯入範本](assets/map-components/import-aem-servervscode.png)

4. 重複這些步驟以匯入內容，但選取位 **於的** 「原則」檔案夾 `/conf/wknd-spa-angular/settings/wcm/templates/policies`。

5. 檢查位 `filter.xml` 於的檔案 `ui.content/src/main/content/META-INF/vault/filter.xml`。

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

   文 `filter.xml` 件負責標識隨軟體包一起安裝的節點的路徑。 請注意 `mode="merge"` 每個篩選器上的，指出現有內容不會修改，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此請務必不要覆寫程 **式碼** 。 有關使用 [篩選元素的詳細資訊](https://jackrabbit.apache.org/filevault/filter.html) ，請參閱FileVault檔案。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 並 `ui.apps/src/main/content/META-INF/vault/filter.xml` 瞭解每個模組管理的不同節點。
