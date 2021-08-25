---
title: 整合SPA |開始使用AEM SPA編輯器和Angular
description: 了解以Angular撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 了解如何使用Angular的CLI工具等現代前端工具，針對AEM JSON模型API快速開發SPA。
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2191'
ht-degree: 0%

---


# 整合SPA {#integrate-spa}

了解以Angular撰寫的單頁應用程式(SPA)原始碼如何與Adobe Experience Manager(AEM)專案整合。 了解如何使用現代化的前端工具（例如WebPack開發伺服器），針對AEM JSON模型API快速開發SPA。

## 目標

1. 了解SPA專案如何與AEM與用戶端程式庫整合。
2. 了解如何使用本機開發伺服器進行專屬的前端開發。
3. 探索使用&#x200B;**proxy**&#x200B;和靜態&#x200B;**mock**&#x200B;檔案，針對AEM JSON模型API進行開發

## 您將建置的

本章將向SPA添加簡單的`Header`元件。 在建置此靜態`Header`元件的過程中，將使用數種AEM SPA開發方法。

![AEM中的新標題](./assets/integrate-spa/final-header-component.png)

*擴充SPA以新增靜態元 `Header` 件*

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`Angular/integrate-spa-solution`在本機檢出程式碼。

## 整合方法 {#integration-approach}

已在AEM專案中建立兩個模組：`ui.apps`和`ui.frontend`。

