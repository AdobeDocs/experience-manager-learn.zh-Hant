---
title: 整合SPA | AEM SPA Editor and Angular快速入門
description: 瞭解如何將在Angular中撰寫的單頁應用程式(SPA)原始程式碼與Adobe Experience Manager (AEM)專案整合。 瞭解如何使用現代前端工具(例如Angular的CLI工具)，以針對AEM JSON模型API快速開發SPA。
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
duration: 536
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2045'
ht-degree: 0%

---

# 整合SPA {#integrate-spa}

瞭解如何將在Angular中撰寫的單頁應用程式(SPA)原始程式碼與Adobe Experience Manager (AEM)專案整合。 瞭解如何使用現代前端工具（如Webpack開發伺服器），以針對AEM JSON模型API快速開發SPA。

## 目標

1. 瞭解SPA專案如何與AEM和使用者端程式庫整合。
2. 瞭解如何使用本機開發伺服器來進行專屬的前端開發。
3. 探索使用&#x200B;**proxy**&#x200B;和靜態&#x200B;**Mock**&#x200B;檔案來針對AEM JSON模型API開發

## 您將建置的內容

本章會將簡單的`Header`元件新增至SPA。 在建立此靜態`Header`元件的過程中，使用了數種AEM SPA開發方法。

![在AEM中新增標題](./assets/integrate-spa/final-header-component.png)

*已延伸SPA以新增靜態`Header`元件*

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. 使用Maven將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)，請新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`Angular/integrate-spa-solution`在本機取出程式碼。

## 整合方法 {#integration-approach}

已建立兩個模組作為AEM專案的一部分： `ui.apps`和`ui.frontend`。

