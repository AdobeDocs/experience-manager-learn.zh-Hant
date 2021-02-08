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
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3291'
ht-degree: 0%

---


# 用戶端程式庫和前端工作流程{#client-side-libraries}

瞭解如何使用用戶端資料庫或用戶端資料庫來部署及管理Adobe Experience Manager(AEM)Sites實作的CSS和Javascript。 本教學課程還將介紹如何將[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)模組（解耦[webpack](https://webpack.js.org/)項目）整合到端到端構建過程中。

## 必備條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

此外，建議您檢閱[元件基本功能](component-basics.md#client-side-libraries)教學課程，以瞭解用戶端程式庫和AEM的基礎。

### Starter Project

>[!NOTE]
>
> 如果您成功完成上一章，可以重新使用項目，並跳過簽出起始項目的步驟。

查看教學課程所建立的基線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/client-side-libraries-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用您的Maven技巧，將程式碼庫部署至本機AEM實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)上檢視完成的程式碼，或切換至分支`tutorial/client-side-libraries-solution`，在本機檢出程式碼。

## 目標

1. 瞭解如何透過可編輯的範本將用戶端程式庫包含在頁面上。
1. 瞭解如何使用UI.Frontend Module和網頁套件開發伺服器進行專屬的前端開發。
1. 瞭解將編譯的CSS和JavaScript傳送至網站實作的端對端工作流程。

## 您將建立的{#what-you-will-build}

在本章中，您將新增WKND網站和「文章頁面範本」的一些基準樣式，以便讓實作更接近[UI設計模型](assets/pages-templates/wknd-article-design.xd)。 您將使用進階的前端工作流程，將Webpack專案整合至AEM用戶端程式庫。

![完成的樣式](assets/client-side-libraries/finished-styles.png)

*套用基線樣式的文章頁面*

## 背景 {#background}

用戶端程式庫提供組織和管理AEM網站實作所需CSS和JavaScript檔案的機制。 用戶端程式庫或用戶端程式庫的基本目標為：

1. 將CSS/JS儲存在小型的獨立檔案中，以方便開發和維護
1. 以有組織的方式管理對第三方架構的依賴
1. 將CSS/JS串連至一或兩個請求，將用戶端請求數減至最少。

有關使用[用戶端程式庫的詳細資訊，請參閱這裡。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

用戶端程式庫確實有一些限制。 最值得注意的是，對常用前端語言（例如Sass、LESS和TypeScript）的支援有限。 在教學課程中，我們將瞭解&#x200B;**ui.frontend**&#x200B;模組如何協助解決此問題。

將起始程式碼庫部署至本機AEM例項，並導覽至[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此頁面目前未設定樣式。 我們接下來將實作WKND品牌的用戶端程式庫，以新增CSS和Javascript至頁面。

## 客戶端庫組織{#organization}

接下來，我們將探討由[AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)產生的clientlibs的組織。

![高階客戶程式庫組織](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高階圖用戶端程式庫組織與頁面包含*

>[!NOTE]
>
> 下列用戶端程式庫組織由AEM Project Archetype產生，但僅代表一個起點。 專案如何最終管理CSS和Javascript並將它傳送至Sites實作，會因資源、技能和需求而大幅不同。

1. 使用VSCode或其他IDE可開啟&#x200B;**ui.apps**&#x200B;模組。
1. 展開路徑`/apps/wknd/clientlibs`以查看原型生成的clientlibs。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   我們將在下面詳細檢查這些clientlib。

1. 下表匯總了客戶機庫。 有關[包括客戶端庫的詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)。

   | 名稱 | 說明 | 附註 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND網站運作所需的CSS和JavaScript基本層級 | 內嵌核心元件用戶端lib |
   | `clientlib-grid` | 產生[版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)運作所需的CSS。 | 您可在此處設定行動／平板電腦中斷點 |
   | `clientlib-site` | 包含WKND網站的特定網站主題 | 由`ui.frontend`模組生成 |
   | `clientlib-dependencies` | 嵌入任何第三方依賴項 | 由`ui.frontend`模組生成 |

1. 請注意，`clientlib-site`和`clientlib-dependencies`是從原始碼控制項忽略的。 這是按設計進行的，因為這些將在構建時由`ui.frontend`模組生成。

## 更新基本樣式{#base-styles}

接著，更新&#x200B;**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;模組中定義的基本樣式。 `ui.frontend`模組中的檔案將生成包含站點主題和任何第三方相關性的`clientlib-site`和`clientlib-dependecies`庫。

在支援[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)等語言時，用戶端程式庫有一些限制。 有許多開放原始碼工具，例如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)，可加速並最佳化前端開發。 **ui.frontend**&#x200B;模組的目標是能夠使用這些工具管理大多數前端源檔案。

1. 開啟&#x200B;**ui.frontend**&#x200B;模組並導航至`src/main/webpack/site`。
1. 開啟檔案`main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` 是模組中所有Sass檔案的入口 `ui.frontend` 點。它將包含`_variables.scss`檔案，其中包含一系列品牌變數，可用於專案中不同的Sass檔案。 `_base.scss`檔案也包含在內，並定義HTML元素的一些基本樣式。 規則運算式包含`src/main/webpack/components`下個別元件樣式的所有樣式。 另一個規則運算式包含`src/main/webpack/site/styles`下的所有檔案。

1. 檢查檔案`main.ts`。 `main.ts` 包含 `main.scss` 並包含規則運算式，以收集 `.js` 專 `.ts` 案中的任何或檔案。此入口點將由[webpack配置檔案](https://webpack.js.org/configuration/)用作整個`ui.frontend`模組的入口點。

1. 檢查`src/main/webpack/site/styles`下面的檔案：

   ![樣式檔案](assets/client-side-libraries/style-files.png)

   範本中全域元素的這些檔案樣式，例如頁首、頁尾和主要內容容器。 這些檔案中的CSS規則針對不同的HTML元素`header`、`main`和`footer`。 這些HTML元素由上一章[Pages和Templates](./pages-templates.md)中的原則定義。

1. 展開`src/main/webpack`下的`components`資料夾並檢查檔案。

   ![元件Sass檔案](assets/client-side-libraries/component-sass-files.png)

   每個檔案都映射到核心元件，如[Accordion元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)。 每個核心元件都使用[區塊元素修飾元](https://getbem.com/)或BEM記號建立，讓您更容易使用樣式規則來定位特定CSS類別。 `/components`下方的檔案已由AEM Project Archetype（AEM專案原型）捨棄，每個元件的BEM規則都不同。

1. 下載WKND基本樣式&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)**&#x200B;和&#x200B;**unzip**&#x200B;檔案。

   ![WKND基本樣式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   為加速教學課程，我們提供了數個Sass檔案，這些檔案會根據核心元件和文章頁面範本的結構來建置WKND品牌。

1. 使用上一步驟的檔案覆蓋`ui.frontend/src`的內容。 郵遞區號的內容應覆寫下列資料夾：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![已變更的檔案](assets/client-side-libraries/changed-files-uifrontend.png)

   檢查變更的檔案，以檢視WKND樣式實作的詳細資訊。

## 檢查ui.frontend整合{#ui-frontend-integration}

內建在&#x200B;**ui.frontend**&#x200B;模組中的關鍵整合項目[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)會從webpack/npm專案擷取已編譯的CSS和JS對象，並將它們轉換為AEM用戶端程式庫。

![ui.frontend架構整合](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype會自動設定此整合。 接下來，探索它的運作方式。


1. 開啟命令行終端並使用`npm install`命令安裝&#x200B;**ui.frontend**&#x200B;模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需在新克隆或生成項目後運行一次。

1. 在同一終端中，使用`npm run dev`命令建立並部署&#x200B;**ui.frontend**&#x200B;模組：

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > 您可能會收到「錯誤於」等錯誤。/src/main/webpack/site/main.scss」。
   > 這通常是因為您的環境在運行`npm install`後發生了更改。
   > 執行`npm rebuild node-sass`以修正問題。 如果本機開發機器上安裝的`npm`版本與`aem-guides-wknd/pom.xml`檔案中Maven `frontend-maven-plugin`使用的版本不同，就會發生此情況。 通過修改pom檔案中的版本以匹配本地版本或相反，可以永久修正此問題。

1. 命令`npm run dev`應建立並編譯Webpack項目的原始碼，並最終在&#x200B;**ui.apps**&#x200B;模組中填入&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependences**。

   >[!NOTE]
   >
   >此外，還有`npm run prod`描述檔，可將JS和CSS精簡化。 每當透過Maven觸發Webpack組建時，這都是標準編譯。 有關[ui.frontend模組的詳細資訊，請參閱](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. 檢查`ui.frontend/dist/clientlib-site/css/site.css`下方的檔案`site.css`。 這是根據Sass來源檔案編譯的CSS。

   ![分散式網站css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 檢查檔案`ui.frontend/clientlib.config.js`。 這是npm外掛程式[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的設定檔，可將`/dist`的內容轉換為用戶端程式庫，並將它移至`ui.apps`模組。

1. 在&#x200B;**ui.apps**&#x200B;模組的`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`檢查檔案`site.css`。 這應該是&#x200B;**ui.frontend**&#x200B;模組中`site.css`檔案的相同副本。 現在它位於&#x200B;**ui.apps**&#x200B;模組中，可將它部署至AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由於&#x200B;**clientlib-site**&#x200B;是在建立時期使用&#x200B;**npm**&#x200B;或&#x200B;**maven**&#x200B;編譯，因此可安全地從&#x200B;**ui.apps**&#x200B;模組的來源控制項忽略它。 檢查&#x200B;**ui.apps**&#x200B;下方的`.gitignore`檔案。

1. 使用開發人員工具或Maven技巧，將`clientlib-site`程式庫與AEM的本機例項同步。

   ![同步Clientlib網站](assets/client-side-libraries/sync-clientlib-site.png)

1. 在AEM中開啟LA Skatepark文章，網址為：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![更新文章的基本樣式](assets/client-side-libraries/updated-base-styles.png)

   您現在應該會看到文章的更新樣式。 您可能需要進行硬式重新整理，以清除瀏覽器快取的任何CSS檔案。

   它開始越來越接近模型了！

   >[!NOTE]
   >
   > 當從專案`mvn clean install -PautoInstallSinglePackage`的根目錄觸發Maven組建時，上述建立ui.frontend程式碼並部署至AEM的步驟會自動執行。

>[!CAUTION]
>
> 所有專案可能不需要使用&#x200B;**ui.frontend**&#x200B;模組。 **ui.frontend**&#x200B;模組會增加額外的複雜性，如果不需要／希望使用這些進階前端工具(Sass、webpack、npm...)，則可能不需要。

## 頁面和範本包含{#page-inclusion}

接下來，讓我們檢視AEM頁面中客戶端的參考方式。 在網頁開發中，最常見的最佳實務是在關閉`</body>`標籤之前，將CSS加入HTML標題`<head>`和JavaScript中。

1. 在&#x200B;**ui.apps**&#x200B;模組中，導覽至`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`。

   ![結構頁元件](assets/client-side-libraries/customheaderlibs-html.png)

   這是`page`元件，用於呈現WKND實作中的所有頁面。

1. 開啟檔案`customheaderlibs.html`。 請注意`${clientlib.css @ categories='wknd.base'}`行。 這表示包含`wknd.base`類別的clientlib的CSS將透過此檔案包含，有效地將&#x200B;**clientlib-base**&#x200B;包含在我們所有頁面的標題中。

1. 更新`customheaderlibs.html`以包含我們先前在&#x200B;**ui.frontend**&#x200B;模組中指定之Google字型樣式的參考。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. 檢查檔案`customfooterlibs.html`。 此檔案（如`customheaderlibs.html`）是建置專案時要覆寫的。 此處，行`${clientlib.js @ categories='wknd.base'}`表示來自&#x200B;**clientlib-base**&#x200B;的JavaScript將包含在我們所有頁面的底部。

1. 使用開發人員工具或使用您的Maven技巧，將`page`元件匯出至AEM伺服器。

1. 瀏覽至[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)的「文章頁面」範本

1. 按一下&#x200B;**頁資訊**&#x200B;表徵圖，在菜單中選擇&#x200B;**頁策略**&#x200B;開啟&#x200B;**頁策略**&#x200B;對話框。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy.png)

   *頁面資訊>頁面政策*

1. 請注意，`wknd.dependencies`和`wknd.site`的類別列在此處。 依預設，會分割透過「頁面原則」設定的clientlib，以在頁面標題中包含CSS，並在內文結尾處包含JavaScript。 如有需要，您可明確列出要將clientlib JavaScript載入頁面標題中。 `wknd.dependencies`的情況如下。

   ![文章頁面範本功能表頁面原則](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 您也可以使用`customheaderlibs.html`或`customfooterlibs.html`指令碼，直接從頁面元件參考`wknd.site`或`wknd.dependencies`，如我們之前在`wknd.base` clientlib中所見。 使用範本可提供一些彈性，讓您可以挑選並選擇每個範本使用的客戶端。 例如，如果您有非常繁重的JavaScript程式庫，而且只會用於選取的範本。

1. 導覽至使用&#x200B;**文章頁面範本**&#x200B;建立的&#x200B;**LA Skateparks**&#x200B;頁面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 您應該會看到字型的差異。

1. 按一下「**頁面資訊**」圖示，然後在功能表中選取「檢視為已發佈」**，以在AEM編輯器外開啟文章頁面。**

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

   請注意，clientlibs使用proxy `/etc.clientlibs`端點。 您也應該會在頁面底部看到下列clientlib:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 如果遵循6.5/6.4的規定，則用戶端程式庫將不會自動縮小。 請參閱[HTML程式庫管理員上的檔案，以啟用精簡（建議）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)。

   >[!WARNING]
   >
   >在發佈端，用戶端程式庫是&#x200B;**not**，由於應使用[ Dispatcher section](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)來限制此路徑，因此應使用&#x200B;**Dispatcher section**&#x200B;來提供。 用戶端程式庫的[allowProxy屬性](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)可確保從&#x200B;**/etc.clientlibs**&#x200B;提供CSS和JS。

## Webpack DevServer —— 靜態標籤{#webpack-dev-static}

在前幾項練習中，我們可以在&#x200B;**ui.frontend**&#x200B;模組中更新數個Sass檔案，並透過建立程式，最終在AEM中看到這些變更。 接下來，我們將探討運用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)的技巧，針對&#x200B;**static** HTML快速開發我們的前端樣式。

如果大部分的樣式和前端程式碼都由可能無法輕鬆存取AEM環境的專屬前端開發人員執行，此技巧就十分有用。 此技巧也可讓FED直接對HTML進行修改，然後將修改交給AEM開發人員，以做為元件實作。

1. 複製LA skatepark文章頁面的頁面來源，網址為[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 重新開啟IDE。 將複製的標籤從AEM貼入&#x200B;**ui.frontend**&#x200B;模組（位於`src/main/webpack/static`下方）的`index.html`。
1. 編輯複製的標籤並刪除對&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependences**&#x200B;的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我們可以刪除這些引用，因為webpack dev伺服器將自動生成這些對象。

1. 在&#x200B;**ui.frontend**&#x200B;模組中運行以下命令，從新終端啟動webpack dev伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 這應該會在[http://localhost:8080/](http://localhost:8080/)開啟一個新的瀏覽器視窗，並加上靜態標籤。

1. 編輯檔案`src/main/webpack/site/_variables.scss`。 將`$text-color`規則取代為：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   儲存變更。

1. 您應該會自動在[http://localhost:8080](http://localhost:8080)的瀏覽器中看到自動反映的變更。

   ![本機WebPack開發伺服器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看`/aem-guides-wknd.ui.frontend/webpack.dev.js`檔案。 這包含用於啟動webpack-dev-server的webpack配置。 請注意，它會從本機執行的AEM例項中代理路徑`/content`和`/etc.clientlibs`。 這是影像和其他clientlibs（非由&#x200B;**ui.frontend**&#x200B;程式碼管理）的可用方式。

   >[!CAUTION]
   >
   > 靜態標籤的影像src會指向本機AEM例項上的即時影像元件。 如果影像路徑變更、AEM未啟動或瀏覽器未登入本機AEM例項，影像將會顯示中斷。 如果將影像交給外部資源，也可以用靜態參照來取代影像。

1. 通過鍵入`CTRL+C`，可以從命令行中&#x200B;**stop** webpack伺服器。

## Webpack DevServer —— 監視和Aemsync {#webpack-dev-watch}

另一種技術是讓Node.js監視`ui.frontend`模組中src檔案的任何檔案更改。 每當檔案變更時，它會快速編譯用戶端程式庫，並使用[aemsync](https://www.npmjs.com/package/aemsync) npm模組，將變更同步至執行中的AEM伺服器。

1. 在&#x200B;**ui.frontend**&#x200B;模組內運行以下命令，從新終端以&#x200B;**watch**&#x200B;模式啟動webpack dev server:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 這將編譯`src`檔案，並與AEM同步變更，網址為[http://localhost:4502](http://localhost:4502)

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

   ![已部署至AEM的變更](assets/client-side-libraries/changes-deployed-aem-watch.png)

   變更應部署至AEM。 有輕微的延遲，您必須手動重新整理瀏覽器才能檢視更新。 不過，如果您使用新元件和對話方塊編寫，直接在AEM中檢視變更會很有用。

1. 將更改還原為`_variables.scss`並保存更改。 稍有延遲後，變更應再次與AEM的本機例項同步。

1. 停止webpack dev伺服器，並從項目的根目錄執行完整的Maven構建：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   同樣地， `ui.frontend`模組會編譯、轉換為clientlibraries，並透過`ui.apps`模組部署至AEM。 但這次馬文為我們做了一切。

## 恭喜！{#congratulations}

恭喜的是，「文章頁面」現在有一些與WKND品牌相符的一致樣式，而且您已熟悉&#x200B;**ui.frontend**&#x200B;模組！

### 後續步驟{#next-steps}

瞭解如何使用Experience Manager的Style System建置個別樣式並重複使用核心元件。 [使用樣式系統開](style-system.md) 發內容時，可使用樣式系統，以特定品牌的CSS和範本編輯器的進階原則組態來擴充核心元件。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在`tutorial/client-side-libraries-solution`的Git位置上檢視並部署程式碼。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`tutorial/client-side-libraries-solution`分支。

## 其他工具和資源{#additional-resources}

### aemfed {#develop-aemfed}

[**Aemfedis**](https://aemfed.io/) 是開放原始碼、命令列工具，可用來加速前端開發。它由[aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://www.npmjs.com/package/browser-sync)和[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)提供支援。

在高階&#x200B;**aemfed**&#x200B;設計為監聽&#x200B;**ui.apps**&#x200B;模組中的檔案變更，並直接自動同步至執行中的AEM例項。 本端瀏覽器會根據變更自動重新整理，進而加速前端開發。 此外，它還可與Sling Log Tracer搭配使用，以直接在終端中自動顯示任何伺服器端錯誤。

如果您在&#x200B;**ui.apps**&#x200B;模組中進行許多工作、修改HTL指令碼並建立自訂元件，**aemfed**&#x200B;可成為非常強大的使用工具。 [您可在這裡找到完整的檔案。](https://github.com/abmaonline/aemfed).

### 調試客戶端庫{#debugging-clientlibs}

使用&#x200B;**類別**&#x200B;和&#x200B;**的不同方法嵌入**&#x200B;的多個客戶端庫時，疑難排解會很麻煩。 AEM提供數種工具來協助處理此問題。 最重要的工具之一是&#x200B;**重建用戶端程式庫**，這會強制AEM重新編譯任何LESS檔案並產生CSS。

* [**轉儲庫**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列出在AEM實例中註冊的所有客戶端庫。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Test Output**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  —— 允許使用者根據類別查看clientlib的預期HTML輸出。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**庫相關性驗證**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出顯示所有找不到的相關性或嵌入類別。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Rebuild Client Libraries**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  —— 允許使用者強制AEM重建所有用戶端程式庫，或使用戶端程式庫的快取失效。此工具在使用LESS進行開發時特別有效，因為這會迫使AEM重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理與重建所有程式庫比較有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客戶端庫](assets/client-side-libraries/rebuild-clientlibs.png)
