---
title: 將SPA元件對應至AEM元件 |開始使用AEM SPA編輯器和Angular
description: 了解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2372'
ht-degree: 0%

---

# 將SPA元件對應至AEM元件 {#map-components}

了解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。 元件對應可讓使用者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。

本章更深入探討AEM JSON模型API，以及AEM元件公開的JSON內容如何以prop形式自動插入Angular元件。

## 目標

1. 了解如何將AEM元件對應至SPA元件。
2. 了解 **容器** 元件和 **內容** 元件。
3. 建立新的Angular元件，將其映射至現有的AEM元件。

## 您將建置的

本章將檢查提供的 `Text` SPA元件已對應至AEM `Text`元件。 新 `Image` SPA元件已建立，可在SPA中使用，並在AEM中製作。 的現成功能 **版面容器** 和 **範本編輯器** 也將使用策略來建立外觀更多樣的視圖。

![章節範例最終製作](./assets/map-components/final-page.png)

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   若使用 [AEM 6.x](overview.md#compatibility) 新增 `classic` 設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或切換到分支，本地檢出代碼 `Angular/map-components-solution`.

## 映射方法

基本概念是將SPA元件對應至AEM元件。 AEM元件、執行伺服器端，將內容匯出為JSON模型API的一部分。 SPA會使用JSON內容，在瀏覽器中執行用戶端。 系統會建立SPA元件與AEM元件之間的1:1對應。

![將AEM元件對應至Angular元件的概觀](./assets/map-components/high-level-approach.png)

*將AEM元件對應至Angular元件的概觀*

## Inspect the Text Component

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 提供 `Text` 對應至AEM的元件 [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 這是 **內容** 元件，在中呈現 *內容* 從AEM。

讓我們查看元件的運作方式。

### Inspect JSON模型

1. 在跳入SPA程式碼之前，請務必了解AEM提供的JSON模型。 導覽至 [核心元件程式庫](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 並查看文本元件的頁面。 核心元件程式庫提供所有AEM核心元件的範例。
2. 選取 **JSON** 標籤中的一個示例：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性： `text`, `richText`，和 `:type`.

   `:type` 是保留的屬性，它列出 `sling:resourceType` （或路徑）。 的值 `:type` 用來將AEM元件對應至SPA元件。

   `text` 和 `richText` 是公開給SPA元件的其他屬性。

### Inspect the Text元件

1. 開啟新終端機並導覽至 `ui.frontend` 資料夾。 執行 `npm install` 然後 `npm start` 開始 **webpack開發伺服器**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   此 `ui.frontend` 模組目前已設定為使用 [模擬JSON模型](./integrate-spa.md#mock-json).

2. 您應會看到新的瀏覽器視窗開啟至 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![具有模擬內容的Webpack開發伺服器](assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 展開 `ui.frontend` 模組並開啟檔案 **text.component.ts** 在 `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.jsAngular元件原始碼](assets/map-components/vscode-ide-text-js.png)

4. 要檢查的第一個區域是 `class TextComponent` 在第35行：

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

   [@Input()](https://angular.io/api/core/Input) decorator可用來宣告哪些欄位的值是透過已對應的JSON物件設定，且先前已檢閱。

   `@HostBinding('innerHtml') get content()` 是一種方法，會從 `this.text`. 若內容為RTF(由 `this.richText` 標幟)會略過Angular的內建安全性。 Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) 用來「拖曳」原始HTML，並防止跨網站指令碼漏洞。 方法會系結至 `innerHtml` 屬性使用 [@HostBinding](https://angular.io/api/core/HostBinding) 裝飾師。

5. 下一步檢查 `TextEditConfig` 在第24行：

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上述程式碼負責決定何時在AEM製作環境中呈現預留位置。 若 `isEmpty` 方法傳回 **true** 然後呈現佔位符。

6. 最後，請查看 `MapTo` ~第53行呼叫：

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** 由AEM SPA Editor JS SDK提供(`@adobe/cq-angular-editable-components`)。 路徑 `wknd-spa-angular/components/text` 代表 `sling:resourceType` 的AEM元件。 此路徑符合 `:type` 由先前觀察到的JSON模型公開。 **MapTo** 剖析JSON模型回應，並將正確值傳遞至 `@Input()` SPA元件的變數。

   您可以找到AEM `Text` 元件定義 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. 修改 **en.model.json** 檔案位置 `ui.frontend/src/mocks/json/en.model.json`.

   ~第62行更新第一個 `Text` 值 **`H1`** 和 **`u`** 標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   返回瀏覽器，查看 **webpack開發伺服器**:

   ![更新的文字模型](assets/map-components/updated-text-model.png)

   嘗試切換 `richText` 介於 **true** / **false** 以查看實際運作中的轉譯邏輯。

8. Inspect **text.component.html** at `ui.frontend/src/app/components/text/text.component.html`.

   此檔案為空，因為元件的整個內容由 `innerHTML` 屬性。

9. Inspect **app.module.ts** at `ui.frontend/src/app/app.module.ts`.

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

   此 **TextComponent** 未明確包含，而是透過動態 **AEMResponsiveGridComponent** 由AEM SPA Editor JS SDK提供。 因此，必須列於 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 陣列。

## 建立影像元件

接下來，建立 `Image` Angular元件，對應至AEM [影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 此 `Image` 元件是 **內容** 元件。

### Inspect JSON

在跳入SPA程式碼之前，請檢查AEM提供的JSON模型。

1. 導覽至 [核心元件程式庫中的影像範例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   屬性 `src`, `alt`，和 `title` 用來填入SPA `Image` 元件。

   >[!NOTE]
   >
   > 有其他已公開的影像屬性(`lazyEnabled`, `widths`)，讓開發人員建立最適化和延遲載入元件。 本教學課程中建置的元件簡單，而且是 **not** 使用這些進階屬性。

2. 返回到IDE並開啟 `en.model.json` at `ui.frontend/src/mocks/json/en.model.json`. 由於這是專案的全新元件，因此我們需要「模擬」影像JSON。

   在~70行，為 `image` 模型(別忘了尾隨逗號 `,` 在第二個 `text_386303036`)和更新 `:itemsOrder` 陣列。

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

   專案包含的範例影像位於 `/mock-content/adobestock-140634652.jpeg` 與 **webpack開發伺服器**.

   您可以檢視完整 [en.model.json，此處](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. 新增要由元件顯示的庫存像片。

   建立名為 **影像** 在 `ui.frontend/src/mocks`. 下載 [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) 並將其置於新建立的 **影像** 檔案夾。 如有需要，歡迎使用您自己的影像。

### 實作影像元件

1. 停止 **webpack開發伺服器** 中。
2. 運行AngularCLI以建立新映像元件 `ng generate component` 從內 `ui.frontend` 資料夾：

   ```shell
   $ ng generate component components/image
   ```

3. 在IDE中，開啟 **image.component.ts** at `ui.frontend/src/app/components/image/image.component.ts` 並更新如下：

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

   `ImageEditConfig` 是設定，可根據 `src` 屬性已填入。

   `@Input()` of `src`, `alt`，和 `title` 是從JSON API對應的屬性。

   `hasImage()` 是可判斷是否應轉譯影像的方法。

   `MapTo` 將SPA元件對應至位於的AEM元件 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. 開啟 **image.component.html** 並更新如下：

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   這會使 `<img>` 元素 `hasImage` 傳回 **true**.

5. 開啟 **image.component.scss** 並更新如下：

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
   > 此 `:host-context` 規則 **關鍵** 讓AEM SPA編輯器預留位置可正常運作。 所有打算在AEM頁面編輯器中製作的SPA元件至少都需要此規則。

6. 開啟 `app.module.ts` 並新增 `ImageComponent` 到 `entryComponents` 陣列：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   贊 `TextComponent`, `ImageComponent` 會動態載入，且必須包含在 `entryComponents` 陣列。

7. 啟動 **webpack開發伺服器** 查看 `ImageComponent` 呈現。

   ```shell
   $ npm run start:mock
   ```

   ![模擬中新增的影像](assets/map-components/image-added-mock.png)

   *已新增影像至SPA*

   >[!NOTE]
   >
   > **獎金挑戰**:實作新方法以顯示 `title` 作為影像下方的標題。

## 更新AEM中的原則

此 `ImageComponent` 元件只會顯示在 **webpack開發伺服器**. 接著，將更新的SPA部署至AEM並更新範本原則。

1. 停止 **webpack開發伺服器** 和 **根** 在專案中，使用您的Maven技能將變更部署至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從AEM開始畫面導覽至 **[!UICONTROL 工具]** > **[!UICONTROL 範本]** > **[WKND SPAAngular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   選取並編輯 **SPA頁面**:

   ![編輯SPA頁面範本](assets/map-components/edit-spa-page-template.png)

3. 選取 **版面容器** 按一下 **原則** 表徵圖可編輯策略：

   ![佈局容器策略](./assets/map-components/layout-container-policy.png)

4. 在 **允許的元件** > **WKND SPAAngular — 內容** >檢查 **影像** 元件：

   ![已選取影像元件](assets/map-components/check-image-component.png)

   在 **預設元件** > **新增對應** 並選擇 **影像 — WKND SPAAngular — 內容** 元件：

   ![設定預設元件](assets/map-components/default-components.png)

   輸入 **mime類型** of `image/*`.

   按一下 **完成** 以保存策略更新。

5. 在 **版面容器** 按一下 **原則** 圖示 **文字** 元件：

   ![文本元件策略表徵圖](./assets/map-components/edit-text-policy.png)

   建立新策略，命名為 **WKND SPA文字**. 在 **外掛程式** > **格式** >選中所有框以啟用其他格式選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   在 **外掛程式** > **段落樣式** >核取方塊 **啟用段落樣式**:

   ![啟用段落樣式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下 **完成** 以保存策略更新。

6. 導覽至 **首頁** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   您也應該可以編輯 `Text` 元件和在 **全螢幕** 模式。

   ![全螢幕RTF編輯](assets/map-components/full-screen-rte.png)

7. 您也應該可以從 **資產尋找器**:

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過 [AEM Assets](http://localhost:4502/assets.html/content/dam) 或安裝標準程式碼庫的完整版 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest). 此 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 包含許多可在WKND SPA上重複使用的影像。 可使用 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect版面容器

支援 **版面容器** 由AEM SPA Editor SDK自動提供。 此 **版面容器**，如名稱所示，為 **容器** 元件。 容器元件是接受JSON結構的元件，代表 *其他* 元件，並以動態方式具現化。

讓我們進一步檢查「版面容器」。

1. 在IDE開啟中 **responsive-grid.component.ts** at `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   此 `AEMResponsiveGridComponent` 是作為AEM SPA Editor SDK的一部分實作，並透過 `import-components`.

2. 在瀏覽器中導覽至 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON模型API — 回應式格線](./assets/map-components/responsive-grid-modeljson.png)

   此 **版面容器** 元件具有 `sling:resourceType` of `wcm/foundation/components/responsivegrid` 和是由SPA編輯器識別，使用 `:type` 屬性，就像 `Text` 和 `Image` 元件。

   使用重新調整元件大小的功能相同 [版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 可與SPA編輯器搭配使用。

3. 返回 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 新增其他 **影像** 並嘗試使用 **版面** 選項：

   ![使用「佈局」模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

4. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 並觀察 `columnClassNames` 作為JSON的一部分：

   ![列類名](./assets/map-components/responsive-grid-classnames.png)

   類名 `aem-GridColumn--default--4` 指出元件應寬4列（基於12列網格）。 有關 [可在此處找到回應式格線](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. 返回到IDE，並在 `ui.apps` 模組有在 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. 開啟檔案 `less/grid.less`.

   此檔案確定斷點(`default`, `tablet`，和 `phone`)使用 **版面容器**. 此檔案旨在根據項目規格進行定制。 當前斷點設定為 `1200px` 和 `650px`.

6. 您應該可以使用 `Text` 製作檢視的元件，如下所示：

   ![章節範例最終製作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜您，您已學習如何將SPA元件對應至AEM元件，並實作了新的 `Image` 元件。 您也可以探索 **版面容器**.

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或切換到分支，本地檢出代碼 `Angular/map-components-solution`.

### 後續步驟 {#next-steps}

[導航和路由](navigation-routing.md)  — 了解如何使用SPA Editor SDK對應至AEM頁面，以支援SPA中的多個檢視。 動態導航是使用Angular路由器實現的，並添加到現有的標頭元件中。

## 額外 — 保留原始碼控制的配置 {#bonus}

在許多情況下，尤其是在AEM專案開始時，最好將設定（例如範本和相關內容原則）保留到原始碼控制。 這可確保所有開發人員針對相同的內容和設定組合工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟程度，管理模板的做法就可以交給特定的一組電源用戶。

接下來的幾個步驟將使用Visual Studio Code IDE和 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 但可以使用任何工具和配置為 **提取** 或 **匯入** 來自本機AEM例項的內容。

1. 在Visual Studio代碼IDE中，確保您 **VSCode AEM同步** 透過Marketplace擴充功能安裝：

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展開 **ui.content** 模組，並導覽至 `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **按一下右鍵** the `templates` 資料夾和選取 **從AEM伺服器匯入**:

   ![VSCode匯入範本](assets/map-components/import-aem-servervscode.png)

4. 重複匯入內容的步驟，但選取 **原則** 位於 `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect `filter.xml` 位於 `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   此 `filter.xml` 檔案負責標識隨軟體包一起安裝的節點的路徑。 請注意 `mode="merge"` 在指出不會修改現有內容的每個篩選器上，只會新增新內容。 由於內容作者可能更新這些路徑，因此程式碼部署必須確實更新 **not** 覆寫內容。 請參閱 [FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html) 以取得使用篩選元素的詳細資訊。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解每個模組管理的不同節點。
