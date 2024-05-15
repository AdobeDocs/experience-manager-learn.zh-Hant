---
title: 使用者端程式庫和前端工作流程
description: 瞭解如何使用使用者端資料庫來針對Adobe Experience Manager (AEM) Sites實作部署及管理CSS和JavaScript。 瞭解如何將ui.frontend模組（一個webpack專案）整合到端對端建置流程中。
version: 6.4, 6.5, Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# 使用者端程式庫和前端工作流程 {#client-side-libraries}

瞭解如何使用使用者端資料庫或clientlibs來針對Adobe Experience Manager (AEM) Sites實作部署及管理CSS和JavaScript。 本教學課程也會說明 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組，分離式 [webpack](https://webpack.js.org/) 專案，可整合至端對端建置流程。

## 先決條件 {#prerequisites}

檢閱設定所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

也建議檢閱 [元件基本知識](component-basics.md#client-side-libraries) 教學課程以瞭解使用者端程式庫和AEM的基礎。

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

檢視教學課程建置的基底程式碼：

1. 檢視 `tutorial/client-side-libraries-start` 分支來源 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven指令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以於檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 或切換至分支以在本機簽出程式碼 `tutorial/client-side-libraries-solution`.

## 目標

1. 瞭解如何透過可編輯的範本將使用者端程式庫納入頁面中。
1. 瞭解如何使用 `ui.frontend` 模組和webpack開發伺服器，用於專屬的前端開發。
1. 瞭解將編譯的CSS和JavaScript傳遞至Sites實作的端對端工作流程。

## 您即將建置的內容 {#what-build}

在本章中，您為WKND網站和文章頁面範本新增一些基準樣式，以將實施更接近於 [UI設計模型](assets/pages-templates/wknd-article-design.xd). 您可使用進階前端工作流程，將webpack專案整合至AEM使用者端程式庫。

![完成的樣式](assets/client-side-libraries/finished-styles.png)

*套用基線樣式的文章頁面*

## 背景 {#background}

使用者端資料庫提供一種機制，可整理和管理AEM Sites實施所需的CSS和JavaScript檔案。 使用者端程式庫或clientlibs的基本目標為：

1. 將CSS/JS儲存在小型獨立檔案中，以方便開發和維護
1. 以有條理的方式管理對協力廠商架構的相依性
1. 將CSS/JS串連為一或兩個請求，以將使用者端請求的數量降至最低。

更多關於使用的資訊 [您可以在此處找到使用者端資料庫。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

使用者端程式庫確實有一些限制。 最顯著的是對熱門前端語言（例如Sass、LESS和TypeScript）的有限支援。 在教學課程中，讓我們來看一下 **ui.frontend** 模組有助於解決這個問題。

將入門程式碼基底部署至本機AEM執行個體，並導覽至 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 此頁面未設定樣式。 讓我們實作WKND品牌的使用者端資料庫，以將CSS和JavaScript新增至頁面。

## 使用者端資料庫組織 {#organization}

接下來，讓我們探索產生的clientlibs組織。 [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![高階clientlibrary組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖表使用者端資料庫組織和頁面包含*

>[!NOTE]
>
> 以下使用者端程式庫組織由AEM Project Archetype產生，但僅代表起點。 專案最終如何管理並向Sites實作CSS和JavaScript，可能會因資源、技能組合和需求而有很大的差異。

1. 使用VSCode或其他IDE開啟 **ui.apps** 模組。
1. 展開路徑 `/apps/wknd/clientlibs` 以檢視原型產生的clientlibs。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在以下區段中，將檢閱這些clientlibs的更多詳細資訊。

1. 下表摘要列出使用者端程式庫。 更多關於以下內容的詳細資訊： [包含使用者端資料庫如下所述](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 名稱 | 說明 | 附註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND網站運作所需的CSS和JavaScript基本層級 | 內嵌核心元件使用者端程式庫 |
   | `clientlib-grid` | 產生所需的CSS [版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 才能運作。 | 行動/平板電腦中斷點可在此處設定 |
   | `clientlib-site` | 包含WKND網站的網站特定主題 | 產生者： `ui.frontend` 模組 |
   | `clientlib-dependencies` | 內嵌任何第三方相依性 | 產生者： `ui.frontend` 模組 |

1. 請留意 `clientlib-site` 和 `clientlib-dependencies` 會從原始檔控制中忽略。 這是刻意設計，因為這些是在建置時由 `ui.frontend` 模組。

## 更新基本樣式 {#base-styles}

接下來，更新在 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模組。 中的檔案 `ui.frontend` 模組產生 `clientlib-site` 和 `clientlib-dependecies` 包含網站主題及任何第三方相依性的程式庫。

使用者端資料庫不支援更進階的語言，例如 [Sas](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/). 有多種開放原始碼工具，例如 [npm](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/) 加速並最佳化前端開發。 的目標 **ui.frontend** 模組能夠使用這些工具來管理大部分的前端來源檔案。

1. 開啟 **ui.frontend** 模組並導覽至 `src/main/webpack/site`.
1. 開啟檔案 `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` 是中Sass檔案的進入點 `ui.frontend` 模組。 其中包括 `_variables.scss` 檔案，其中包含要用於專案中不同Sass檔案的一系列品牌變數。 此 `_base.scss` 也會包含檔案，並定義HTML元素的一些基本樣式。 規則運算式包含「 」下個別元件樣式的樣式 `src/main/webpack/components`. 另一個規則運算式包含 `src/main/webpack/site/styles`.

1. Inspect檔案 `main.ts`. 內容包括 `main.scss` 和規則運算式，以收集任何 `.js` 或 `.ts` 檔案時。 此進入點由 [webpack組態檔](https://webpack.js.org/configuration/) 作為整個的進入點 `ui.frontend` 模組。

1. Inspect底下的檔案 `src/main/webpack/site/styles`：

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   這些檔案樣式適用於範本中的全域元素，例如「頁首」、「頁尾」和主要內容容器。 這些檔案中的CSS規則以不同的HTML元素為目標 `header`， `main`、和  `footer`. 這些HTML元素是由上一章中的原則所定義 [頁面和範本](./pages-templates.md).

1. 展開 `components` 資料夾位於 `src/main/webpack` 並檢查檔案。

   ![元件Ass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都會對應至核心元件，例如 [收合式選單元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). 每個核心元件都是透過建置 [區塊元素修飾元](https://getbem.com/) 或BEM標籤法，讓樣式規則更容易鎖定特定的CSS類別。 底下的檔案 `/components` 已由AEM Project原型用每個元件的不同BEM規則進行除錯。

1. 下載WKND基本樣式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 和 **unzip** 檔案。

   ![WKND基本樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為了加速教學課程，我們提供多個Sass檔案，這些檔案會根據核心元件和文章頁面範本的結構來實施WKND品牌。

1. 覆寫的內容 `ui.frontend/src` 包含上一步驟中的檔案。 zip的內容應覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect變更的檔案，以檢視WKND樣式實施的詳細資訊。

## Inspect ui.frontend整合 {#ui-frontend-integration}

內建於中的重要整合元件 **ui.frontend** 模組， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 從webpack/npm專案取得編譯的CSS和JS成品，並將其轉換成AEM使用者端資料庫。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM專案原型會自動設定此整合。 接下來，探索其運作方式。


1. 開啟命令列終端機並安裝 **ui.frontend** 模組使用 `npm install` 命令：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需要執行一次，例如在專案的新複製或產生之後。

1. 在中啟動webpack開發伺服器 **觀看** 模式，方法是執行下列命令：

   ```shell
   $ npm run watch
   ```

1. 這會編譯來源檔案 `ui.frontend` 模組並與AEM同步變更，位於 [http://localhost:4502](http://localhost:4502)

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

1. 指令 `npm run watch` 最終會填入 **clientlib-site** 和 **clientlib-dependencies** 在 **ui.apps** 然後會自動與AEM同步的模組。

   >[!NOTE]
   >
   >此外， `npm run prod` 會縮制JS和CSS的設定檔。 這是透過Maven觸發Webpack建置時的標準編譯。 關於的更多詳細資料 [ui.frontend模組可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect檔案 `site.css` 下 `ui.frontend/dist/clientlib-site/site.css`. 這是根據Sass來源檔案編譯的CSS。

   ![分散式網站CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect檔案 `ui.frontend/clientlib.config.js`. 這是npm外掛程式的設定檔， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 會轉換以下專案的內容： `/dist` 移至使用者端程式庫，並將其移至 `ui.apps` 模組。

1. Inspect檔案 `site.css` 在 **ui.apps** 模組在 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 這應該是 `site.css` 來自的檔案 **ui.frontend** 模組。 現在它已進入 **ui.apps** 模組；可部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 從 **clientlib-site** 會在建置期間編譯，使用 **npm**，或 **maven**&#x200B;中，您可以從原始檔控制安全地忽略它 **ui.apps** 模組。 Inspect `.gitignore` 下的檔案 **ui.apps**.

1. 在AEM開啟LA Skatepark文章，網址為： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![更新文章的基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應該會看到文章的更新樣式。 您可能需要執行硬重新整理，以清除瀏覽器快取的任何CSS檔案。

   現在看起來離模型更近了！

   >[!NOTE]
   >
   > 當從專案的根觸發Maven組建時，會自動執行上述針對建置和部署ui.frontend程式碼到AEM所執行的步驟 `mvn clean install -PautoInstallSinglePackage`.

## 進行樣式變更

接下來，在 `ui.frontend` 模組以檢視 `npm run watch` 自動將樣式部署至本機AEM執行個體。

1. 從， `ui.frontend` 模組開啟檔案： `ui.frontend/src/main/webpack/site/_variables.scss`.
1. 更新 `$brand-primary` 顏色變數：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   儲存變更。

1. 返回瀏覽器並重新整理AEM頁面，檢視更新內容：

   ![使用者端資源庫](assets/client-side-libraries/style-update-brand-primary.png)

1. 將變更恢復為 `$brand-primary` 使用命令設定顏色並停止webpack建置 `CTRL+C`.

>[!CAUTION]
>
> 使用 **ui.frontend** 並非所有專案都需要使用模組。 此 **ui.frontend** 模組增加了額外的複雜性，如果不需要或想要使用某些進階前端工具(Sass、webpack、npm...)，則可能不需要使用。

## 頁面和範本包含 {#page-inclusion}

接下來，讓我們檢閱在AEM頁面中參照clientlibs的方式。 Web開發的常見最佳實務是在HTML標題中包含CSS `<head>` 和JavaScript，緊接著關閉 `</body>` 標籤之間。

1. 瀏覽至文章頁面範本，網址為 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 按一下 **頁面資訊** 圖示，然後在功能表中選取 **頁面原則** 以開啟 **頁面原則** 對話方塊。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面原則*

1. 請注意， `wknd.dependencies` 和 `wknd.site` 此處列出。 依預設，透過頁面原則設定的clientlibs會分割以在頁面標頭中包含CSS並在內文結尾包含JavaScript。 您可以明確列出要載入頁面標題中的clientlib JavaScript。 以下專案即是如此 `wknd.dependencies`.

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 您也可以參照 `wknd.site` 或 `wknd.dependencies` 直接從頁面元件，使用 `customheaderlibs.html` 或 `customfooterlibs.html` 指令碼。 使用「範本」可提供彈性，讓您可以挑選並選擇每個範本要使用的clientlibs。 例如，如果您有一個只會在選取範本中使用的大型JavaScript程式庫。

1. 導覽至 **LA滑板場** 使用建立的頁面 **文章頁面範本**： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 按一下 **頁面資訊** 圖示，然後在功能表中選取 **以發佈狀態檢視** 以在AEM編輯器外部開啟文章頁面。

   ![以發佈的形式檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 檢視頁面來源： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 而且您應該可以在「 」檢視下列clientlib參照 `<head>`：

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs正在使用Proxy `/etc.clientlibs` 端點。 您也應該會看到下列clientlib包含在頁面底部：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 對於AEM 6.5/6.4，使用者端程式庫不會自動縮制。 請參閱相關的檔案： [HTML庫管理員以啟用縮制（建議）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >在發佈端，使用者端程式庫非常重要 **非** 服務來源 **/apps** 因為基於安全理由，應該使用 [Dispatcher篩選區段](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 此 [allowProxy屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 的，以確保從提供CSS和JS **/etc.clientlibs**.

### 後續步驟 {#next-steps}

瞭解如何使用Experience Manager的樣式系統來實作個別樣式並重複使用核心元件。 [使用樣式系統進行開發](style-system.md) 涵蓋使用樣式系統，以透過範本編輯器的品牌特定CSS和進階原則設定來擴充核心元件。

檢視完成的程式碼： [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上檢閱並部署程式碼至本機 `tutorial/client-side-libraries-solution`.

1. 原地複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 檢視 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和資源 {#additional-resources}

### Webpack DevServer — 靜態標籤 {#webpack-dev-static}

在前幾個練習中，中有數個Sass檔案 **ui.frontend** 模組已更新，並透過建置流程最終看到這些變更反映在AEM中。 接下來，讓我們來看一下使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 快速開發前端樣式，並與 **靜態** HTML。

如果大多數樣式和前端計畫碼是由無法輕鬆存取AEM環境的專屬前端開發人員執行，這項技術會很實用。 此技術也允許FED直接修改HTML，然後可以交由AEM開發人員實作為元件。

1. 複製LA滑板公園文章頁面的頁面來源，位於 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. 重新開啟IDE。 將複製的AEM標籤貼到 `index.html` 在 **ui.frontend** 下方模組 `src/main/webpack/static`.
1. 編輯複製的標示並移除對的參照 **clientlib-site** 和 **clientlib-dependencies**：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   移除這些參考，因為webpack開發伺服器會自動產生這些成品。

1. 從新的終端機啟動webpack開發伺服器，方法是從 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在開啟新的瀏覽器視窗 [http://localhost:8080/](http://localhost:8080/) 使用靜態標籤。

1. 編輯檔案 `src/main/webpack/site/_variables.scss` 檔案。 取代 `$text-color` 規則中指定下列專案：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應該會自動看見變更，並在以下日期自動反映在瀏覽器中： [http://localhost:8080](http://localhost:8080).

   ![本機Webpack開發伺服器變更](assets/client-side-libraries/local-webpack-dev-server.png)

1. 檢閱 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 檔案。 這包含用來啟動webpack-dev-server的webpack設定。 它會代理路徑 `/content` 和 `/etc.clientlibs` 從本機執行中的AEM執行個體。 這就是影像和其他clientlibs （並非由管理）的方式。 **ui.frontend** 程式碼)的欄位都會開放使用。

   >[!CAUTION]
   >
   > 靜態標籤的影像src指向本機AEM例項上的即時影像元件。 如果影像的路徑變更、AEM未啟動或瀏覽器未登入本機AEM執行個體，影像會顯示為已損毀。 如果移交給外部資源，也可以使用靜態參照取代影像。

1. 您可以 **停止** 從命令列輸入webpack伺服器 `CTRL+C`.

### 已封存 {#develop-aemfed}

**[已封存](https://aemfed.io/)** 是一個開放原始碼的命令列工具，可用來加速前端開發。 其提供者： [aemsync](https://www.npmjs.com/package/aemsync)， [Browsersync](https://browsersync.io/)、和 [Sling記錄追蹤器](https://sling.apache.org/documentation/bundles/log-tracers.html).

從高層面來看， `aemfed`設計用來監聽 **ui.apps** 模組，並自動將其直接同步至執行中的AEM執行個體。 本機瀏覽器會根據變更自動重新整理，從而加速前端開發。 它也設計為與Sling記錄追蹤器搭配使用，以直接在終端機中自動顯示任何伺服器端錯誤。

如果您在「 」內做了許多工作 **ui.apps** 模組、修改HTL指令碼以及建立自訂元件 **已封存** 可能是強大的工具。 [如需完整檔案，請參閱此處](https://github.com/abmaonline/aemfed).

### 偵錯使用者端程式庫 {#debugging-clientlibs}

使用不同的方法 **類別** 和 **內嵌** 若要包含多個使用者端程式庫，進行疑難排解會相當麻煩。 AEM公開了幾個工具來幫助解決此問題。 最重要的工具之一是 **重新建置使用者端資料庫** 會強制AEM重新編譯任何LESS檔案並產生CSS。

* [**傾印程式庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM執行個體中註冊的使用者端程式庫。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**測試輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 可讓使用者根據類別檢視clientlib include的預期HTML輸出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**程式庫相依性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 反白標示任何找不到的相依性或內嵌類別。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重新建置使用者端資料庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 可讓使用者強制AEM重建使用者端程式庫或讓使用者端程式庫的快取失效。 使用LESS開發時，此工具相當有效，因為這會強制AEM重新編譯產生的CSS。 一般而言，讓快取失效，然後執行頁面重新整理比重新建置程式庫更有效率。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建使用者端程式庫](assets/client-side-libraries/rebuild-clientlibs.png)
