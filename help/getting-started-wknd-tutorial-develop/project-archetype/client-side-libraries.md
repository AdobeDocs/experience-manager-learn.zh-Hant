---
title: 用戶端程式庫與前端工作流程
description: 了解如何使用用戶端程式庫為 Adobe Experience Manager (AEM) Sites 實施部署和管理 CSS 和 JavaScript。了解如何將 ui.frontend 模組 (一個 webpack 專案) 與端到端建置過程整合。
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
workflow-type: ht
source-wordcount: '2475'
ht-degree: 100%

---

# 用戶端程式庫與前端工作流程 {#client-side-libraries}

了解如何使用用戶端程式庫 (或簡稱 clientlibs) 來部署和管理為 Adobe Experience Manager (AEM) Sites 實施的 CSS 和 JavaScript。此教學課程也介紹 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組，即分離的 [webpack](https://webpack.js.org/) 專案，如何與端到端建置流程整合。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具與指示。

建議也要檢閱[元件基礎知識](component-basics.md#client-side-libraries)教學課程，來了解用戶端程式庫與 AEM 的基礎知識。

### 入門專案

>[!NOTE]
>
> 若已成功完成上一章，您可以重複使用該專案並略過摸索入門專案的步驟。

查看作為本教學課程基礎內容的基準程式碼：

1. 查看來自 [GitHub](https://github.com/adobe/aem-guides-wknd) 的 `tutorial/client-side-libraries-start` 分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用您的 Maven 技能將程式碼基底部署到本機 AEM 實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 若是使用 AEM 6.5 或 6.4，請將 `classic` 設定檔附加到任何 Maven 命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 上檢視完成的程式碼，或透過切換到分支 `tutorial/client-side-libraries-solution` 在本機查看程式碼。

## 目標

1. 了解如何透過可編輯範本將用戶端程式庫包含到頁面上。
1. 了解如何使用 `ui.frontend` 模組以及 webpack 開發伺服器來專門進行前端開發。
1. 了解將編譯後的 CSS 和 JavaScript 傳遞到 Sites 實施的端到端工作流程。

## 您將要建置的內容 {#what-build}

在本章中，您將為 WKND 網站和文章頁面範本新增一些基準樣式，以便讓實施更加符合 [UI 設計模型](assets/pages-templates/wknd-article-design.xd)。您使用進階的前端工作流程，把 webpack 專案與 AEM 用戶端程式庫整合。

![已完成的樣式](assets/client-side-libraries/finished-styles.png)

*已套用基準樣式的文章頁面*

## 背景 {#background}

用戶端程式庫提供一個機制，讓我們能夠組織和管理 AEM Sites 實施所需的 CSS 和 JavaScript 檔案。用戶端程式庫 (或簡稱 clientlibs) 的基本目標是：

1. 把 CSS/JS 儲存為小型的獨立檔案，開發與維護更輕鬆
1. 透過有條理的方式管理第三方框架的相依性
1. 將 CSS/JS 串聯成一個或兩個請求，盡量減少用戶端請求的數量。

如需使用[用戶端程式庫的更多資訊，請參閱這裡。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

用戶端程式庫確實有一些限制。最值得注意的，就是對於常見的前端語言，如 Sass、LESS 和 TypeScript 的支援不多。在教學課程中，我們將了解 **ui.frontend** 模組如何協助解決這個問題。

將入門程式碼基底部署到本機 AEM 實例並導覽至 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。此頁面未設定樣式。我們為 WKND 品牌實施用戶端程式庫，以便將 CSS 和 JavaScript 新增至頁面中。

## 用戶端資料庫的組織 {#organization}

接下來，我們會探索由 [AEM 專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)產生的 clientlibs 的組織。

![用戶端程式庫組織的概要圖](./assets/client-side-libraries/high-level-clientlib-organization.png)

*用戶端程式碼組織與包含頁面的概要圖*

>[!NOTE]
>
> 下列用戶端程式庫組織是由 AEM 專案原型產生的，但是僅代表起點。專案最終如何管理 CSS 和 JavaScript 並傳遞給 Sites 實施，可能會因資源、技能組合和要求不同而有極大差異。

1. 使用 VSCode 或其他 IDE 開啟 **ui.apps** 模組。
1. 展開路徑 `/apps/wknd/clientlibs` 以便檢視由原型產生的 clientlibs。

   ![ui.apps 中的 clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在以下區段中，我們會詳細檢閱這些 clientlibs。

1. 下表是各種用戶端程式庫的摘要。關於[包含用戶端程式庫的更多詳細資訊請參閱這裡。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=zh-hant#developing)

   | 名稱 | 說明 | 備註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 網站運作所需的基本等級 CSS 和 JavaScript | 嵌入核心元件用戶端程式庫 |
   | `clientlib-grid` | 產生讓[版本模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)能夠運作所需的 CSS。 | 可以在這裡設定行動裝置/平板電腦的中斷點 |
   | `clientlib-site` | 包含 WKND 網站的網站特定主題 | 由 `ui.frontend` 模組產生 |
   | `clientlib-dependencies` | 嵌入任何第三方相依性 | 由 `ui.frontend` 模組產生 |

1. 請觀察，原始碼控制已忽略 `clientlib-site` 和 `clientlib-dependencies`。這是刻意的設計，因為這些是在建置時由 `ui.frontend` 模組產生的。

## 更新基礎樣式 {#base-styles}

接下來，更新 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模組中定義的基礎樣式。 `ui.frontend` 模組中的檔案產生 `clientlib-site` 和 `clientlib-dependecies` 程式庫，而這兩個程式庫含有網站主題和任何第三方相依性。

用戶端程式庫不支援更加進階的語言，例如 [Sass](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/)。有數種開放原始碼工具，例如 [NPM](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/) 可加速和最佳化前端開發。**ui.frontend** 模組的目標是能夠使用這些工具來管理大多數前端原始碼檔案。

1. 開啟 **ui.frontend** 模組並導覽至 `src/main/webpack/site`。
1. 開啟檔案 `main.scss`。

   ![main.scss - 進入點](assets/client-side-libraries/main-scss.png)

   `main.scss` 是 `ui.frontend` 模組中 Sass 檔案的進入點。其中包括 `_variables.scss` 檔案，此檔案中包含一系列的品牌變數，可以在專案中用於不同的 Sass 檔案。`_base.scss` 檔案也包含在內，並定義一些 HTML 元素的基本樣式。規則運算式包括 `src/main/webpack/components`下各個元件樣式的樣式。另一個規則運算式包括 `src/main/webpack/site/styles` 下的檔案。

1. 檢查檔案 `main.ts`。其中包括 `main.scss` 和一個規則運算式，用於收集專案中任何 `.js` 或 `.ts` 檔案。[webpack 設定檔案](https://webpack.js.org/configuration/)使用這個進入點作為整個 `ui.frontend` 模組的進入點。

1. 檢查 `src/main/webpack/site/styles` 之下的檔案：

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   這些檔案會對範本中的全域元素 (例如頁首、頁尾和主要內容容器) 設定樣式。這些檔案中的 CSS 規則針對不同的 HTML 元素 `header`、`main` 和 `footer` 設定樣式。這些 HTML 元素由上一章[頁面和範本](./pages-templates.md)中的原則予以定義。

1. 展開 `src/main/webpack` 下的 `components` 資料夾並檢查檔案。

   ![元件 Sass 檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案對應到一個核心元件，例如[摺疊式元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=zh-hant)。每個核心元件均使用 [Block Element Modifier](https://getbem.com/) 或簡稱 BEM 的命名規範來建置，因此更容易選定特定 CSS 類別並套用樣式規則。`/components` 之下的檔案已經由 AEM 專案原型使用每個元件不同的 BEM 規則產生基本架構。

1. 下載 WKND 基礎樣式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 並將檔案&#x200B;**解壓縮**。

   ![WKND 基礎樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為了加速教學課程的進度，我們提供幾個根據核心元件實施 WKND 品牌的 Sass 檔案以及文章頁面範本的結構。

1. 使用上一個步驟中的檔案覆寫 `ui.frontend/src` 的內容。Zip 檔的內容應覆寫以下資料夾：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![已變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   檢查已變更的檔案以查看 WKND 樣式實施的詳細資訊。

## 檢查 ui.frontend 整合 {#ui-frontend-integration}

 **ui.frontend** 模組內建的一個關鍵整合部分 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)，從 webpack/npm 專案中提取已編譯的 CSS 和 JS 成品，並將其轉換為 AEM 用戶端程式庫。

![ui.frontend 架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM 專案原型自動設定此整合。接下來，探索其運作方式。


1. 開啟命令列終端機並使用 `npm install` 命令安裝 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 僅需要執行一次，例如原地複製新專案或產生專案之後。

1. 開啟 `ui.frontend/package.json`，並在 **scripts** **start** 命令中新增 `--env writeToDisk=true`。

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 執行下列命令，以&#x200B;**監聽**&#x200B;模式啟動 webpack dev server：

   ```shell
   $ npm run watch
   ```

1. 這樣會編譯來自 `ui.frontend` 模組的原始碼檔案並將變更同步至位在 [http://localhost:4502](http://localhost:4502) 的 AEM

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

1. 命令 `npm run watch` 最後填入 **ui.apps** 模組中的 **clientlib-site** 和 **clientlib-dependencies**，然後自動與 AEM 同步。

   >[!NOTE]
   >
   >還有一個 `npm run prod` 設定檔可以將 JS 和 CSS 精簡化。每當透過 Maven 觸發 webpack 建置時，這就是標準編譯方式。有關 [ui.frontend 模組的更多詳細資訊請參閱這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. 檢查 `ui.frontend/dist/clientlib-site/site.css` 之下的檔案 `site.css`：這是根據 Sass 原始碼檔案編譯的 CSS。

   ![分散式網站 CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 檢查檔案 `ui.frontend/clientlib.config.js`。這是 npm 外掛程式 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 的設定檔，可以把 `/dist` 的內容轉換成用戶端程式庫並把程式庫移至 `ui.apps` 模組。

1. 檢查 **ui.apps** 模組中的檔案 `site.css`，位在 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`。這應該是與來自 **ui.frontend** 模組的 `site.css` 檔案完全相同的副本。現在這個檔案位在 **ui.apps** 模組中，所以可以部署到 AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 因為 **clientlib-site** 是在建置時間使用 **npm** 或 **maven** 編譯的，所以在 **ui.apps** 模組中不需要把這個檔案納入原始碼控制。檢查 **ui.apps** 之下的 `.gitignore` 檔案。

1. 在 AEM 中開啟洛杉磯滑板場的文章，位於：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![已更新文章的基礎樣式](assets/client-side-libraries/updated-base-styles.png)

   現在您應該看到更新樣式後的文章。您可能必須進行強制重新整理，清除瀏覽器快取的任何 CSS 檔案。

   文章看起來非常接近模型的樣子！

   >[!NOTE]
   >
   > 從專案 `mvn clean install -PautoInstallSinglePackage` 的根目錄觸發 Maven 建置時，上述建置與部署 ui.frontend 程式碼到 AEM 的步驟會自動執行。

## 改變樣式

接下來，對 `ui.frontend` 模組進行小型變更，並檢視 `npm run watch` 自動把樣式部署到本機 AEM 實例。

1. 從 `ui.frontend` 模組開啟檔案：`ui.frontend/src/main/webpack/site/_variables.scss`。
1. 更新 `$brand-primary` 顏色變數：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   儲存變更。

1. 返回瀏覽器並重新整理 AEM 頁面以查看更新：

   ![用戶端程式庫](assets/client-side-libraries/style-update-brand-primary.png)

1. 將變更恢復為 `$brand-primary` 顏色並使用命令 `CTRL+C` 停止 webpack 建置。

>[!CAUTION]
>
> 不是所有專案都需要使用 **ui.frontend** 模組。**ui.frontend** 模組會增加操作的複雜性，如果沒有需要或意願使用一些進階的前端工具 (例如 Sass、webpack、npm…)，則可能不需要使用。

## 頁面與範本的包含操作 {#page-inclusion}

接下來，我們回顧如何在 AEM 頁面中參照 clientlibs。網頁開發有一個常見的最佳做法，就是在關閉 `</body>` 標記之前，在 HTML 標頭 `<head>` 和 JavaScript 中包含 CSS。

1. 瀏覽至文章頁面範本，路徑是：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 按一下「**頁面資訊**」圖示，然後在選單中選取「**頁面原則**」，以便開啟「**頁面原則**」對話框。

   ![文章頁面範本選單頁面原則](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊 > 頁面原則*

1. 請注意，這裡列出了 `wknd.dependencies` 和 `wknd.site` 的類別。在預設情況下，透過頁面原則設定的 clientlibs 會分成兩頭，一頭在頁面標頭包含 CSS，而另一頭在正文末端包含 JavaScript。您可以明確列出要在頁面標頭中載入的 clientlib JavaScript。這是 `wknd.dependencies` 的情況。

   ![文章頁面範本選單頁面原則](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用 `customheaderlibs.html` 或 `customfooterlibs.html` 指令碼，直接從頁面元件參照 `wknd.site` 或 `wknd.dependencies`。使用範本可以享有彈性，因為您可以選擇每個範本使用哪些 clientlib。例如，您有一個只會在特定範本上使用的大型 JavaScript 程式庫。

1. 導覽至使用&#x200B;**文章頁面範本**&#x200B;建立的&#x200B;**洛杉磯滑板場**&#x200B;頁面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 按一下「**頁面資訊**」圖示，然後在選單中選取「**以發佈頁面形式檢視**」，以便在 AEM 編輯器外部開啟文章頁面。

   ![以發佈頁面形式檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 檢視 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 的頁面原始碼，然後應該會在 `<head>` 看到以下 clientlib 參照：

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs 正在使用 Proxy `/etc.clientlibs` 端點。您應該也會看到，在頁面底部包含了以下 clientlib：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 對於 AEM 6.5/6.4，用戶端程式庫不會自動精簡化。請參閱 [HTML 程式庫管理員上的文件以啟用精簡化 (建議)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-hant#using-preprocessors)。

   >[!WARNING]
   >
   >對於發佈端而言，最重要的是用戶端程式庫&#x200B;**不是**&#x200B;由 **/apps** 提供，因為基於安全性理由應該使用 [Dispatcher 篩選器區段](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)限制存取這個路徑。用戶端程式庫的 [allowProxy 屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 可確保 CSS 和 JS 由 **/etc.clientlibs** 提供。

### 後續步驟 {#next-steps}

了解如何使用 Experience Manager 的樣式系統實施個別樣式以及重複使用核心元件。[使用樣式系統進行開發](style-system.md)涵蓋使用樣式系統透過品牌特定的 CSS 和範本編輯器的進階原則設定來擴充核心元件。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上檢視已完成的程式碼，或在 Git 分支 `tutorial/client-side-libraries-solution` 上本機檢閱與部署程式碼。

1. 原地複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和資源 {#additional-resources}

### Webpack DevServer - 靜態標記 {#webpack-dev-static}

在先前的幾個練習中，**ui.frontend** 模組中有幾個 Sass 檔案已經更新，而且透過建置過程，最終看到這些變更反映在 AEM 中。接下來，我們來了解使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 針對&#x200B;**靜態** HTML 快速部署前端樣式的技術。

如果大多數樣式和前端程式碼是由可能無法輕鬆存取 AEM 環境的專門前端開發人員執行，則這項技術非常方便。這項技術也讓 FED 可以直接對 HTML 進行修改，然後將其轉交給 AEM 開發人員作為元件來實施。

1. 複製洛杉磯滑板場文章頁面的頁面原始碼，位於  [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 重新開啟您的 IDE。將從 AEM 複製的標記貼上到 `src/main/webpack/static` 下方 **ui.frontend** 模組中的 `index.html`。
1. 編輯所複製的標記並移除對 **clientlib-site** 和 **clientlib-dependencies** 的任何參照：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   移除這些參照，因為 webpack dev server 會自動產生這些成品。

1. 在 **ui.frontend** 模組內執行以下指令，便能從新終端機啟動 webpack dev server：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在 [http://localhost:8080/](http://localhost:8080/) 開啟一個帶有靜態標記的新瀏覽器視窗。

1. 編輯檔案 `src/main/webpack/site/_variables.scss`。用以下內容取代 `$text-color` 規則：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應該會看到瀏覽器在 [http://localhost:8080](http://localhost:8080) 自動反映相關變更。

   ![本機 webpack dev server 變更](assets/client-side-libraries/local-webpack-dev-server.png)

1. 檢閱 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 檔案。這樣會包含用來啟動 webpack-dev-server 的 webpack 設定。其使用 Proxy 處理來自本機執行的 AEM 實例之路徑 `/content` 和 `/etc.clientlibs`。這就是讓影像和其他 clientlibs (不是由 **ui.frontend** 程式碼管理) 變成可用的方式。

   >[!CAUTION]
   >
   > 靜態標記的影像 src 指向本機 AEM 實例上的即時影像元件。如果影像的路徑變更、AEM 未啟動，或是瀏覽器未登入本機 AEM 實例，則影像可能無法正常顯示。如果轉交給外部資源，也可能使用靜態參照取代影像。

1. 您可以在命令列輸入 `CTRL+C` 來&#x200B;**停止** webpack 伺服器。

### 用戶端程式庫除錯 {#debugging-clientlibs}

使用不同的&#x200B;**分類**&#x200B;方法和&#x200B;**嵌入**&#x200B;方式來包含多個用戶端程式庫，可能會讓故障排除的工作變得繁瑣。AEM 提供多種工具來協助解決這個問題。其中一項最重要的工具是&#x200B;**重新建置用戶端程式庫**，此程式庫會強制 AEM 重新編譯任何 LESS 檔案並產生 CSS。

* [**程式庫傾印**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - 列出在 AEM 實例中註冊的用戶端程式庫。`<host>/libs/granite/ui/content/dumplibs.html`

* [**測試輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 允許使用者根據類別查看包含 clientlib 後的預期 HTML 輸出。`<host>/libs/granite/ui/content/dumplibs.test.html`

* [**程式庫相依性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 醒目標示任何找不到的相依性或嵌入式類別。`<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重新建置用戶端程式庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 允許使用者強制 AEM 重新建置用戶端程式庫或使用戶端程式庫的快取無效。在使用 LESS 開發時，這個工具很有效，因為可以強制 AEM 重新編譯所產生的 CSS。一般來說，相較於重新建置程式庫，讓快取失效然後重新整理頁面是比較有效的做法。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重新建置用戶端程式庫](assets/client-side-libraries/rebuild-clientlibs.png)
