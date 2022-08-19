---
title: 客戶端庫和前端工作流
description: 瞭解如何使用客戶端庫或客戶端庫為Adobe Experience Manager()站點實現部署和管理CSSAEM和Javascript。 瞭解如何將webpack項目ui.frontend模組整合到端到端構建流程中。
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: d49dbfae3292f93b7f63f424731966934dc6a5ba
workflow-type: tm+mt
source-wordcount: '2878'
ht-degree: 1%

---

# 客戶端庫和前端工作流 {#client-side-libraries}

瞭解如何使用客戶端庫或客戶端庫來部署和管理Adobe Experience Manager(AEM)站點實現的CSS和Javascript。 本教程還將介紹 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組，解耦 [網路包](https://webpack.js.org/) 可以整合到端到端構建流程中。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

此外，還建議對 [元件基礎](component-basics.md#client-side-libraries) 教程，瞭解客戶端庫和的基AEM礎。

### 入門項目

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出啟動程式項目的步驟。

檢查本教程所基於的基線代碼：

1. 查看 `tutorial/client-side-libraries-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用Maven技能將代碼AEM庫部署到本地實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使AEM用6.5或6.4，則追加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 或通過切換到分支本地檢出代碼 `tutorial/client-side-libraries-solution`。

## 目標

1. 瞭解客戶端庫如何通過可編輯模板包含在頁面中。
1. 瞭解如何使用UI.Frontend Module和webpack開發伺服器進行專用前端開發。
1. 瞭解將編譯的CSS和JavaScript交付到站點實現的端到端工作流。

## 您將構建的 {#what-you-will-build}

在本章中，您將為WKND站點和文章頁面模板添加一些基線樣式，以使實施更接近 [UI設計模型](assets/pages-templates/wknd-article-design.xd)。 您將使用高級前端工作流將Webpack項目整合到客AEM戶端庫。

![已完成樣式](assets/client-side-libraries/finished-styles.png)

*應用基線樣式的文章頁*

## 背景 {#background}

客戶端庫提供了組織和管理AEM Sites實現所需的CSS和JavaScript檔案的機制。 客戶端庫或客戶端庫的基本目標是：

1. 將CSS/JS儲存在小型離散檔案中，以便更輕鬆地開發和維護
1. 以有組織的方式管理對第三方框架的依賴性
1. 通過將CSS/JS連接到一兩個請求中，可最大限度地減少客戶端請求數。

有關使用的詳細資訊 [客戶端庫可在此處找到。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

客戶端庫確實存在一些限制。 最引人注目的是對Sass 、 LESS和TypeScript等常用前端語言的有限支援。 在本教程中，我們將瞭解 **ui.frontend** 模組可幫助解決此問題。

將啟動程式碼庫部署到本AEM地實例並導航至 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此頁面當前未設定樣式。 我們接下來將為WKND品牌實現客戶端庫，以將CSS和Javascript添加到頁面。

## 客戶端庫組織 {#organization}

接下來，我們將探討由 [項AEM目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)。

![高級客戶機庫組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高級圖客戶端庫組織和包含頁面*

>[!NOTE]
>
> 以下客戶端庫組織由「項目原型」生AEM成，但僅代表一個起點。 項目如何最終管理和將CSS和Javascript提供到站點實施，可能會因資源、技能和要求而大為不同。

1. 使用VSCode或其他IDE開啟 **ui.apps** 中。
1. 展開路徑 `/apps/wknd/clientlibs` 查看原型生成的客戶端。

   ![ui.apps中的客戶端](assets/client-side-libraries/four-clientlib-folders.png)

   我們將在下面詳細檢查這些客戶端。

1. 下表概述了客戶端庫。 有關 [包括客戶端庫可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)。

   | 名稱 | 說明 | 附註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND站點運行所需的CSS和JavaScript的基本級別 | 嵌入核心元件客戶端庫 |
   | `clientlib-grid` | 生成CSS [佈局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 工作。 | 可在此處配置移動/平板電腦斷點 |
   | `clientlib-site` | 包含WKND站點的站點特定主題 | 由 `ui.frontend` 模組 |
   | `clientlib-dependencies` | 嵌入任何第三方依賴項 | 由 `ui.frontend` 模組 |

1. 注意 `clientlib-site` 和 `clientlib-dependencies` 從原始碼管理忽略。 這是按設計進行的，因為這些將在生成時由 `ui.frontend` 中。

## 更新基本樣式 {#base-styles}

接下來，更新在 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 中。 中的檔案 `ui.frontend` 模組將生成 `clientlib-site` 和 `clientlib-dependecies` 包含網站主題和任何第三方依賴項的庫。

在支援語言方面，客戶端庫有一些限制，例如 [薩斯](https://sass-lang.com/) 或 [類型指令碼](https://www.typescriptlang.org/)。 有許多開源工具，如 [NPM](https://www.npmjs.com/) 和 [網路包](https://webpack.js.org/) 加快和優化前端開發。 目標 **ui.frontend** 模組能夠使用這些工具來管理大多數前端源檔案。

1. 開啟 **ui.frontend** 模組和導航 `src/main/webpack/site`。
1. 開啟檔案 `main.scss`

   ![main-scs — 入口點](assets/client-side-libraries/main-scss.png)
客戶端庫/主scs

   `main.scss` 是所有Sass檔案的入口點 `ui.frontend` 中。 將包括 `_variables.scss` 檔案，其中包含要在項目中的不同Sass檔案中使用的一系列品牌變數。 的 `_base.scss` 檔案，並為HTML元素定義一些基本樣式。 規則運算式包含下面各個元件樣式的所有樣式 `src/main/webpack/components`。 另一個規則運算式包含下面所有檔案 `src/main/webpack/site/styles`。

1. Inspect檔案 `main.ts`。 包括 `main.scss` 和用規則運算式來收集 `.js` 或 `.ts` 檔案。 此入口點將由 [WebPack配置檔案](https://webpack.js.org/configuration/) 作為整個 `ui.frontend` 中。

1. Inspect下面的檔案 `src/main/webpack/site/styles`:

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   模板中全局元素的這些檔案樣式，如頁眉、頁腳和主內容容器。 這些檔案中的CSS規則針對不同的HTML元素 `header`。 `main`,  `footer`。 這些HTML元素由上一章中的策略定義 [頁面和模板](./pages-templates.md)。

1. 展開 `components` 資料夾 `src/main/webpack` 檢查檔案。

   ![元件Sass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都映射到核心元件，如 [折疊式元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)。 每個核心元件都使用 [塊要素修改量](https://getbem.com/) 或使用BEM符號，使用樣式規則更容易針對特定CSS類。 下面的檔案 `/components` Project Archetype對每個元件AEM使用不同的邊界元規則進行了研究。

1. 下載WKND基本樣式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 和 **解壓縮** 檔案。

   ![WKND基樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為加快本教程的進度，我們提供了幾個Sass檔案，這些檔案基於核心元件和文章頁面模板的結構來實施WKND品牌。

1. 覆蓋 `ui.frontend/src` 檔案。 zip的內容應覆蓋以下資料夾：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![更改的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect更改的檔案，查看WKND樣式實施的詳細資訊。

## Inspectui.frontend整合 {#ui-frontend-integration}

內置於 **ui.frontend** 模組， [aem-clientlib生成器](https://github.com/wcm-io-frontend/aem-clientlib-generator) 從webpack/npm項目中獲取已編譯的CSS和JS對象，並將它們轉AEM換為客戶端庫。

![ui.frontend體系結構整合](assets/client-side-libraries/ui-frontend-architecture.png)

「項AEM目原型」自動設定此整合。 接下來，瞭解它的工作原理。


1. 開啟命令行終端並安裝 **ui.frontend** 模組使用 `npm install` 命令：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需在新克隆或項目的生成後運行一次。

1. 在中啟動Webpack Dev伺服器 **看** 模式：

   ```shell
   $ npm run watch
   ```

1. 這將編譯 `src` 檔案 `ui.frontend` 模組，並將更改同步AEM於 [http://localhost:4502](http://localhost:4502)

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

1. 命令 `npm run watch` 最終填充 **客戶端庫站點** 和 **客戶端庫依賴項** 的 **ui.apps** 模組，然後自動與同步AEM。

   >[!NOTE]
   >
   >還有 `npm run prod` 將精簡JS和CSS的配置檔案。 這是每當通過Maven觸發Webpack生成時的標準編譯。 有關 [ui.frontend模組可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspect檔案 `site.css` 下 `ui.frontend/dist/clientlib-site/site.css`。 這是基於Sass源檔案的已編譯CSS。

   ![分佈式站點css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect檔案 `ui.frontend/clientlib.config.js`。 這是npm插件的配置檔案， [aem-clientlib生成器](https://github.com/wcm-io-frontend/aem-clientlib-generator) 可以改變 `/dist` 移入客戶端庫，並將其移到 `ui.apps` 中。

1. Inspect檔案 `site.css` 的 **ui.apps** 模組 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`。 這應該是 `site.css` 檔案 **ui.frontend** 中。 現在它已進入 **ui.apps** 模組可以部署到AEM。

   ![ui.apps客戶端lib站點](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 自 **客戶端庫站點** 在生成時編譯，使用 **npm** 或 **馬**&#x200B;從原始碼管理中可以安全地忽略 **ui.apps** 中。 Inspect `.gitignore` 檔案 **ui.apps**。

1. 在以下位置開啟LA Stalepark文AEM章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![文章的更新基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應看到文章的更新樣式。 可能需要進行硬刷新才能清除瀏覽器快取的所有CSS檔案。

   它開始變得離莫卡酒更近了！

   >[!NOTE]
   >
   > 當從項目根觸發Maven生成時，將自AEM動執行上述用於生成和部署ui.frontend代碼的步驟 `mvn clean install -PautoInstallSinglePackage`。

## 更改樣式

接下來，在 `ui.frontend` 要查看的模組 `npm run watch` 自動將樣式部署到本地實AEM例。

1. 在 `ui.frontend` 模組開啟檔案： `ui.frontend/src/main/webpack/site/_variables.scss`。
1. 更新 `$brand-primary` 顏色變數：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   儲存變更。

1. 返回到瀏覽器並刷新AEM頁面以查看更新：

   ![客戶端庫](assets/client-side-libraries/style-update-brand-primary.png)

1. 將更改還原到 `$brand-primary` 使用命令對webpack生成進行顏色和停止 `CTRL+C`。

>[!CAUTION]
>
> 使用 **ui.frontend** 模組可能並非所有項目都必需。 的 **ui.frontend** 模組增加了額外的複雜性，如果不需要/不想使用這些高級前端工具(Sass、webpack、npm...)，則可能不需要。

## 頁面和模板包含 {#page-inclusion}

接下來，讓我們回顧一下在頁面中如何引用客戶AEM端。 在Web開發中，常見的最佳做法是將CSS包括在HTML標題中 `<head>` 和JavaScript在關閉前 `</body>` 標籤。

1. 瀏覽至文章頁面模板(位於 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 按一下 **頁面資訊** 表徵圖，在菜單中選擇 **頁面策略** 開啟 **頁面策略** 對話框。

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面策略*

1. 請注意， `wknd.dependencies` 和 `wknd.site` 清單。 預設情況下，通過「頁面策略」配置的客戶端將被拆分，以將CSS包括在頁面頭中，將JavaScript包括在正文端。 如果需要，可以顯式列出將客戶端庫JavaScript載入到頁面頭中。 這就是 `wknd.dependencies`。

   ![文章頁面模板菜單頁面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以引用 `wknd.site` 或 `wknd.dependencies` 直接從頁面元件中，使用 `customheaderlibs.html` 或 `customfooterlibs.html` 劇本，正如我們之前看到的 `wknd.base` 客戶端庫。 使用「模板」可以靈活選擇每個模板使用的客戶端。 例如，如果您有非常重的JavaScript庫，該庫將僅用於選擇模板。

1. 導航到 **洛杉磯滑板** 頁面 **文章頁面模板**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 按一下 **頁面資訊** 表徵圖，在菜單中選擇 **查看為已發佈** 開啟編輯器外的文章AEM頁面。

   ![以已發佈狀態檢視](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看的頁面源 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 並且您應能在 `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   請注意，客戶端正在使用代理 `/etc.clientlibs` 端點。 您還應看到頁面底部包含的以下客戶端庫：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 如果在6.5/6.4上執行操作，則客戶端庫將不會自動縮小。 請參閱 [HTML庫管理器以啟用修改（推薦）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)。

   >[!WARNING]
   >
   >在發佈端，客戶端庫是 **不** 從 **/app** 因為應使用 [調度器篩選器部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)。 的 [allowProxy屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 客戶端庫的CSS和JS可從 **/etc.clientlibs**。

### 後續步驟 {#next-steps}

瞭解如何使用Experience Manager的樣式系統實施單個樣式並重新使用核心元件。 [以體制發展](style-system.md) 包括使用樣式系統擴展核心元件，其中包含特定於品牌的CSS和模板編輯器的高級策略配置。

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git框上本地查看和部署代碼 `tutorial/client-side-libraries-solution`。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 儲存庫。
1. 查看 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和資源 {#additional-resources}

### Webpack DevServer — 靜態標籤 {#webpack-dev-static}

在前幾個練習中，我們可以更新 **ui.frontend** 模組，並通過構建過程，最終看到這些變AEM化。 接下來，我們將看到一種技術 [WebPack-Dev伺服器](https://webpack.js.org/configuration/dev-server/) 快速發展我們的前端風格 **靜態** HTML。

如果大多數樣式和前端代碼將由可能無法輕鬆訪問環境的專用前端開發人員執行，則此技術非常AEM方便。 該技術還允許FED直接對HTML進行修改，然後可將該修改交給開發者AEM以作為元件實施。

1. 複製LA滑板項目頁面的頁面源，位於 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 重新開啟IDE。 將複製的標AEM記貼上到 `index.html` 的 **ui.frontend** 模組下 `src/main/webpack/static`。
1. 編輯複製的標籤並刪除對 **客戶端庫站點** 和 **客戶端庫依賴項**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我們可以刪除這些引用，因為webpack dev伺服器將自動生成這些對象。

1. 通過從以下命令從中從新終端啟動webpack dev伺服器 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這將在以下位置開啟新的瀏覽器窗口： [http://localhost:8080/](http://localhost:8080/) 和靜態標籤。

1. 編輯檔案 `src/main/webpack/site/_variables.scss` 的子菜單。 替換 `$text-color` 規則，如下所示：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應自動查看在瀏覽器上自動反映的更改 [http://localhost:8080](http://localhost:8080)。

   ![本地WebPack Dev伺服器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 的子菜單。 這包含用於啟動webpack-dev-server的webpack配置。 請注意，它代表路徑 `/content` 和 `/etc.clientlibs` 從本地運行的實AEM例。 這就是映像和其他客戶端(不由 **ui.frontend** 代碼)。

   >[!CAUTION]
   >
   > 靜態標籤的影像源指向本地實例上的即時影像組AEM件。 如果映像的路徑更改、未啟動或AEM瀏覽器未登錄到本地實例，則映像將顯AEM示為斷開。 如果切換到外部資源，也可以用靜態引用替換影像。

1. 你可以 **停止** 通過鍵入命令行中的webpack伺服器 `CTRL+C`。

### 美國 {#develop-aemfed}

[**美國**](https://aemfed.io/) 是一種開源命令行工具，可用於加快前端開發。 它由  [Aemon](https://www.npmjs.com/package/aemsync)。 [瀏覽器同步](https://www.npmjs.com/package/browser-sync) 和 [Sling日誌跟蹤器](https://sling.apache.org/documentation/bundles/log-tracers.html)。

高級別 **美國** 是用於偵聽 **ui.apps** 模組，並將它們直接同步到正在運行的AEM實例。 根據這些變化，本地瀏覽器將自動刷新，從而加快前端開發。 它還與Sling Log跟蹤器配合使用，可自動在終端中直接顯示任何伺服器端錯誤。

如果你在 **ui.apps** 模組，修改HTL指令碼並建立自定義元件， **美國** 可以成為非常強大的工具。 [可在此處找到完整文檔](https://github.com/abmaonline/aemfed)。

### 調試客戶端庫 {#debugging-clientlibs}

使用不同的方法 **類別** 和 **床** 若要包括多個客戶端庫，則進行故障排除會非常麻煩。 公AEM開了幾種工具來幫助處理。 最重要的工具之一是 **重建客戶端庫** 這將強AEM制重新編譯任何LESS檔案並生成CSS。

* [**轉儲清單**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出實例中註冊的所有客戶端AEM庫。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test輸出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允許用戶查看客戶端lib的預期HTML輸出（基於類別）。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**庫相關性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出顯示找不到的任何依賴項或嵌入類別。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客戶端庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允許用戶強制重AEM建所有客戶端庫或使客戶端庫的快取無效。 此工具在使用LESS開發時特別有效，因為這AEM會強制重新編譯生成的CSS。 通常，與重建所有庫相比，使快取失效並執行頁面刷新更有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客戶端庫](assets/client-side-libraries/rebuild-clientlibs.png)
