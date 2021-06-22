---
title: 用戶端程式庫和前端工作流程
description: 了解如何使用用戶端資料庫或clientlib來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 本教學課程也說明如何將ui.frontend模組（Webpack專案）整合至端對端建置程式。
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心元件、AEM專案原型
topic: 內容管理、開發
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 32320905786682a852baf7d777cb06de0072c439
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 0%

---


# 用戶端程式庫和前端工作流程{#client-side-libraries}

了解如何使用用戶端資料庫或clientlib來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 本教學課程也將說明如何將[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)模組（解耦[webpack](https://webpack.js.org/)專案）整合至端對端建置程式。

## 必備條件 {#prerequisites}

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

建議您檢閱[元件基本知識](component-basics.md#client-side-libraries)教學課程，以了解用戶端程式庫和AEM的基本知識。

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/client-side-libraries-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用您的Maven技能，將程式碼基底部署至本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)上檢視完成的程式碼，或切換至分支`tutorial/client-side-libraries-solution`在本機檢出程式碼。

## 目標

1. 了解透過可編輯的範本將用戶端程式庫納入頁面的方式。
1. 了解如何使用UI.Frontend模組和Web Pack開發伺服器進行專用的前端開發。
1. 了解將編譯的CSS和JavaScript傳遞至Sites實作的端對端工作流程。

## 您要建立的{#what-you-will-build}

在本章中，您將為WKND網站和文章頁面範本新增一些基線樣式，以便讓實作更接近[ UI設計模型](assets/pages-templates/wknd-article-design.xd)。 您將使用進階前端工作流程，將Webpack專案整合至AEM用戶端程式庫。

![已完成樣式](assets/client-side-libraries/finished-styles.png)

*已套用基線樣式的文章頁面*

## 背景 {#background}

用戶端資料庫提供組織及管理AEM Sites實作所需CSS和JavaScript檔案的機制。 用戶端程式庫或clientlib的基本目標為：

1. 將CSS/JS儲存在小型獨立檔案中，以便進行開發和維護
1. 以有組織的方式管理對第三方框架的依賴
1. 將CSS/JS串連至一或兩個請求，將用戶端請求數減到最少。

有關使用[客戶端庫的更多資訊，請參見此處。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

用戶端程式庫確實有一些限制。 最引人注目的是對熱門前端語言（如Sass、LESS和TypeScript）的有限支援。 在教學課程中，我們將探討&#x200B;**ui.frontend**&#x200B;模組如何協助解決此問題。

將入門程式碼基底部署至本機AEM執行個體，並導覽至[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此頁面目前未設定樣式。 我們接下來將實作WKND品牌的用戶端資料庫，以新增CSS和Javascript至頁面。

## 用戶端程式庫組織 {#organization}

接下來，我們將探索[AEM專案原型](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)產生的clientlib組織。

![高層級客戶程式庫組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖表用戶端程式庫組織與頁面包含*

>[!NOTE]
>
> 下列用戶端程式庫組織是由AEM專案原型產生，但僅代表一個起點。 專案最終如何管理CSS和Javascript以供Sites實作之用，可能會因資源、技能和需求而大不相同。

1. 使用VSCode或其他IDE開啟&#x200B;**ui.apps**&#x200B;模組。
1. 展開路徑`/apps/wknd/clientlibs`以檢視原型產生的clientlib。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   我們將在下文詳細檢查這些客戶端。

1. 下表匯總了客戶端庫。 有關[包括客戶端庫的更多詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)。

   | 名稱 | 說明 | 附註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND網站運作所需的CSS和JavaScript基本層級 | 內嵌核心元件用戶端LIB |
   | `clientlib-grid` | 生成[佈局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)工作所需的CSS。 | 您可在此處設定行動裝置/平板電腦分界點 |
   | `clientlib-site` | 包含WKND站點的站點特定主題 | 由`ui.frontend`模組產生 |
   | `clientlib-dependencies` | 內嵌任何第三方相依性 | 由`ui.frontend`模組產生 |

1. 請注意，從原始碼控制項中忽略`clientlib-site`和`clientlib-dependencies`。 這是根據設計，因為這些將在建置時由`ui.frontend`模組產生。

## 更新基本樣式{#base-styles}

接下來，更新&#x200B;**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;模組中定義的基本樣式。 `ui.frontend`模組中的檔案將生成包含站點主題和任何第三方依賴項的`clientlib-site`和`clientlib-dependecies`庫。

若是支援[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)等語言，用戶端程式庫會有一些限制。 有許多開放原始碼工具，例如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)，可加速並最佳化前端開發。 **ui.frontend**&#x200B;模組的目標是能夠使用這些工具管理大多數前端源檔案。

1. 開啟&#x200B;**ui.frontend**&#x200B;模組並導覽至`src/main/webpack/site`。
1. 開啟檔案`main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` 是模組中所有Sass檔案的入 `ui.frontend` 口。其中將包含`_variables.scss`檔案，該檔案包含要用於專案中不同Sass檔案的一系列品牌變數。 也包含`_base.scss`檔案，並定義HTML元素的一些基本樣式。 規則運算式包含`src/main/webpack/components`下個別元件樣式的所有樣式。 另一個規則運算式包含`src/main/webpack/site/styles`下的所有檔案。

1. 
   1. Inspect檔案`main.ts`。 它包含`main.scss`和規則運算式，用於收集專案中的任何`.js`或`.ts`檔案。 此入口點將被[webpack配置檔案](https://webpack.js.org/configuration/)用作整個`ui.frontend`模組的入口點。

1. Inspect `src/main/webpack/site/styles`下的檔案：

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   範本中全域元素的這些檔案樣式，例如「頁首」、「頁尾」和主要內容容器。 這些檔案中的CSS規則以不同的HTML元素`header`、`main`和`footer`為目標。 這些HTML元素由前一章[頁面和範本](./pages-templates.md)中的原則定義。

1. 展開`src/main/webpack`下的`components`資料夾並檢查檔案。

   ![元件Sass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都對應至核心元件，例如[折疊式功能表元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)。 每個核心元件都以[區塊元素修飾元](https://getbem.com/)或BEM標籤法建置，讓您更容易使用樣式規則來鎖定特定CSS類別。 `/components`下方的檔案已由AEM專案原型匯整，且各元件的BEM規則不同。

1. 下載WKND基本樣式&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)**&#x200B;和&#x200B;**unzip**&#x200B;檔案。

   ![WKND基本樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為加速本教學課程，我們提供數個Sass檔案，這些檔案會根據核心元件和文章頁面範本結構來實作WKND品牌。

1. 使用上一步中的檔案覆蓋`ui.frontend/src`的內容。 zip的內容應會覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect已變更的檔案，以查看WKND樣式實作的詳細資訊。

## Inspect ui.frontend整合{#ui-frontend-integration}

**ui.frontend**&#x200B;模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)內建的重要整合項目會從webpack/npm專案擷取已編譯的CSS和JS成品，並轉換為AEM用戶端程式庫。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM專案原型會自動設定此整合。 接下來，探索其運作方式。


1. 開啟命令行終端機，然後使用`npm install`命令安裝&#x200B;**ui.frontend**&#x200B;模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需執行一次，即可執行新克隆或生成項目。

1. 在同一終端機中，使用`npm run dev`命令建立並部署&#x200B;**ui.frontend**&#x200B;模組：

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > 您可能會收到「錯誤」之類的錯誤。/src/main/webpack/site/main.scss」。
   > 這通常是因為您的環境自執行`npm install`以來已變更。
   > 執行`npm rebuild node-sass`以修正問題。 如果本機開發電腦上安裝的`npm`版本與`aem-guides-wknd/pom.xml`檔案中的Maven `frontend-maven-plugin`版本不同，就會發生此情況。 您可以修改pom檔案中的版本以符合本機版本，或反之亦然，借此永久修正此問題。

1. 命令`npm run dev`應建立並編譯Webpack項目的原始碼，並最終在&#x200B;**ui.apps**&#x200B;模組中填入&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**。

   >[!NOTE]
   >
   >也有`npm run prod`設定檔，將縮制JS和CSS。 這是每當透過Maven觸發Webpack組建時的標準編譯。 如需[ui.frontend模組的詳細資訊，請前往此處](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspect `ui.frontend/dist/clientlib-site/site.css`下方的檔案`site.css`。 這是根據Sass源檔案編譯的CSS。

   ![分佈式網站CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect檔案`ui.frontend/clientlib.config.js`。 這是npm外掛程式[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的設定檔案，可將`/dist`的內容轉換為用戶端程式庫，並將其移至`ui.apps`模組。

1. Inspect **ui.apps**&#x200B;模組`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`中的檔案`site.css`。 這應該是&#x200B;**ui.frontend**&#x200B;模組中`site.css`檔案的相同副本。 現在，它位於&#x200B;**ui.apps**&#x200B;模組中，可部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由於&#x200B;**clientlib-site**&#x200B;是在建置期間使用&#x200B;**npm**&#x200B;或&#x200B;**maven**&#x200B;編譯，因此可以從&#x200B;**ui.apps**&#x200B;模組的原始碼控制項安全地忽略。 Inspect **ui.apps**&#x200B;下方的`.gitignore`檔案。

1. 使用開發人員工具或Maven技能，將`clientlib-site`程式庫與AEM的本機例項同步。

   ![同步Clientlib站點](assets/client-side-libraries/sync-clientlib-site.png)

1. 在AEM中，開啟LA Skatepark文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![更新文章的基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應該會看到文章的更新樣式。 您可能需要執行硬式重新整理，才能清除瀏覽器快取的任何CSS檔案。

   它越來越接近模型了！

   >[!NOTE]
   >
   > 從專案`mvn clean install -PautoInstallSinglePackage`的根觸發Maven組建時，上述為建立ui.frontend程式碼並部署至AEM所執行的步驟會自動執行。

>[!CAUTION]
>
> 並非所有專案都需使用&#x200B;**ui.frontend**&#x200B;模組。 **ui.frontend**&#x200B;模組會增加額外的複雜性，如果不需要/不想使用這些進階前端工具(Sass、webpack、npm...)，則可能不需要它。

## 頁面和範本包含{#page-inclusion}

接下來，我們來檢閱AEM頁面中如何參考clientlib。 Web開發的常見最佳作法是在`</body>`標籤結尾之前，先將CSS包含在HTML標題`<head>`和JavaScript中。

1. 在&#x200B;**ui.apps**&#x200B;模組中導覽至`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`。

   ![結構頁面元件](assets/client-side-libraries/customheaderlibs-html.png)

   這是`page`元件，用於呈現WKND實作中的所有頁面。

1. 開啟檔案`customheaderlibs.html`。 請注意行`${clientlib.css @ categories='wknd.base'}`。 這表示包含`wknd.base`類別之clientlib的CSS將透過此檔案包含，有效地包含在所有頁面的標題中&#x200B;**clientlib-base**。

1. 更新`customheaderlibs.html`以包含先前在&#x200B;**ui.frontend**&#x200B;模組中指定的Google字型樣式參考。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect檔案`customfooterlibs.html`。 此檔案（如`customheaderlibs.html`）可透過實作專案來覆寫。 在此，行`${clientlib.js @ categories='wknd.base'}`表示來自&#x200B;**clientlib-base**&#x200B;的JavaScript將包含在所有頁面的底部。

1. 使用開發人員工具或使用您的Maven技能，將`page`元件匯出至AEM伺服器。

1. 瀏覽至文章頁面範本，網址為[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示，然後在功能表中選取&#x200B;**頁面原則**&#x200B;以開啟&#x200B;**頁面原則**&#x200B;對話方塊。

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面原則*

1. 請注意，`wknd.dependencies`和`wknd.site`的類別列於此處。 依預設，透過「頁面原則」設定的clientlib會被分割，以在頁面標題中包含CSS，並在內文結尾處包含JavaScript。 如有需要，您可明確列出要在頁面標題中載入clientlib JavaScript。 `wknd.dependencies`就是這樣。

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 您也可以直接使用`customheaderlibs.html`或`customfooterlibs.html`指令碼，從頁面元件參考`wknd.site`或`wknd.dependencies`，如前所見`wknd.base` clientlib。 使用範本可提供一些彈性，您可以挑選並選擇每個範本使用的clientlib。 例如，如果您有很重的JavaScript程式庫，該程式庫只會用於選取的範本。

1. 導覽至使用&#x200B;**文章頁面範本**&#x200B;建立的&#x200B;**LA Skateparks**&#x200B;頁面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 字型應該有所不同。

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示，然後在功能表中選取&#x200B;**檢視已發佈**&#x200B;以開啟AEM編輯器外部的文章頁面。

   ![以已發佈狀態檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的頁面源，您應該可以在`<head>`中看到以下clientlib引用：

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs使用Proxy `/etc.clientlibs`端點。 您也應該會看到頁面底部包含下列clientlib:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 如果在6.5/6.4上執行後續操作，用戶端程式庫將不會自動縮制。 請參閱[HTML程式庫管理員上的檔案，以啟用明確化（建議）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)。

   >[!WARNING]
   >
   >在發佈端，用戶端程式庫是&#x200B;**不**&#x200B;從&#x200B;**/apps**&#x200B;提供的，這很重要，因為應使用[Dispatcher篩選器區段](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)，基於安全原因限制此路徑。 用戶端程式庫的[allowProxy屬性](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)可確保從&#x200B;**/etc.clientlibs**&#x200B;提供CSS和JS。

## Webpack DevServer — 靜態標籤{#webpack-dev-static}

在前幾項練習中，我們得以更新&#x200B;**ui.frontend**&#x200B;模組中的數個Sass檔案，並透過建置程式，最終看到AEM中反映的這些變更。 接下來，我們將探討運用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)來針對&#x200B;**static** HTML快速開發前端樣式的技術。

如果大部分的樣式和前端程式碼將由可能無法輕鬆存取AEM環境的專用前端開發人員執行，此技術就十分實用。 此技術也允許FED直接對HTML進行修改，然後HTML就能轉交給AEM開發人員，以作為元件實施。

1. 複製LA Skatepark文章頁面的頁面源，網址為[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 重新開啟IDE。 將從AEM複製的標籤貼入&#x200B;**ui.frontend**&#x200B;模組`src/main/webpack/static`下方的`index.html`。
1. 編輯複製的標籤，並刪除對&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**&#x200B;的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我們可以刪除這些引用，因為Webpack開發伺服器將自動生成這些對象。

1. 從&#x200B;**ui.frontend**&#x200B;模組內執行以下命令，從新終端啟動Webpack開發伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應會在[http://localhost:8080/](http://localhost:8080/)開啟一個新的瀏覽器視窗，其中含有靜態標籤。

1. 編輯檔案`src/main/webpack/site/_variables.scss`檔案。 將`$text-color`規則取代為：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應會自動在[http://localhost:8080](http://localhost:8080)上看到瀏覽器中反映的變更。

   ![本地WebPack開發伺服器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看`/aem-guides-wknd.ui.frontend/webpack.dev.js`檔案。 這包含用於啟動Webpack-dev-server的Webpack配置。 請注意，它會代理來自本機執行之AEM例項的路徑`/content`和`/etc.clientlibs`。 這是讓影像和其他clientlib（不由&#x200B;**ui.frontend**&#x200B;程式碼管理）可用的方式。

   >[!CAUTION]
   >
   > 靜態標籤的影像src指向本機AEM例項上的即時影像元件。 如果影像路徑變更、AEM未啟動，或瀏覽器未登入本機AEM例項，影像就會顯示損毀。 如果切換為外部資源，也可以使用靜態參照來取代影像。

1. 您可以鍵入`CTRL+C`，從命令行&#x200B;**停止** Webpack伺服器。

## Webpack DevServer — 觀看和Aemsync {#webpack-dev-watch}

另一種技術是讓Node.js監看`ui.frontend`模組中src檔案的任何檔案變更。 每當檔案變更，它都會快速編譯用戶端程式庫，並使用[aemsync](https://www.npmjs.com/package/aemsync) npm模組將變更同步至執行中的AEM伺服器。

1. 在&#x200B;**ui.frontend**&#x200B;模組內執行以下命令，從新終端機以&#x200B;**watch**&#x200B;模式啟動Webpack開發伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 這會編譯`src`檔案，並透過[http://localhost:4502](http://localhost:4502)與AEM同步變更

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. 導覽至AEM和LA Skateparks文章：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![部署至AEM的變更](assets/client-side-libraries/changes-deployed-aem-watch.png)

   變更應部署至AEM。 稍有延遲，您必須手動重新整理瀏覽器才能查看更新。 不過，如果您使用新元件和對話方塊編寫，直接在AEM中檢視變更將有所助益。

1. 將更改還原為`_variables.scss`並保存更改。 稍有延遲後，變更應會再次與AEM的本機例項同步。

1. 停止Webpack開發伺服器，並從專案的根目錄執行完整的Maven組建：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   再次， `ui.frontend`模組會編譯、轉換為clientlibrary，並透過`ui.apps`模組部署至AEM。 但這次Maven為我們做一切。

## 恭喜！ {#congratulations}

恭喜， 「文章頁面」現在提供一些符合WKND品牌的一致樣式，而且您已熟悉&#x200B;**ui.frontend**&#x200B;模組！

### 後續步驟{#next-steps}

了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 [使用樣式系統開](style-system.md) 發，涵蓋使用樣式系統，透過品牌專用的CSS和範本編輯器的進階政策設定來擴充核心元件。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git列`tutorial/client-side-libraries-solution`上檢閱並將程式碼部署於本機。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`tutorial/client-side-libraries-solution`分支。

## 其他工具和資源{#additional-resources}

### aemfed &lt;a0/{#develop-aemfed}

[****](https://aemfed.io/) aemfedis是開放原始碼的命令列工具，可用來加速前端開發。它由[aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://www.npmjs.com/package/browser-sync)和[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)提供支援。

在高層級&#x200B;**aemfed**，設計為監聽&#x200B;**ui.apps**&#x200B;模組內的檔案變更，並直接自動同步至執行中的AEM例項。 根據變更，本機瀏覽器會自動重新整理，進而加速前端開發。 此外，此功能也可與Sling Log Tracer搭配使用，以直接在終端中自動顯示任何伺服器端錯誤。

若您在&#x200B;**ui.apps**&#x200B;模組內執行大量工作、修改HTL指令碼並建立自訂元件，**aemfed**&#x200B;可成為功能強大的工具。 [您可以在此處找到完整檔案。](https://github.com/abmaonline/aemfed)

### 調試客戶端庫{#debugging-clientlibs}

使用不同的&#x200B;**類別**&#x200B;和&#x200B;**內嵌**&#x200B;方法來包含多個用戶端程式庫，疑難排解可能會很麻煩。 AEM會公開數種工具，以提供相關協助。 最重要的工具之一是&#x200B;**重建客戶端庫** ，這將強制AEM重新編譯任何LESS檔案並生成CSS。

* [**傾印Lib**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM例項中註冊的所有用戶端程式庫。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**測試輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 可讓使用者根據類別查看clientlib的預期HTML輸出。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**庫相依性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 會反白標示找不到的任何相依性或內嵌類別。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建用戶端程式庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 可讓使用者強制AEM重建所有用戶端程式庫，或使用戶端程式庫的快取無效。此工具在使用LESS進行開發時特別有效，因為這可能會強迫AEM重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理或重建所有程式庫會更有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客戶端庫](assets/client-side-libraries/rebuild-clientlibs.png)