`ui.frontend`模組是包含所有SPA原始碼的[webpack](https://webpack.js.org/)專案。 大部分的SPA開發與測試將在Webpack專案中完成。 觸發生產組建時，會使用webpack建置及編譯SPA。 編譯的成品（CSS和Javascript）會複製到`ui.apps`模組中，然後部署到AEM執行階段。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*對SPA整合的高階描述。*

有關前端版本編號的其他資訊，請參見[這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

## Inspect SPA整合 {#inspect-spa-integration}

接下來，檢查`ui.frontend`模組，了解[AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)自動產生的SPA。

1. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 本教程將使用[Visual Studio代碼IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA專案](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展開並檢查`ui.frontend`資料夾。 開啟檔案`ui.frontend/package.json`

3. 在`dependencies`下，應該會看到幾個與`@angular`相關的項目：

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   `ui.frontend`模組是使用[AngularCLI工具](https://angular.io/cli)生成的[Angular應用程式](https://angular.io)，該工具包含路由。

4. 此外，還有三個前置詞為`@adobe`的相依性：

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上述模組組成[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)，並提供功能，讓您能夠將SPA元件對應至AEM元件。

5. 在`package.json`檔案中，定義了多個`scripts`:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   這些指令碼基於通用的[AngularCLI命令](https://angular.io/cli/build)，但已稍作修改以與較大的AEM項目一起使用。

   `start`  — 使用本機Web伺服器在本機執行Angular應用程式。已更新，以代理本機AEM例項的內容。

   `build`  — 編譯Angular應用程式以進行生產發佈。新增`&& clientlib`負責在建置期間將編譯的SPA複製到`ui.apps`模組，作為用戶端程式庫。 npm模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)用於促進此操作。

   有關可用指令碼的更多詳細資訊，請參見[此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

6. Inspect檔案`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)使用此配置檔案來確定如何生成客戶端庫。

7. Inspect檔案`ui.frontend/pom.xml`。 此檔案將`ui.frontend`資料夾轉換為[Maven模組](https://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`檔案已更新，以在Maven建置期間使用[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)至&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

8. Inspect檔案`app.component.ts`(`ui.frontend/src/app/app.component.ts`):

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` 是SPA的入口。`ModelManager` 由AEM SPA Editor JS SDK提供。它負責呼叫應用程式並將`pageModel`（JSON內容）插入。

## 新增標題元件 {#header-component}

接著，將新元件新增至SPA，並將變更部署至本機AEM例項以查看整合。

1. 開啟新的終端機視窗，並導覽至`ui.frontend`資料夾：

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 全局安裝[AngularCLI](https://angular.io/cli#installing-angular-cli)這用於生成Angular元件，以及通過&#x200B;**ng**&#x200B;命令構建和提供Angular應用程式。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 此項目使用的&#x200B;**@angular/cli**&#x200B;版本為&#x200B;**9.1.7**。 建議保持AngularCLI版本同步。

3. 從`ui.frontend`資料夾內運行AngularCLI `ng generate component`命令，建立新的`Header`元件。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   這將在`ui.frontend/src/app/components/header`為新Angular頭元件建立骨架。

4. 在所選IDE中開啟`aem-guides-wknd-spa`項目。 導覽至`ui.frontend/src/app/components/header`資料夾。

   ![IDE中的標頭元件路徑](assets/integrate-spa/header-component-path.png)

5. 開啟檔案`header.component.html`並用以下內容替換內容：

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   請注意，這會顯示靜態內容，因此此Angular元件不需要對預設產生的`header.component.ts`進行任何調整。

6. 在`ui.frontend/src/app/app.component.html`開啟檔案&#x200B;**app.component.html**。 新增`app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   這會包含所有頁面內容上方的`header`元件。

7. 開啟新終端機並導覽至`ui.frontend`資料夾並執行`npm run build`命令：

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 導覽至`ui.apps`資料夾。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`下方，應該會看到已編譯的SPA檔案已從`ui.frontend/build`資料夾複製。

   ![在ui.apps中產生的用戶端程式庫](assets/integrate-spa/compiled-spa-uiapps.png)

9. 返回終端機並導覽至`ui.apps`資料夾。 執行以下Maven命令：

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   這會將`ui.apps`套件部署至本機執行中的AEM例項。

10. 開啟瀏覽器標籤並導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您現在應該會在SPA中看到`Header`元件的內容。

   ![初始標題實作](assets/integrate-spa/initial-header-implementation.png)

   從專案根目錄觸發Maven組建時（即`mvn clean install -PautoInstallSinglePackage`），會自動執行步驟&#x200B;**7-9**。 您現在應了解SPA和AEM用戶端程式庫之間整合的基本知識。 請注意，您仍可以編輯和新增AEM中的`Text`元件，但是`Header`元件不可編輯。

## Webpack開發伺服器 — 代理JSON API {#proxy-json}

如先前的練習所示，執行組建，並將用戶端程式庫同步至AEM的本機例項需要幾分鐘的時間。 這是最終測試可接受的選項，但不適用於大部分SPA開發。

[webpack開發伺服器](https://webpack.js.org/configuration/dev-server/)可用來快速開發SPA。 SPA是由AEM產生的JSON模型驅動。 在本練習中，來自AEM執行個體的JSON內容將&#x200B;**proxided**&#x200B;至由[Angular專案](https://angular.io/guide/build)設定的開發伺服器。

1. 返回IDE，在`ui.frontend/proxy.conf.json`開啟檔案&#x200B;**proxy.conf.json**。

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   [Angular應用程式](https://angular.io/guide/build#proxying-to-a-backend-server)提供簡單的代理API請求機制。 在`context`中指定的模式通過本地AEM快速啟動`localhost:4502`進行複製。

2. 在`ui.frontend/src/index.html`開啟檔案&#x200B;**index.html**。 這是開發伺服器使用的根HTML檔案。

   請注意，`base href="/"`中有一個條目。 [base標籤](https://angular.io/guide/deployment#the-base-tag)對於應用程式解析相對URL而言至關重要。

   ```html
   <base href="/">
   ```

3. 開啟終端機視窗，並導覽至`ui.frontend`資料夾。 運行命令`npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. 開啟新的瀏覽器標籤（如果尚未開啟），並導覽至[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)。

   ![Webpack開發伺服器 — 代理json](assets/integrate-spa/webpack-dev-server-1.png)

   您應該會看到與AEM相同的內容，但未啟用任何製作功能。

5. 返回IDE，在`ui.frontend/src/assets`處建立名為`img`的新資料夾。
6. 下載以下WKND標誌並添加到`img`資料夾中：

   ![WKND標誌](./assets/integrate-spa/wknd-logo-dk.png)

7. 在`ui.frontend/src/app/components/header/header.component.html`開啟&#x200B;**header.component.html**&#x200B;並包含標誌：

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   儲存對&#x200B;**header.component.html**&#x200B;的變更。

8. 返回瀏覽器。 您應會立即看到應用程式的變更反映在內。

   ![標題中新增標誌](assets/integrate-spa/added-logo-localhost.png)

   由於我們代理內容，因此您可以繼續在&#x200B;**AEM**&#x200B;中更新內容，並看到它們反映在&#x200B;**webpack開發伺服器**&#x200B;中。 請注意，內容變更只會顯示在&#x200B;**Webpack開發伺服器**&#x200B;中。

9. 停止終端中`ctrl+c`的本地Web伺服器。

## Webpack開發伺服器 — 模擬JSON API {#mock-json}

另一種快速開發的方法是使用靜態JSON檔案作為JSON模型。 借由「模擬」JSON，我們移除了對本機AEM例項的相依性。 此外，前端開發人員也能更新JSON模型，以測試功能，並推動對JSON API的變更，JSON API稍後將由後端開發人員實作。

模擬JSON的初始設定&#x200B;**需要本機AEM例項**。

1. 在瀏覽器中，導覽至[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   這是由AEM匯出的JSON，此JSON會驅動應用程式。 複製JSON輸出。

2. 返回到IDE，導航到`ui.frontend/src`並添加名為&#x200B;**mocks**&#x200B;和&#x200B;**json**&#x200B;的新資料夾，以匹配以下資料夾結構：

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 在`ui.frontend/public/mocks/json`下方建立名為&#x200B;**en.model.json**&#x200B;的新檔案。 從&#x200B;**Step 1**&#x200B;貼上JSON輸出至此處。

   ![模擬模型Json檔案](assets/integrate-spa/mock-model-json-created.png)

4. 在`ui.frontend`下方建立新檔案&#x200B;**proxy.mock.conf.json**。 將下列項目填入檔案：

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   此代理配置將重寫以`/content/wknd-spa-angular/us`開頭的`/mocks/json`請求，並提供對應的靜態JSON檔案，例如：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 開啟檔案&#x200B;**angular.json**。 使用更新的&#x200B;**assets**&#x200B;陣列新增新的&#x200B;**dev**&#x200B;設定，以參考已建立的&#x200B;**mocks**&#x200B;資料夾。

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![AngularJSON開發資產更新資料夾](assets/integrate-spa/dev-assets-update-folder.png)

   建立專用的&#x200B;**dev**&#x200B;設定，可確保&#x200B;**mocks**&#x200B;資料夾僅在開發期間使用，且不會部署至生產組建中的AEM。

6. 在&#x200B;**angular.json**&#x200B;檔案中，下一步更新&#x200B;**browserTarget**&#x200B;設定，以使用新的&#x200B;**dev**&#x200B;設定：

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![AngularJSON建置開發更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. 開啟檔案`ui.frontend/package.json`並新增新的&#x200B;**start:mock**&#x200B;命令以參考&#x200B;**proxy.mock.conf.json**&#x200B;檔案。

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   新增命令可讓您輕鬆在代理設定之間切換。

8. 如果當前正在運行，請停止&#x200B;**Webpack開發伺服器**。 使用&#x200B;**start:mock**&#x200B;指令碼啟動&#x200B;**webpack開發伺服器**:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   導覽至[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)，您應該會看到相同的SPA，但現在會從&#x200B;**mock** JSON檔案提取內容。

9. 對先前建立的&#x200B;**en.model.json**&#x200B;檔案進行小幅變更。 更新的內容應立即反映在&#x200B;**webpack開發伺服器**&#x200B;中。

   ![模擬模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

   能夠操控JSON模型並查看即時SPA上的效果，可協助開發人員了解JSON模型API。 它還允許前端和後端開發並行進行。

## 使用Sass新增樣式

接著，專案將新增一些已更新的樣式。 此專案將新增[Sass](https://sass-lang.com/)支援以使用變數等一些實用功能。

1. 開啟終端窗口，如果啟動，則停止&#x200B;**Webpack開發伺服器**。 從`ui.frontend`資料夾內輸入以下命令以更新Angular應用程式以處理&#x200B;**.scss**&#x200B;檔案。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   這將更新`angular.json`檔案，並在檔案底部添加新條目：

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. 安裝`normalize-scss`以標準化瀏覽器間的樣式：

   ```shell
   $ npm install normalize-scss --save
   ```

3. 返回到IDE，在`ui.frontend/src`下建立名為`styles`的新資料夾。
4. 在`ui.frontend/src/styles`下建立名為`_variables.scss`的新檔案，並填入下列變數：

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. 將位於`ui.frontend/src/styles.css`的檔案&#x200B;**styles.css**&#x200B;的副檔名重新命名為&#x200B;**styles.scs**。 將內容替換為：

   ```scss
   /* styles.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. 更新&#x200B;**angular.json**，並使用&#x200B;**styles.scs**&#x200B;重新命名&#x200B;**style.css**&#x200B;的所有參考。 應該有3個參考。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 更新標題樣式

接下來，使用Sass將一些品牌專屬樣式新增至&#x200B;**Header**&#x200B;元件。

1. 啟動&#x200B;**Webpack開發伺服器**&#x200B;以即時查看樣式更新：

   ```shell
   $ npm run start:mock
   ```

2. 在`ui.frontend/src/app/components/header`下，將&#x200B;**header.component.css**&#x200B;重新命名為&#x200B;**header.component.scss**。 將下列項目填入檔案：

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. 更新&#x200B;**header.component.js**&#x200B;以參考&#x200B;**header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. 返回瀏覽器和&#x200B;**Webpack開發伺服器**:

   ![樣式化標題 — webpack開發伺服器](assets/integrate-spa/styled-header.png)

   您現在應該會看到已新增至&#x200B;**Header**&#x200B;元件的樣式。

## 將SPA更新部署至AEM

對&#x200B;**Header**&#x200B;所做的變更目前只能透過&#x200B;**webpack開發伺服器**&#x200B;顯示。 將更新的SPA部署至AEM以查看變更。

1. 停止&#x200B;**Webpack開發伺服器**。
2. 導覽至專案`/aem-guides-wknd-spa`的根目錄，並使用Maven將專案部署至AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您應該會看到已更新的&#x200B;**Header**，並套用標誌和樣式：

   ![更新AEM中的標題](assets/integrate-spa/final-header-component.png)

   現在更新的SPA已在AEM中，製作即可繼續。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA並探索與AEM的整合！ 您現在知道使用&#x200B;**webpack開發伺服器**，針對AEM JSON模型API開發SPA的兩種方法。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`Angular/integrate-spa-solution`在本機檢出程式碼。

### 後續步驟 {#next-steps}

[將SPA元件對應至AEM元件](map-components.md)  — 了解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager(AEM)元件。元件對應可讓作者在AEM SPA編輯器中對SPA元件進行動態更新，與傳統AEM製作類似。