`ui.frontend`模組是包含所有SPA原始碼的[webpack](https://webpack.js.org/)專案。 大部分的SPA開發和測試都在webpack專案中完成。 觸發生產組建時，會使用webpack建置及編譯SPA。 編譯的成品（CSS和JavaScript）會複製到`ui.apps`模組，然後部署至AEM執行階段。

![ui.frontend高階架構](assets/integrate-spa/ui-frontend-architecture.png)

*SPA整合的高階描述。*

有關前端組建的其他資訊可在[此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=zh-Hant)找到。

## 檢查SPA整合 {#inspect-spa-integration}

接下來，請檢查`ui.frontend`模組以瞭解由[AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=zh-Hant)自動產生的SPA。

1. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 此教學課程將使用[Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=zh-Hant#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA專案](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展開並檢查`ui.frontend`資料夾。 開啟檔案`ui.frontend/package.json`

3. 在`dependencies`下，您應該會看到數個與`@angular`相關的專案：

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

   `ui.frontend`模組是使用包含路由的[Angular CLI工具](https://angular.io/cli)產生的[Angular應用程式](https://angular.io)。

4. 同時有三個前置詞為`@adobe`的相依性：

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上述模組構成[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html?lang=zh-Hant)，並提供可將SPA元件對應至AEM元件的功能。

5. 在`package.json`檔案中定義了多個`scripts`：

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   這些指令碼是以一般[Angular CLI命令](https://angular.io/cli/build)為基礎，但已稍作修改，以便與較大的AEM專案搭配使用。

   `start` — 使用本機Web伺服器在本機執行Angular App。 它已更新，以代理本機AEM執行個體的內容。

   `build` — 編譯Angular應用程式以進行生產分送。 加入`&& clientlib`負責在建置期間將編譯的SPA作為使用者端程式庫複製到`ui.apps`模組中。 npm模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)已用來協助處理這個問題。

   有關可用指令碼的更多詳細資料，請參閱[這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=zh-Hant)。

6. 檢查檔案`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)使用此設定檔來決定如何產生使用者端程式庫。

7. 檢查檔案`ui.frontend/pom.xml`。 此檔案會將`ui.frontend`資料夾轉換為[Maven模組](https://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`檔案已更新，以便在Maven組建期間使用[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)至&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

8. 在`ui.frontend/src/app/app.component.ts`檢查檔案`app.component.ts`：

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

   `app.component.js`是SPA的進入點。 `ModelManager`由AEM SPA Editor JS SDK提供。 它負責呼叫`pageModel` （JSON內容）並將其插入應用程式。

## 新增標頭元件 {#header-component}

接下來，將新元件新增至SPA，並將變更部署至本機AEM執行個體，以檢視整合。

1. 開啟新的終端機視窗並瀏覽至`ui.frontend`資料夾：

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 全域安裝[Angular CLI](https://angular.io/cli#installing-angular-cli)這是用來產生Angular元件，以及透過&#x200B;**ng**&#x200B;命令建置及提供Angular應用程式。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 此專案使用的&#x200B;**@angular/cli**&#x200B;版本是&#x200B;**9.1.7**。 建議讓Angular CLI版本維持同步。

3. 從`ui.frontend`資料夾內執行Angular CLI `ng generate component`命令，以建立新的`Header`元件。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   這將會在`ui.frontend/src/app/components/header`為新的Angular Header元件建立骨架。

4. 在您選擇的IDE中開啟`aem-guides-wknd-spa`專案。 導覽至 `ui.frontend/src/app/components/header` 檔案夾。

   IDE中的![Header元件路徑](assets/integrate-spa/header-component-path.png)

5. 開啟檔案`header.component.html`並將內容取代為下列內容：

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   請注意，這會顯示靜態內容，因此此Angular元件不需要對預設產生的`header.component.ts`進行任何調整。

6. 在`ui.frontend/src/app/app.component.html`開啟檔案&#x200B;**app.component.html**。 新增`app-header`：

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   這會在所有頁面內容上方包含`header`元件。

7. 開啟新的終端機，並導覽至`ui.frontend`資料夾並執行`npm run build`命令：

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 導覽至`ui.apps`資料夾。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`下方，您應該會看到已從`ui.frontend/build`資料夾複製編譯的SPA檔案。

   ![在ui.apps中產生的使用者端資料庫](assets/integrate-spa/compiled-spa-uiapps.png)

9. 返回終端機並導覽至`ui.apps`資料夾。 執行下列Maven命令：

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

   這會將`ui.apps`封裝部署至AEM的本機執行個體。

10. 開啟瀏覽器索引標籤並導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您現在應該會看到`Header`元件的內容顯示在SPA中。

   ![初始標頭實作](assets/integrate-spa/initial-header-implementation.png)

   從專案的根目錄（即`mvn clean install -PautoInstallSinglePackage`）觸發Maven組建時，會自動執行步驟&#x200B;**7至9**。 您現在應該瞭解SPA與AEM使用者端程式庫之間整合的基本概念。 請注意，您仍然可以在AEM中編輯與新增`Text`元件，但`Header`元件不可編輯。

## Webpack Dev Server - JSON API的Proxy {#proxy-json}

如先前練習所示，執行組建並將使用者端程式庫同步至AEM的本機執行個體需要幾分鐘的時間。 這對於最終測試來說是可接受的，但對於大多數SPA開發而言並不理想。

可以使用[webpack開發伺服器](https://webpack.js.org/configuration/dev-server/)來快速開發SPA。 SPA由AEM產生的JSON模型驅動。 在本練習中，來自AEM執行個體的JSON內容&#x200B;**已代理**&#x200B;至由[Angular專案](https://angular.io/guide/build)設定的開發伺服器。

1. 返回IDE並在`ui.frontend/proxy.conf.json`開啟檔案&#x200B;**proxy.conf.json**。

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

   [Angular應用程式](https://angular.io/guide/build#proxying-to-a-backend-server)提供簡單的機制來代理API要求。 `context`中指定的模式是透過本機AEM快速入門`localhost:4502`進行代理處理。

2. 在`ui.frontend/src/index.html`開啟檔案&#x200B;**index.html**。 這是開發伺服器使用的根HTML檔案。

   請注意，`base href="/"`有一個專案。 [基底標籤](https://angular.io/guide/deployment#the-base-tag)對於應用程式解析相對URL而言至關重要。

   ```html
   <base href="/">
   ```

3. 開啟終端機視窗並導覽至`ui.frontend`資料夾。 執行命令`npm start`：

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

4. 開啟新的瀏覽器索引標籤（如果尚未開啟），並導覽至[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)。

   ![Webpack開發伺服器 — Proxy json](assets/integrate-spa/webpack-dev-server-1.png)

   您應該會看到與AEM相同的內容，但未啟用任何撰寫功能。

5. 返回IDE並在`ui.frontend/src/assets`建立名為`img`的新資料夾。
6. 下載下列WKND標誌並新增至`img`資料夾：

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

   將變更儲存至&#x200B;**header.component.html**。

8. 返回瀏覽器。 您應該會立即看到應用程式的變更反映出來。

   ![標誌已新增至標頭](assets/integrate-spa/added-logo-localhost.png)

   您可以繼續在&#x200B;**AEM**&#x200B;中進行內容更新，並看到這些更新反映在&#x200B;**webpack開發伺服器**&#x200B;中，因為我們正在代理內容。 請注意，內容變更只會顯示在&#x200B;**webpack開發伺服器**&#x200B;中。

9. 停止終端機中具有`ctrl+c`的本機Web伺服器。

## Webpack Dev Server - Mock JSON API {#mock-json}

快速開發的另一種方法是使用靜態JSON檔案充當JSON模型。 透過「嘲弄」 JSON，我們移除對本機AEM執行個體的相依性。 它也能讓前端開發人員更新JSON模型，以測試功能並推動JSON API的變更，之後再由後端開發人員實作。

模擬JSON的初始設定&#x200B;**需要本機AEM執行個體**。

1. 在瀏覽器中導覽至[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   這是AEM匯出的驅動應用程式JSON。 複製JSON輸出。

2. 返回IDE導覽至`ui.frontend/src`並新增名為&#x200B;**mocks**&#x200B;和&#x200B;**json**&#x200B;的新資料夾，以符合下列資料夾結構：

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 在`ui.frontend/public/mocks/json`下建立名為&#x200B;**en.model.json**&#x200B;的新檔案。 從這裡貼上&#x200B;**步驟1**&#x200B;的JSON輸出。

   ![模擬模型Json檔案](assets/integrate-spa/mock-model-json-created.png)

4. 在`ui.frontend`下方建立新檔案&#x200B;**proxy.mock.conf.json**。 將下列專案填入檔案中：

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

   此Proxy設定會重寫以`/content/wknd-spa-angular/us`開頭並為`/mocks/json`的請求，並提供對應的靜態JSON檔案，例如：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 開啟檔案&#x200B;**angular.json**。 新增具有已更新&#x200B;**資產**&#x200B;陣列的新&#x200B;**dev**&#x200B;設定，以參考已建立的&#x200B;**mocks**&#x200B;資料夾。

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

   ![Angular JSON Dev Assets更新資料夾](assets/integrate-spa/dev-assets-update-folder.png)

   建立專用的&#x200B;**dev**&#x200B;設定可確保&#x200B;**mocks**&#x200B;資料夾僅在開發期間使用，而且絕對不會部署至生產組建中的AEM。

6. 在&#x200B;**angular.json**&#x200B;檔案中，接著更新&#x200B;**browserTarget**&#x200B;設定以使用新的&#x200B;**dev**&#x200B;設定：

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

   ![Angular JSON組建開發更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. 開啟檔案`ui.frontend/package.json`並新增新的&#x200B;**start：mock**&#x200B;命令以參照&#x200B;**proxy.mock.conf.json**&#x200B;檔案。

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

   新增指令可讓您在Proxy設定之間輕鬆切換。

8. 如果目前正在執行，請停止&#x200B;**webpack開發伺服器**。 使用&#x200B;**start：mock**&#x200B;指令碼啟動&#x200B;**webpack dev server**：

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   導覽至[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)，您應該會看到相同的SPA，但現在正在從&#x200B;**模型** JSON檔案提取內容。

9. 對先前建立的&#x200B;**en.model.json**&#x200B;檔案進行小幅變更。 更新後的內容應立即反映在&#x200B;**webpack開發伺服器**&#x200B;中。

   ![模型的json更新](./assets/integrate-spa/webpack-mock-model.gif)

   能夠操控JSON模型並檢視對即時SPA的影響有助於開發人員瞭解JSON模型API。 此外，前端和後端開發均可並行進行。

## 使用Sass新增樣式

接著，將一些更新的樣式新增至專案。 此專案將針對變數等幾項有用的功能新增[Sass](https://sass-lang.com/)支援。

1. 開啟終端機視窗並停止&#x200B;**webpack開發伺服器** （如果啟動）。 從`ui.frontend`資料夾內輸入以下命令，以更新Angular應用程式以處理&#x200B;**.scss**&#x200B;檔案。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   這會在檔案底部以新專案更新`angular.json`檔案：

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. 安裝`normalize-scss`以跨瀏覽器標準化樣式：

   ```shell
   $ npm install normalize-scss --save
   ```

3. 返回IDE，並在`ui.frontend/src`下方建立名為`styles`的新資料夾。
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

5. 將位於`ui.frontend/src/styles.css`的檔案&#x200B;**styles.css**&#x200B;的副檔名重新命名為&#x200B;**styles.scss**。 以下列專案取代內容：

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

6. 更新&#x200B;**angular.json**，並使用&#x200B;**styles.scss**&#x200B;重新命名&#x200B;**style.css**&#x200B;的所有參考。 應該有3個參考。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 更新標題樣式

接著，使用Sass將一些品牌特定樣式新增至&#x200B;**Header**&#x200B;元件。

1. 啟動&#x200B;**webpack dev server**&#x200B;以即時檢視樣式更新：

   ```shell
   $ npm run start:mock
   ```

2. 在`ui.frontend/src/app/components/header`底下，將&#x200B;**header.component.css**&#x200B;重新命名為&#x200B;**header.component.scss**。 將下列專案填入檔案中：

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

3. 更新&#x200B;**header.component.ts**&#x200B;以參照&#x200B;**header.component.scss**：

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

4. 返回瀏覽器與&#x200B;**webpack開發伺服器**：

   ![樣式標頭 — webpack dev server](assets/integrate-spa/styled-header.png)

   您現在應該會看到更新的樣式已新增至&#x200B;**Header**&#x200B;元件。

## 將SPA更新部署至AEM

對&#x200B;**標頭**&#x200B;所做的變更目前只能透過&#x200B;**webpack開發伺服器**&#x200B;顯示。 將更新的SPA部署至AEM以檢視變更。

1. 停止&#x200B;**webpack開發伺服器**。
2. 導覽至專案`/aem-guides-wknd-spa`的根目錄，並使用Maven將專案部署至AEM：

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您應該會看到已套用標誌和樣式的更新&#x200B;**Header**：

   ![已更新AEM中的標題](assets/integrate-spa/final-header-component.png)

   現在，更新的SPA已匯入AEM，製作作業可以繼續進行。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA並探索與AEM整合！ 您現在知道使用&#x200B;**webpack開發伺服器**&#x200B;針對AEM JSON模型API開發SPA的兩種不同方法。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上檢視完成的程式碼，或切換至分支`Angular/integrate-spa-solution`在本機取出程式碼。

### 後續步驟 {#next-steps}

[將SPA元件對應至AEM元件](map-components.md) — 瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓作者在AEM SPA Editor中對SPA元件進行動態更新，類似於傳統的AEM編寫。
