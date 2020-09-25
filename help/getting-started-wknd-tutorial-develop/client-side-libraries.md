---
title: 用戶端程式庫與前端工作流程
description: 瞭解如何使用用戶端資料庫或用戶端資料庫來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 本教學課程也將說明如何將webpack專案ui.frontend模組整合至端對端建置程式。
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 0%

---


# 用戶端程式庫與前端工作流程 {#client-side-libraries}

瞭解如何使用用戶端資料庫或用戶端資料庫來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 本教學課程也將涵蓋 [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組(解耦的 [webpack](https://webpack.js.org/) project)如何整合至端對端建置程式。

## 必備條件 {#prerequisites}

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

此外，建議您檢閱元件基 [本功能教學課程](component-basics.md#client-side-libraries) ，以瞭解用戶端程式庫和AEM的基礎。

### Starter Project

查看教學課程所建立的基線程式碼：

1. 克隆github.com/adobe/aem-guides-wknd [儲存庫](https://github.com/adobe/aem-guides-wknd) 。
1. 查看分 `client-side-libraries/start` 支

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. 使用您的Maven技巧，將程式碼庫部署至本機AEM實例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) ，或切換至分支，在本機檢出程式碼 `client-side-libraries/solution`。

## 目標

1. 瞭解如何透過可編輯的範本將用戶端程式庫包含在頁面上。
1. 瞭解如何使用UI.Frontend Module和網頁套件開發伺服器進行專屬的前端開發。
1. 瞭解將編譯的CSS和JavaScript傳送至網站實作的端對端工作流程。

## 您將建立的 {#what-you-will-build}

在本章中，您將新增WKND網站和「文章頁面範本」的一些基準樣式，以便讓實作更接近 [UI設計模型](assets/pages-templates/wknd-article-design.xd)。 您將使用進階的前端工作流程，將Webpack專案整合至AEM用戶端程式庫。

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## 背景 {#background}

用戶端程式庫提供組織和管理AEM網站實作所需CSS和JavaScript檔案的機制。 用戶端程式庫或用戶端程式庫的基本目標為：

1. 將CSS/JS儲存在小型的獨立檔案中，以方便開發和維護
1. 以有組織的方式管理對第三方架構的依賴
1. 將CSS/JS串連至一或兩個請求，將用戶端請求數減至最少。

有關使用用戶 [端程式庫的詳細資訊，請參閱這裡。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

用戶端程式庫確實有一些限制。 最值得注意的是，對常用前端語言（例如Sass、LESS和TypeScript）的支援有限。 在教學課程中，我們將瞭解 **** ui.frontend模組如何協助解決此問題。

將入門程式碼庫部署至本機AEM實例，並導覽至 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此頁面目前未設定樣式。 我們接下來將實作WKND品牌的用戶端程式庫，以新增CSS和Javascript至頁面。

## 用戶端程式庫組織 {#organization}

接下來，我們將探討 [AEM Project Archetype產生的clientlibs組織](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)。

![高階客戶程式庫組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖用戶端程式庫組織與頁面包含*

>[!NOTE]
>
> 下列用戶端程式庫組織由AEM Project Archetype產生，但僅代表一個起點。 專案如何最終管理CSS和Javascript並將它傳送至Sites實作，會因資源、技能和需求而大幅不同。

1. 使用Eclipse或其他IDE開啟 **ui.apps模組** 。
1. 展開路徑 `/apps/wknd/clientlibs` 以檢視原型產生的clientlib。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   我們將在下面詳細檢查這些clientlib。

1. 檢查屬性 `clientlibs/clientlib-base`。

   **clientlib-base** 代表WKND網站運作所需的CSS和JavaScript基本層級。 請注意設 `categories` 置為的屬性 `wknd.base`。 `categories` 是clientlibs的標籤機制，也是它們的引用方式。

   請注意 `embed` 屬性和 `String[]` 值。 該屬 `embed` 性根據其類別嵌入其他客戶端。 **clientlib-base** 將包含所有需要的AEM Core Component clientlibrary。 這包括要運作的Carousel、Quick search元件等物件。 **clientlib-base不會包含** 其本身的任何CSS和Javascript，但只會內嵌其他用戶端程式庫。 **clientlib-base** 嵌入 **clientlib-grid** clientlib的類別 `wknd.grid`。

   請注意 `allowProxy` 將屬性設定為 `true`。 始終在clientlibs上設定是最佳 `allowProxy=true` 做法。 此屬 `allowProxy` 性可讓我們將clientlib與我們的應用程式碼一起儲存在 `/apps`****`/etc.clientlibs` Butter Clientlibs中，但接著會以前置詞的路徑傳送clientlibs，以避免將任何應用程式碼洩露給使用者。 如需allowProxy屬性 [的詳細資訊，請參閱這裡](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)。

1. 檢查屬性 `clientlibs/clientlib-grid`。

   **clientlib-grid** 負責包含／產生「版面」模式所 [需的CSS](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html) ，以便搭配AEM Sites編輯器使用。 **clientlib-grid** 有一個類別設定為， `wknd.grid` 並通過 **clientlib-base嵌入**。

   您可自訂格線，以使用不同數量的欄和中斷點。 接下來，我們將更新生成的預設斷點。

1. 更新檔 `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`案：

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   這會變更中斷點，以對應我們在中設定的範本中斷點 `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`。

   請注意，此檔案實際上會參 `grid_base.less` 照包含自 `/libs` 訂混音的檔案，以產生格線。

1. 檢查屬性 `clientlibs/clientlib-site`。

   **clientlib-site** 將包含WKND品牌的所有特定網站樣式。 請注意的類別 `wknd.site`。 產生此clientlib的CSS和Javascript實際上會維護在模組中 `ui.frontend` 。 我們接下來將探討此項整合。

1. 檢查屬性 `clientlibs/clientlib-dependencies`。

   **clientlib-dependencies** is inted to embed any third party dependencies. 它是個別的clientlib，因此可視需要將它載入HTML頁面的頂端。 請注意的類別 `wknd.dependencies`。 產生此clientlib的CSS和Javascript實際上會維護在模組中 `ui.frontend` 。 我們將在教學課程的稍後部份探索此項整合。

## 使用ui.frontend模組 {#ui-frontend}

接下來，我們將探討 **[ui.frontend模組的使用](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 。

### 動機

在支援 [Sass](https://sass-lang.com/) 或 [TypeScript等語言時，用戶端程式庫有一](https://www.typescriptlang.org/)些限制。 此外，開放原始碼工具(例如 [NPM](https://www.npmjs.com/)[和webpack](https://webpack.js.org/) )也大量湧現，可加速並最佳化前端開發。

ui.frontend模組的基本理念是 **** ，能夠使用像NPM和Webpack這樣的絕佳工具來管理大部份的前端開發。 在 **ui.frontend** 模組中內建的重要整合項目 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) ，會從webpack/npm專案擷取已編譯的CSS和JS對象，並將它們轉換為AEM用戶端程式庫。 這可讓前端開發人員更自由地選擇不同的工具和技術。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

### 使用

現在，我們將透過ui.frontend模組新增一些Sass檔案(`.scss` 擴充功能)，為WKND品 **牌新增基本樣式** 。

1. 開啟 **ui.frontend模組** ，並導覽至 `src/main/webpack/base/sass`。

   ![ui.frontend模組](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. 在資料夾下建立名 `_variables.scss` 為的新檔案 `src/main/webpack/base/sass`。
1. 填 `_variables.scss` 入下列：

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sass可讓我們建立變數，然後可在不同檔案中使用這些變數，以確保一致性。 請注意字型系列。 在教學課程的稍後部份，我們將瞭解如何加入對Google網頁字型的呼叫，以便使用這些字型。

1. 建立下方的另一 `_elements.scss` 個檔 `src/main/webpack/base/sass` 案，並填入下列檔案：

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   請注意， `_elements.scss` 檔案會使用中的變數 `_variables.scss`。

1. 更新 `_shared.scss` 下方 `src/main/webpack/base/sass` 以包含 `_elements.scss` 和檔 `_variables.scss` 案。

   ```css
   @import './variables';
   @import './elements';
   ```

1. 開啟命令列終端，然後使 **用命令安裝ui.frontend**`npm install` 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需在新克隆或生成項目後運行一次。

1. 在同一終端中，使用命令 **建立和部署ui.frontend**`npm run dev` 模組：

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   命令應 `npm run dev` 該為Webpack項目生成並編譯原始碼，並最終在ui.apps模組中填 **入clientlib** -site **** 和clientlib-dependencies **** 。

   >[!NOTE]
   >
   >此外，還有一 `npm run prod` 個描述檔會將JS和CSS減少。 每當透過Maven觸發Webpack組建時，這都是標準編譯。 如需有關 [ui.frontend模組的詳細資訊，請參閱這裡](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. 檢查下方的 `site.css` 檔案 `ui.frontend/dist/clientlib-site/css/site.css`。 請注意，CSS大部分是由先前建立之檔案的內 `_elements.scss` 容所組成，但變數已取代為實際值。

   ![分散式網站css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 檢查檔案 `ui.frontend/clientlib.config.js`。 這是npm外掛程式 [aem-clientlib-generator的設定檔](https://github.com/wcm-io-frontend/aem-clientlib-generator)。 **aem-clientlib-generator** 是負責轉換已編譯的CSS/JavaScript並將它複製至 **ui.apps模組的工具** 。

1. 請在上檢 `site.css` 查 **ui.apps模組中的檔案** 。 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`這應該是來自ui.frontend模 `site.css` 組的 **相同檔案副本** 。 現在它已位於 **ui.apps模組中** ，可將它部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由於 **clientsite** 實際上是在建立時間內使用 **npm** 或 **maven編譯，所以實際上可以從ui.appsLib模組的來源控制中忽略它****** 。 檢查ui. `.gitignore` apps下 **的檔案**。

>[!CAUTION]
>
> 並非所有專 **案都需要使用ui.frontend** 模組。 **ui.frontend** 模組會增加額外的複雜性，如果不需要／想要使用這些進階前端工具(Sass、webpack、npm...)，則可能會過頭。 因此，它被視為AEM Project Archetype的選用部分，而且仍完全支援使用標準用戶端程式庫和Vanilla CSS和JavaScript。

## 頁面和範本包含 {#page-inclusion}

接下來，我們將檢視專案的設定方式，以將clientlibs包含在AEM範本／頁面中。 在網頁開發中，最常見的最佳實務是在結束標籤之前，將CSS `<head>` 加入HTML標題和JavaScript `</body>` 中。

1. 在 **ui.apps模組中** ，導覽至 `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`。

   ![結構頁元件](assets/client-side-libraries/customheaderlibs-html.png)

   這是用 `page` 於呈現WKND實作中所有頁面的元件。

1. 開啟檔案 `customheaderlibs.html`。 請注意這些行 `${clientlib.css @ categories='wknd.base'}`。 這表示包含類別的clientlib的CSS將會透過此檔 `wknd.base` 案加入，有效地將 **clientlib** -base包含在我們所有頁面的標題中。

1. 更新 `customheaderlibs.html` 以包含我們先前在ui.frontend模組中指定之Google字型樣 **式的參考** 。 我們目前也將注釋掉ContextHub...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. 檢查檔案 `customfooterlibs.html`。 此檔案(如 `customheaderlibs.html` 實施專案時)會被覆寫。 這一行表 `${clientlib.js @ categories='wknd.base'}` 示來自clientlib-base **的JavaScript** ，將包含在我們所有頁面的底部。

1. 使用Maven，建立專案並部署至本機AEM例項：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 瀏覽至http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd上的WKND范 [本](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)。

1. 在範本編輯器中選 **取並開啟「文章頁面** 」範本。

   ![選取文章頁面範本](assets/client-side-libraries/open-article-page-template.png)

1. 按一下「 **頁面資訊** 」圖示，並在功能表中選取「頁面原則 **」以開啟「頁** 面原則 **** 」對話方塊。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面政策*

1. 請注意，此處列 `wknd.dependencies` 出了 `wknd.site` 和的類別。 依預設，會分割透過「頁面原則」設定的clientlib，以在頁面標題中包含CSS，並在內文結尾處包含JavaScript。 如有需要，您可明確列出要將clientlib JavaScript載入頁面標題中。 這就是原因 `wknd.dependencies`。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 您也可以直接使用或 `wknd.site` 指令碼 `wknd.dependencies` ，從頁面元件參考或，就像我們在clientlib的前面看到的檔案管 `customheaderlibs.html` 理員一樣 `customfooterlibs.html``wknd.base` 。 使用範本可提供一些彈性，讓您可以挑選並選擇每個範本使用的客戶端。 例如，如果您有非常繁重的JavaScript程式庫，而且只會用於選取的範本。

1. 導覽至使 **用「文章頁面範本** 」建立的 **LA Skateparks頁面**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 您應該會看到字型和套用的一些基本樣式有所差異，以指出在 **ui.frontend模組中建立的CSS** 運作正常。

1. 按一下「 **頁面資訊** 」圖示，然後在功能表中選取「檢 **視為已發佈** 」，以在AEM編輯器外開啟文章頁面。

   ![以已發佈狀態檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 檢視http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled的「頁 [面」來源](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) ，您應該可以在下列位置看到clientlib參考 `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs使用代理端 `/etc.clientlibs` 點。 您也應該會在頁面底部看到下列clientlib:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >在發佈端，用戶端程式庫無法從 ******/apps提供，這是很重要的，因為此路徑應使用** Dispatcher篩選區段以安全理由加以限制 [](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)。 用戶 [端程式庫的allowProxy屬性](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) ，可確保CSS和JS可從/etc.clientlibs取得 **服務**。

## Webpack DevServer {#webpack-dev-server}

在前幾項練習中，我們可以更新 **** ui.frontend模組中的數個Sass檔案，並透過建立程式，最終在AEM中看到這些變更。 接下來，我們將探討如何運 [用webpack-dev-server](https://webpack.js.org/configuration/dev-server/) ，快速開發我們的前端樣式。

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

以下是影片中顯示的高階步驟：

1. 從ui.frontend模組中運行以下命令，以啟 **動webpack dev server** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在http://localhost:8080/上開啟新的瀏覽器 [視窗](http://localhost:8080/) ，並加上靜態標籤。
1. 複製LA skatepark文章頁面的頁面來源，網址為 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 將複製的標籤從AEM貼入 `index.html` ui. **frontend模組下方**`src/main/webpack/static`。
1. 編輯複製的標籤，並移除 **clientlib-site和clientlib** - **dependencies的任何參照**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我們可以刪除這些引用，因為webpack dev伺服器將自動生成這些對象。

1. 編輯檔 `.scss` 案，並查看自動反映在瀏覽器中的變更。
1. 檢閱檔 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 案。 這包含用於啟動webpack-dev-server的webpack配置。 請注意，它會代理路 `/content` 徑和 `/etc.clientlibs` 來自本機執行的AEM例項。 這是影像和其他clientlib(非由 **ui.frontend程式碼管理** )的可用方式。

   >[!CAUTION]
   >
   > 靜態標籤的影像src會指向本機AEM例項上的即時影像元件。 如果影像路徑變更、AEM未啟動，或瀏覽器使用者未登入本機AEM例項，影像將會顯示中斷。
1. 您可 **以輸入** ，從命令行中停止webpack伺服器 `CTRL+C`。

## 整合 {#putting-it-together}

本教學課程的重點在於用戶端程式庫和可與AEM整合的潛在前端工作流程。 有鑑於此，我們將透過安裝 [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip)，來加速實作，此為「文章頁面範本」上使用的「核心」元件提供一些預設樣式：

* [階層連結](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [下載](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [影像](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/components/image.translate.html)
* [清單](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [導覽](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [快速搜尋](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [分隔符號](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

以下是影片中顯示的高階步驟：

1. 下 [載用戶端資料庫-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) ，並解壓縮下方的內容 `ui.frontend/src/main/webpack`。 郵遞區號的內容應覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. 使用webpack dev伺服器預覽新樣式：

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 將程式碼庫部署至本機AEM實例，以檢視套用至LA滑板公園文章的新樣式：

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## 恭喜！ {#congratulations}

恭喜的是，「文章頁面」現在有一些與WKND品牌相符的一致樣式，而且您已熟悉 **ui.frontend** 模組！

### 後續步驟 {#next-steps}

瞭解如何使用Experience Manager的Style System建置個別樣式並重複使用核心元件。 [使用樣式系統進行開發](style-system.md) ，涵蓋使用樣式系統，以特定品牌的CSS和範本編輯器的進階原則組態來擴充核心元件。

在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd) ，或在Git筆刷上在本機檢視並部署程式碼 `client-side-libraries/solution`。

1. 克隆github.com/adobe/aem-wknd-guides [儲存庫](https://github.com/adobe/aem-guides-wknd) 。
1. 看看那 `client-side-libraries/solution` 根樹枝。

## 其他工具與資源 {#additional-resources}

### aemfed {#develop-aemfed}

[**aemfed**](https://aemfed.io/) 是開放原始碼的命令列工具，可用來加速前端開發。 它由 [aemsync](https://www.npmjs.com/package/aemsync)、 [Browsersync](https://www.npmjs.com/package/browser-sync) 和 [Sling Log Tracer提供](https://sling.apache.org/documentation/bundles/log-tracers.html)。

高階Aemfed的 **設計** ，是用來監聽 **** ui.apps模組中的檔案變更，並直接自動同步至執行中的AEM例項。 本端瀏覽器會根據變更自動重新整理，進而加速前端開發。 此外，它還可與Sling Log Tracer搭配使用，以直接在終端中自動顯示任何伺服器端錯誤。

如果您在 **ui.apps模組中進行許多工作、修改HTL指令碼和建立自訂元件，** Aemfed **** 可以是非常強大的工具。 [您可在這裡找到完整的檔案。](https://github.com/abmaonline/aemfed).

### 除錯用戶端程式庫 {#debugging-clientlibs}

使用不同的類別 **和內嵌** , **以包含多個用戶端程式庫** ，疑難排解的麻煩就大了。 AEM提供數種工具來協助處理此問題。 最重要的工具之一是 **Rebuild Client Libraries** ，這會強制AEM重新編譯任何LESS檔案並產生CSS。

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) —— 列出在AEM例項中註冊的所有用戶端程式庫。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) —— 允許用戶根據類別查看clientlib的預期HTML輸出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**庫相依性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出顯示找不到的任何相依性或嵌入類別。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rebuild Client Libraries**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) —— 允許使用者強制AEM重建所有用戶端程式庫，或使用戶端程式庫的快取失效。 此工具在使用LESS進行開發時特別有效，因為這會迫使AEM重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理與重建所有程式庫比較有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客戶端庫](assets/client-side-libraries/rebuild-clientlibs.png)
