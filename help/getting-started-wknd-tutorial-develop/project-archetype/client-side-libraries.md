---
title: 用戶端程式庫和前端工作流程
description: 了解如何使用Client資料庫或clientlibs來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 了解如何將ui.frontend模組（Webpack專案）整合至端對端建置程式。
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2825'
ht-degree: 1%

---

# 用戶端程式庫和前端工作流程 {#client-side-libraries}

了解如何使用用戶端資料庫或clientlib來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和JavaScript。 本教學課程也將說明 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組，去耦 [webpack](https://webpack.js.org/) 可整合至端對端建置程式中。

## 必備條件 {#prerequisites}

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

建議您檢閱 [元件基本知識](component-basics.md#client-side-libraries) 了解用戶端程式庫和AEM基礎知識的教學課程。

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重複使用項目，並跳過簽出起始項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看 `tutorial/client-side-libraries-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，請附加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 或切換到分支以本地簽出代碼 `tutorial/client-side-libraries-solution`.

## 目標

1. 了解透過可編輯的範本將用戶端程式庫納入頁面的方式。
1. 了解如何使用UI.Frontend模組和Web Pack開發伺服器進行專用的前端開發。
1. 了解將編譯的CSS和JavaScript傳遞至Sites實作的端對端工作流程。

## 您將建置的 {#what-you-will-build}

在本章中，您為WKND網站和文章頁面範本新增了一些基線樣式，讓實作更接近 [UI設計模型](assets/pages-templates/wknd-article-design.xd). 您可以使用進階前端工作流程，將Webpack專案整合至AEM用戶端程式庫。

![已完成樣式](assets/client-side-libraries/finished-styles.png)

*已套用基線樣式的文章頁面*

## 背景 {#background}

用戶端資料庫提供組織及管理AEM Sites實作所需CSS和JavaScript檔案的機制。 用戶端程式庫或clientlib的基本目標為：

1. 將CSS/JS儲存在小型獨立檔案中，以便進行開發和維護
1. 以有組織的方式管理對協力廠商架構的依賴
1. 將CSS/JS串連至一或兩個請求，將用戶端請求數減到最少。

使用的詳細資訊 [您可以在此處找到用戶端程式庫。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

用戶端程式庫確實有一些限制。 最引人注目的是對熱門前端語言（如Sass、LESS和TypeScript）的有限支援。 在教學課程中，我們將探討 **ui.frontend** 模組可以幫助解決此問題。

將入門程式碼基底部署至本機AEM執行個體，並導覽至 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 此頁面未設定樣式。 讓我們實作WKND品牌的用戶端資料庫，以新增CSS和JavaScript至頁面。

## 用戶端程式庫組織 {#organization}

接下來，我們將探索 [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![高層級客戶程式庫組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖表用戶端程式庫組織與頁面包含*

>[!NOTE]
>
> 下列用戶端程式庫組織是由AEM專案原型產生，但僅代表一個起點。 專案最終如何管理CSS和JavaScript以供Sites實作之用，可能會因資源、技能集和需求而大不相同。

1. 使用VSCode或其他IDE開啟 **ui.apps** 模組。
1. 展開路徑 `/apps/wknd/clientlibs` 檢視原型產生的clientlib。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   我們會在下方更詳細地檢查這些clientlib。

1. 下表匯總了客戶端庫。 有關的更多詳細資訊 [包括客戶端庫，可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 名稱 | 說明 | 附註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND網站運作所需的CSS和JavaScript基本層級 | 內嵌核心元件用戶端LIB |
   | `clientlib-grid` | 產生 [版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 工作。 | 您可在此處設定行動裝置/平板電腦分界點 |
   | `clientlib-site` | 包含WKND站點的站點特定主題 | 由 `ui.frontend` 模組 |
   | `clientlib-dependencies` | 內嵌任何第三方相依性 | 由 `ui.frontend` 模組 |

1. 請注意 `clientlib-site` 和 `clientlib-dependencies` 會從原始碼控制項中忽略。 這是根據設計，因為這些是在建置時由 `ui.frontend` 模組。

## 更新基本樣式 {#base-styles}

接下來，更新 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模組。 中的檔案 `ui.frontend` 模組產生 `clientlib-site` 和 `clientlib-dependecies` 包含網站主題和任何協力廠商相依性的程式庫。

用戶端程式庫不支援更進階的語言，例如 [薩斯](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/). 有數種開放原始碼工具，例如 [NPM](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/) 加速並優化前端開發。 目標 **ui.frontend** 模組將能夠使用這些工具來管理大多數前端源檔案。

1. 開啟 **ui.frontend** 模組，並導覽至 `src/main/webpack/site`.
1. 開啟檔案 `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
用戶端資料庫/main-scss

   `main.scss` 是 `ui.frontend` 模組。 其中包括 `_variables.scss` 檔案，其中包含要用於專案中不同Sass檔案的一系列品牌變數。 此 `_base.scss` 也包含檔案，並定義HTML元素的一些基本樣式。 規則運算式包含下方個別元件樣式的樣式 `src/main/webpack/components`. 另一個規則運算式包含 `src/main/webpack/site/styles`.

1. Inspect檔案 `main.ts`. 包括 `main.scss` 和規則運算式來收集 `.js` 或 `.ts` 檔案。 此入口點由 [webpack組態檔](https://webpack.js.org/configuration/) 作為整個 `ui.frontend` 模組。

1. Inspect檔案 `src/main/webpack/site/styles`:

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   範本中全域元素的這些檔案樣式，例如「頁首」、「頁尾」和主要內容容器。 這些檔案中的CSS規則會鎖定不同的HTML元素 `header`, `main`，和  `footer`. 這些HTML元素由上一章中的策略定義 [頁面和範本](./pages-templates.md).

1. 展開 `components` 檔案夾 `src/main/webpack` 檢查檔案。

   ![元件Sass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都對應至核心元件，如 [折疊式面板元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). 每個核心元件都以 [區塊元素修飾詞](https://getbem.com/) 或BEM記號，讓您更容易使用樣式規則來鎖定特定CSS類別。 下面的檔案 `/components` AEM專案原型已針對每個元件使用不同的BEM規則來模擬。

1. 下載WKND基本樣式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 和 **unzip** 檔案。

   ![WKND基本樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為加速本教學課程，我們提供數個Sass檔案，這些檔案會根據核心元件和文章頁面範本結構來實作WKND品牌。

1. 覆寫 `ui.frontend/src` 的檔案。 zip的內容應會覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect已變更的檔案，以查看WKND樣式實作的詳細資訊。

## Inspect ui.frontend整合 {#ui-frontend-integration}

內建於 **ui.frontend** 模組， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 從webpack/npm專案擷取已編譯的CSS和JS成品，並將其轉換為AEM用戶端資料庫。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM專案原型會自動設定此整合。 接下來，探索其運作方式。


1. 開啟命令列終端並安裝 **ui.frontend** 模組使用 `npm install` 命令：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需執行一次，即可執行新克隆或生成項目。

1. 在中啟動Webpack開發伺服器 **watch** 模式，方法是：

   ```shell
   $ npm run watch
   ```

1. 這會編譯 `src` 檔案 `ui.frontend` 模組，並與AEM at同步變更 [http://localhost:4502](http://localhost:4502)

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

1. 命令 `npm run watch` 最終填入 **clientlib-site** 和 **clientlib-dependencies** 在 **ui.apps** 模組，然後會自動與AEM同步。

   >[!NOTE]
   >
   >此外， `npm run prod` 可縮制JS和CSS的設定檔。 這是每當透過Maven觸發Webpack組建時的標準編譯。 有關 [ui.frontend模組可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect檔案 `site.css` 在 `ui.frontend/dist/clientlib-site/site.css`. 這是根據Sass源檔案編譯的CSS。

   ![分佈式網站CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect檔案 `ui.frontend/clientlib.config.js`. 這是npm外掛程式的設定檔案， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 來轉換 `/dist` 移入用戶端程式庫，並將其移至 `ui.apps` 模組。

1. Inspect檔案 `site.css` 在 **ui.apps** 模組於 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 這應是 `site.css` 檔案 **ui.frontend** 模組。 現在它已進入 **ui.apps** 模組，可部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 自 **clientlib-site** 在建置期間編譯，使用 **npm**，或 **馬文**，則可安全地從來源控制中忽略 **ui.apps** 模組。 Inspect `.gitignore` 檔案下方 **ui.apps**.

1. 在AEM中，開啟LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![更新文章的基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應該會看到文章的更新樣式。 您可能需要執行硬式重新整理，才能清除瀏覽器快取的任何CSS檔案。

   它越來越接近模型了！

   >[!NOTE]
   >
   > 從專案根目錄觸發Maven組建時，系統會自動執行上述步驟，以建置ui.frontend程式碼並部署至AEM `mvn clean install -PautoInstallSinglePackage`.

## 更改樣式

接下來，在 `ui.frontend` 模組以檢視 `npm run watch` 自動將樣式部署至本機AEM執行個體。

1. 在 `ui.frontend` 模組會開啟檔案： `ui.frontend/src/main/webpack/site/_variables.scss`.
1. 更新 `$brand-primary` 顏色變數：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   儲存變更。

1. 返回瀏覽器並重新整理AEM頁面以查看更新：

   ![用戶端程式庫](assets/client-side-libraries/style-update-brand-primary.png)

1. 將變更還原為 `$brand-primary` 使用命令對webpack生成進行顏色和停止 `CTRL+C`.

>[!CAUTION]
>
> 使用 **ui.frontend** 模組不一定適用於所有專案。 此 **ui.frontend** 模組會增加額外的複雜性，而如果不需要/不想使用這些進階前端工具(Sass、webpack、npm...)，則可能不需要。

## 頁面和範本包含 {#page-inclusion}

接下來，我們來檢閱AEM頁面中如何參考clientlib。 網頁開發的常見最佳作法是在HTML標題中包含CSS `<head>` 和JavaScript，然後關閉 `</body>` 標籤。

1. 瀏覽至「文章頁面」範本(位於 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 按一下 **頁面資訊** 表徵圖和在菜單中選擇 **頁面原則** 開啟 **頁面原則** 對話框。

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面原則*

1. 請注意， `wknd.dependencies` 和 `wknd.site` 列出。 依預設，透過「頁面原則」設定的clientlib會被分割，以在頁面標題中包含CSS，並在內文結尾處包含JavaScript。 如有需要，您可明確列出要在頁面標題中載入clientlib JavaScript。 這是 `wknd.dependencies`.

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 您也可以參考 `wknd.site` 或 `wknd.dependencies` 直接從頁面元件，使用 `customheaderlibs.html` 或 `customfooterlibs.html` 劇本，正如我們之前看到的 `wknd.base` clientlib 。 使用範本可提供一些彈性，您可以挑選並選擇每個範本使用的clientlib。 例如，如果您有大量JavaScript程式庫，該程式庫只會用於選取的範本。

1. 導覽至 **拉斯克特帕克斯** 頁面，使用 **文章頁面範本**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 按一下 **頁面資訊** 表徵圖和在菜單中選擇 **檢視為已發佈** 在AEM編輯器外部開啟文章頁面。

   ![以已發佈狀態檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 檢視的頁面來源 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 而您應可在 `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，clientlibs使用Proxy `/etc.clientlibs` 端點。 您也應該會看到下列clientlib包含在頁面底部：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 如果在6.5/6.4上執行後續操作，用戶端程式庫將不會自動縮制。 請參閱 [HTML程式庫管理員啟用縮制（建議）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >在發佈端，用戶端程式庫是否重要 **not** 提供 **/apps** 因為此路徑應因安全原因而限制，請使用 [Dispatcher篩選器區段](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 此 [allowProxy屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 用戶端程式庫可確保透過 **/etc.clientlibs**.

### 後續步驟 {#next-steps}

了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 [與風格體系一起發展](style-system.md) 涵蓋使用樣式系統，透過品牌專屬CSS和範本編輯器的進階政策設定來擴充核心元件。

在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本機的Git分支檢閱並部署程式碼 `tutorial/client-side-libraries-solution`.

1. 複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和資源 {#additional-resources}

### Webpack DevServer — 靜態標籤 {#webpack-dev-static}

在前幾個練習中，我們能夠更新 **ui.frontend** 模組，並透過建置程式，最終查看這些變更反映在AEM中。 接下來，我們看一個使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 快速發展我們的前端風格 **靜態** HTML。

如果大部分樣式和前端程式碼是由專用前端開發人員執行，且可能無法輕鬆存取AEM環境，則此技術非常實用。 此技術也允許FED直接對HTML進行修改，然後再將修改交給AEM開發人員，以作為元件實施。

1. 複製LA Skatepark文章頁的頁面來源： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. 重新開啟IDE。 將從AEM複製的標籤貼入 `index.html` 在 **ui.frontend** 模組下方 `src/main/webpack/static`.
1. 編輯複製的標注並刪除任何引用 **clientlib-site** 和 **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我們可以刪除這些引用，因為Webpack開發伺服器會自動生成這些對象。

1. 從新終端啟動Webpack開發伺服器，方法是在 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在 [http://localhost:8080/](http://localhost:8080/) ，使用靜態標籤。

1. 編輯檔案 `src/main/webpack/site/_variables.scss` 檔案。 取代 `$text-color` 規則，並包含下列項目：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應會自動在瀏覽器上看到 [http://localhost:8080](http://localhost:8080).

   ![本地WebPack開發伺服器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 檢閱 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 檔案。 這包含用於啟動Webpack-dev-server的Webpack配置。 它可代理路徑 `/content` 和 `/etc.clientlibs` 從本機執行的AEM例項。 這是影像和其他clientlib(不由 **ui.frontend** 代碼)可供使用。

   >[!CAUTION]
   >
   > 靜態標籤的影像src指向本機AEM例項上的即時影像元件。 如果影像路徑變更、AEM未啟動，或瀏覽器未登入本機AEM例項，影像就會顯示損毀。 如果切換為外部資源，也可以使用靜態參照來取代影像。

1. 您可以 **停止** 通過鍵入命令行中的webpack伺服器 `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** 是開放原始碼的命令列工具，可用來加速前端開發。 由 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/)，和 [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

在高級 **aemfed** 設計用於監聽 **ui.apps** 模組，並自動將它們直接同步至執行中的AEM例項。 根據變更，本機瀏覽器會自動重新整理，進而加速前端開發。 此外，此功能也可與Sling Log Tracer搭配使用，以直接在終端中自動顯示任何伺服器端錯誤。

如果您在 **ui.apps** 模組，修改HTL指令碼，以及建立自訂元件， **aemfed** 是功能強大的工具。 [您可以在此處找到完整檔案](https://github.com/abmaonline/aemfed).

### 偵錯用戶端程式庫 {#debugging-clientlibs}

使用不同的方法 **類別** 和 **內嵌** 若要納入多個用戶端程式庫，疑難排解可能會很麻煩。 AEM會公開數種工具，以提供相關協助。 最重要的工具之一是 **重建客戶端庫** 這會強制AEM重新編譯任何LESS檔案並產生CSS。

* [**轉儲庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM例項中註冊的用戶端程式庫。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**測試輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允許用戶根據類別查看clientlib的預期HTML輸出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**庫依賴項驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出顯示找不到的任何依賴項或嵌入類別。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客戶端庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 可讓使用者強制AEM重建用戶端程式庫，或使用戶端程式庫的快取無效。 此工具在使用LESS進行開發時很有效，因為這可以強制AEM重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理或重建程式庫會比更有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客戶端庫](assets/client-side-libraries/rebuild-clientlibs.png)
