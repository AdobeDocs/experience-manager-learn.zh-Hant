---
title: 使用者端程式庫和前端工作流程
description: 瞭解如何使用使用者端資料庫來針對Adobe Experience Manager (AEM) Sites實作部署及管理CSS和JavaScript。 瞭解如何將ui.frontend模組（一個webpack專案）整合到端對端建置流程中。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 0%

---

# 使用者端程式庫和前端工作流程 {#client-side-libraries}

瞭解如何使用使用者端資料庫或clientlibs來針對Adobe Experience Manager (AEM) Sites實作部署及管理CSS和JavaScript。 此教學課程也涵蓋如何將[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hant)模組（解除耦合的[webpack](https://webpack.js.org/)專案）整合至端對端建置程式。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

也建議檢閱[元件基本知識](component-basics.md#client-side-libraries)教學課程，以瞭解使用者端資料庫和AEM的基礎知識。

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

檢視教學課程建置的基底程式碼：

1. 從[GitHub](https://github.com/adobe/aem-guides-wknd)檢視`tutorial/client-side-libraries-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用您的Maven技能將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`設定檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)上檢視完成的程式碼，或切換至分支`tutorial/client-side-libraries-solution`在本機簽出程式碼。

## 目標

1. 瞭解如何透過可編輯的範本將使用者端程式庫納入頁面中。
1. 瞭解如何使用`ui.frontend`模組和Webpack開發伺服器來進行專屬的前端開發。
1. 瞭解將編譯的CSS和JavaScript傳送至Sites實作的端對端工作流程。

## 您即將建置的內容 {#what-build}

在本章中，您為WKND網站和文章頁面範本新增了一些基準線樣式，讓實作更接近[UI設計模型](assets/pages-templates/wknd-article-design.xd)。 您可使用進階前端工作流程，將webpack專案整合至AEM使用者端程式庫。

![已完成樣式](assets/client-side-libraries/finished-styles.png)

已套用基線樣式的&#x200B;*文章頁面*

## 背景 {#background}

使用者端資料庫提供一種機制，可整理和管理AEM Sites實作所需的CSS和JavaScript檔案。 使用者端程式庫或clientlibs的基本目標為：

1. 將CSS/JS儲存在小型獨立檔案中，以方便開發和維護
1. 以有條理的方式管理對協力廠商架構的相依性
1. 將CSS/JS串連為一或兩個請求，以將使用者端請求的數量降至最低。

如需有關使用[使用者端資料庫的詳細資訊，請參閱此處。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hant)

使用者端程式庫確實有一些限制。 最顯著的是對熱門前端語言（例如Sass、LESS和TypeScript）的有限支援。 在教學課程中，讓我們看看&#x200B;**ui.frontend**&#x200B;模組如何協助解決此問題。

將入門程式碼基底部署至本機AEM執行個體，並導覽至[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此頁面未設定樣式。 讓我們實作WKND品牌的使用者端資料庫，以將CSS和JavaScript新增至頁面。

## 使用者端資料庫組織 {#organization}

接下來，讓我們探索由[AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)產生的clientlibs組織。

![高階clientlibrary組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖表使用者端資料庫組織與頁面包含*

>[!NOTE]
>
> 下列使用者端程式庫組織由AEM Project Archetype產生，但僅代表起點。 專案最終如何管理並向Sites實作CSS和JavaScript，可能會因資源、技能組合和需求而有很大的差異。

1. 使用VSCode或其他IDE開啟&#x200B;**ui.apps**&#x200B;模組。
1. 展開路徑`/apps/wknd/clientlibs`以檢視原型產生的clientlibs。

   ui.apps中的![Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在以下區段中，將檢閱這些clientlibs的更多詳細資訊。

1. 下表摘要列出使用者端程式庫。 如需有關[包含使用者端資料庫的更多詳細資料，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=zh-Hant#developing)。

   | 名稱 | 描述 | 備註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND網站運作所需的CSS和JavaScript基本層級 | 內嵌核心元件使用者端程式庫 |
   | `clientlib-grid` | 產生[配置模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=zh-Hant)運作所需的CSS。 | 行動/平板電腦中斷點可在此處設定 |
   | `clientlib-site` | 包含WKND網站的網站特定主題 | 由`ui.frontend`模組產生 |
   | `clientlib-dependencies` | 內嵌任何第三方相依性 | 由`ui.frontend`模組產生 |

1. 請注意，原始檔控制會忽略`clientlib-site`和`clientlib-dependencies`。 這是特意設計，因為這些是由`ui.frontend`模組在建置時產生。

## 更新基本樣式 {#base-styles}

接著，更新&#x200B;**[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hant)**&#x200B;模組中定義的基本樣式。 `ui.frontend`模組中的檔案會產生包含網站主題及任何第三方相依性的`clientlib-site`及`clientlib-dependecies`資料庫。

使用者端資料庫不支援更進階的語言，例如[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)。 有幾個開放原始碼工具，例如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)，可加速並最佳化前端開發。 **ui.frontend**&#x200B;模組的目標是能夠使用這些工具來管理大多數前端來源檔案。

1. 開啟&#x200B;**ui.frontend**&#x200B;模組並導覽至`src/main/webpack/site`。
1. 開啟檔案`main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss`是`ui.frontend`模組中Sass檔案的進入點。 它包含`_variables.scss`檔案，其中包含一系列要用於專案中不同Sass檔案的品牌變數。 也包含`_base.scss`檔案，並定義HTML元素的一些基本樣式。 規則運算式包含`src/main/webpack/components`下個別元件樣式的樣式。 另一個規則運算式包含`src/main/webpack/site/styles`下的檔案。

1. 檢查檔案`main.ts`。 它包含`main.scss`和規則運算式，以收集專案中的任何`.js`或`.ts`檔案。 [Webpack組態檔](https://webpack.js.org/configuration/)使用此進入點作為整個`ui.frontend`模組的進入點。

1. 檢查`src/main/webpack/site/styles`下的檔案：

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   這些檔案樣式適用於範本中的全域元素，例如「頁首」、「頁尾」和主要內容容器。 這些檔案中的CSS規則以不同的HTML元素`header`、`main`和`footer`為目標。 這些HTML元素是由上一章[頁面和範本](./pages-templates.md)中的原則所定義。

1. 展開`src/main/webpack`下的`components`資料夾並檢查檔案。

   ![元件Sass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都會對應至核心元件，例如[摺疊式功能表元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=zh-Hant)。 每個核心元件是以[區塊元素修飾元](https://getbem.com/)或BEM標籤法建置，以便更輕鬆地以樣式規則鎖定特定CSS類別。 `/components`底下的檔案已由AEM專案原型用每個元件的不同BEM規則進行除錯。

1. 下載WKND基本樣式&#x200B;**[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)**&#x200B;和&#x200B;**解壓縮**&#x200B;檔案。

   ![WKND基底樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為了加速教學課程，我們提供多個Sass檔案，這些檔案會根據核心元件和文章頁面範本的結構來實施WKND品牌。

1. 以先前步驟的檔案覆寫`ui.frontend/src`的內容。 zip的內容應覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![已變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   檢查變更的檔案，以檢視WKND樣式實施的詳細資訊。

## 檢查ui.frontend整合 {#ui-frontend-integration}

內建在&#x200B;**ui.frontend**&#x200B;模組[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)中的重要整合專案，會從webpack/npm專案中取得編譯的CSS和JS成品，並將其轉換成AEM使用者端資料庫。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM專案原型會自動設定這項整合。 接下來，探索其運作方式。


1. 開啟命令列終端機，並使用`npm install`命令安裝&#x200B;**ui.frontend**&#x200B;模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install`執行只需要一次，例如在專案的新複製或產生之後。

1. 開啟`ui.frontend/package.json`並在&#x200B;**指令碼** **開始**&#x200B;命令中新增`--env writeToDisk=true`。

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 執行下列命令，以&#x200B;**watch**&#x200B;模式啟動webpack dev server：

   ```shell
   $ npm run watch
   ```

1. 這會編譯來自`ui.frontend`模組的來源檔案，並在[http://localhost:4502](http://localhost:4502)與AEM同步變更

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

1. 命令`npm run watch`最終會填入&#x200B;**ui.apps**&#x200B;模組中的&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**，然後會自動與AEM同步。

   >[!NOTE]
   >
   >此外還有`npm run prod`個人資料，可縮制JS和CSS。 這是透過Maven觸發Webpack建置時的標準編譯。 如需[ui.frontend模組的詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hant)。

1. 檢查`ui.frontend/dist/clientlib-site/site.css`下方的檔案`site.css`。 這是根據Sass來源檔案編譯的CSS。

   ![分散式網站CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 檢查檔案`ui.frontend/clientlib.config.js`。 這是npm外掛程式[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的設定檔，它會將`/dist`的內容轉換為使用者端程式庫，並將其移至`ui.apps`模組。

1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`的&#x200B;**ui.apps**&#x200B;模組中檢查檔案`site.css`。 這應該是來自&#x200B;**ui.frontend**&#x200B;模組的`site.css`檔案的相同復本。 現在位於&#x200B;**ui.apps**&#x200B;模組中，可將其部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由於&#x200B;**clientlib-site**&#x200B;是在建置期間使用&#x200B;**npm**&#x200B;或&#x200B;**maven**&#x200B;編譯的，因此可以安全地從&#x200B;**ui.apps**&#x200B;模組的原始檔控制中忽略它。 檢查&#x200B;**ui.apps**&#x200B;下方的`.gitignore`檔案。

1. 在AEM開啟LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![已更新文章的基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應該會看到文章的更新樣式。 您可能需要執行硬重新整理，以清除瀏覽器快取的任何CSS檔案。

   現在看起來離模型更近了！

   >[!NOTE]
   >
   > 當從專案`mvn clean install -PautoInstallSinglePackage`的根觸發Maven組建時，會自動執行上述為建置並將ui.frontend程式碼部署到AEM而執行的步驟。

## 進行樣式變更

接下來，在`ui.frontend`模組中進行小幅變更，以檢視`npm run watch`會自動將樣式部署至本機AEM執行個體。

1. 從，`ui.frontend`模組開啟檔案： `ui.frontend/src/main/webpack/site/_variables.scss`。
1. 更新`$brand-primary`色彩變數：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   儲存變更。

1. 返回瀏覽器並重新整理AEM頁面，檢視更新內容：

   ![使用者端資料庫](assets/client-side-libraries/style-update-brand-primary.png)

1. 將變更還原為`$brand-primary`顏色，並使用命令`CTRL+C`停止Webpack建置。

>[!CAUTION]
>
> 並非所有專案都需要使用&#x200B;**ui.frontend**&#x200B;模組。 **ui.frontend**&#x200B;模組增加了額外的複雜性，如果不需要或想要使用某些進階前端工具(Sass、webpack、npm...)，則可能不需要它。

## 頁面和範本包含 {#page-inclusion}

接下來，讓我們檢閱如何在AEM頁面中參照clientlibs。 Web開發的常見最佳實務是在關閉`</body>`標籤之前，將CSS納入HTML標題`<head>`和JavaScript中。

1. 瀏覽至[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)上的文章頁面範本

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示，然後在功能表中選取&#x200B;**頁面原則**&#x200B;以開啟&#x200B;**頁面原則**&#x200B;對話方塊。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面原則*

1. 請注意，`wknd.dependencies`和`wknd.site`的類別已在此列出。 依預設，透過頁面原則設定的clientlibs會進行分割，在頁面標頭中包含CSS，並在內文結尾包含JavaScript。 您可以明確列出要載入頁面標題的clientlib JavaScript。 這是`wknd.dependencies`的情況。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用`customheaderlibs.html`或`customfooterlibs.html`指令碼，直接從頁面元件參考`wknd.site`或`wknd.dependencies`。 使用「範本」可提供彈性，讓您可以挑選並選擇每個範本要使用的clientlibs。 例如，如果您有一個只會在選取範本中使用的大型JavaScript資料庫。

1. 導覽至使用&#x200B;**文章頁面範本**&#x200B;建立的&#x200B;**LA滑板場**&#x200B;頁面： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示，然後在功能表中選取&#x200B;**以發佈的形式檢視**，以開啟AEM編輯器外部的文章頁面。

   ![以發佈的形式檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 檢視[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的頁面來源，您應該可以在`<head>`中看到下列clientlib參考：

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs正在使用Proxy `/etc.clientlibs`端點。 您也應該會看到下列clientlib包含在頁面底部：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 若為AEM 6.5/6.4，使用者端程式庫不會自動縮制。 請參閱有關[HTML資料庫管理員的檔案以啟用縮制（建議）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hant#using-preprocessors)。

   >[!WARNING]
   >
   >發佈端很重要的一點是，使用者端程式庫&#x200B;**不是**/應用程式&#x200B;**提供的**，因為基於安全性理由，應使用[Dispatcher篩選區段](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#example-filter-section)來限制此路徑。 使用者端程式庫的[allowProxy屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hant#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)可確保從&#x200B;**/etc.clientlibs**&#x200B;提供CSS和JS。

### 後續步驟 {#next-steps}

瞭解如何使用Experience Manager的樣式系統來實作個別樣式並重複使用核心元件。 [使用樣式系統進行開發](style-system.md)涵蓋使用樣式系統，以使用範本編輯器的品牌特定CSS和進階原則設定來擴充核心元件。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/client-side-libraries-solution`上檢閱並部署本機的程式碼。

1. 複製[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存放庫。
1. 檢視`tutorial/client-side-libraries-solution`分支。

## 其他工具和資源 {#additional-resources}

### Webpack DevServer — 靜態標籤 {#webpack-dev-static}

在前幾個練習中，**ui.frontend**&#x200B;模組中的多個Sass檔案已更新，並透過建置程式最終看到這些變更反映在AEM中。 接下來，讓我們看一下使用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)快速開發針對&#x200B;**static** HTML的前端樣式的技巧。

如果大多數樣式和前端計畫碼是由無法輕鬆存取AEM環境的專屬前端開發人員執行，這項技術會很實用。 此技術也允許FED直接修改HTML，然後交由AEM開發人員實作為元件。

1. 複製位於[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的LA滑板公園文章頁面的頁面來源。
1. 重新開啟IDE。 從AEM複製的標籤貼到`src/main/webpack/static`下方的&#x200B;**ui.frontend**&#x200B;模組中的`index.html`。
1. 編輯複製的標籤，並移除對&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**&#x200B;的任何參考：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   移除這些參考，因為webpack開發伺服器會自動產生這些成品。

1. 從&#x200B;**ui.frontend**&#x200B;模組內執行下列命令，從新的終端機啟動webpack開發伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在[http://localhost:8080/](http://localhost:8080/)以靜態標籤開啟新的瀏覽器視窗。

1. 編輯檔案`src/main/webpack/site/_variables.scss`檔案。 將`$text-color`規則取代為下列內容：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應該會自動看到變更在[http://localhost:8080](http://localhost:8080)上的瀏覽器中自動反映。

   ![本機Webpack開發伺服器變更](assets/client-side-libraries/local-webpack-dev-server.png)

1. 檢閱`/aem-guides-wknd.ui.frontend/webpack.dev.js`檔案。 這包含用來啟動webpack-dev-server的webpack設定。 它從本機執行的AEM執行個體代理路徑`/content`和`/etc.clientlibs`。 這是影像和其他clientlibs （不是由&#x200B;**ui.frontend**&#x200B;程式碼管理）可供使用的方式。

   >[!CAUTION]
   >
   > 靜態標籤的影像src指向本機AEM例項上的即時影像元件。 如果影像的路徑變更、AEM未啟動，或瀏覽器未登入本機AEM執行個體，影像會顯示為已損毀。 如果移交給外部資源，也可以使用靜態參照取代影像。

1. 您可以輸入`CTRL+C`，從命令列&#x200B;**停止** Webpack伺服器。

### 偵錯使用者端程式庫 {#debugging-clientlibs}

使用不同的&#x200B;**類別**&#x200B;和&#x200B;**內嵌**&#x200B;方法，以包含多個使用者端程式庫，可能很難進行疑難排解。 AEM提供數種工具可協助解決此問題。 最重要的工具之一是&#x200B;**重建使用者端資料庫**，它會強制AEM重新編譯任何LESS檔案並產生CSS。

* [**傾印程式庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) — 列出在AEM執行個體中註冊的使用者端程式庫。`<host>/libs/granite/ui/content/dumplibs.html`

* [**測試輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) — 允許使用者根據類別檢視clientlib include的預期HTML輸出。`<host>/libs/granite/ui/content/dumplibs.test.html`

* [**資料庫相依性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) — 反白標示找不到的任何相依性或內嵌類別。`<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建使用者端資料庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) — 允許使用者強制AEM重建使用者端資料庫或使使用者端資料庫的快取失效。 使用LESS開發時，此工具相當有效，因為這會強制AEM重新編譯產生的CSS。 一般而言，讓快取失效，然後執行頁面重新整理比重新建置程式庫更有效率。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重新建置使用者端程式庫](assets/client-side-libraries/rebuild-clientlibs.png)
