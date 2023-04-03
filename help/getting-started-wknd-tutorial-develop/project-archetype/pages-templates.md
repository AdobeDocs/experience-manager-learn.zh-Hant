---
title: AEM Sites快速入門 — 頁面和範本
description: 了解基本頁面元件與可編輯範本之間的關係。 了解如何將核心元件代理至專案。 了解可編輯範本的進階政策設定，以根據Adobe XD的模型建立結構良好的文章頁面範本。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 0%

---

# 頁面和範本 {#pages-and-template}

在本章中，我們將探討基本頁面元件與可編輯範本之間的關係。 了解如何根據某些模型建立未設定樣式的文章範本，來自 [Adobe XD](https://helpx.adobe.com/support/xd.html). 在建立範本的過程中，將介紹可編輯模板的核心元件和高級策略配置。

## 必備條件 {#prerequisites}

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重複使用項目，並跳過簽出起始項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看 `tutorial/pages-templates-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 或切換到分支以本地簽出代碼 `tutorial/pages-templates-solution`.

## 目標

1. Inspect是在Adobe XD中建立的頁面設計，並對應至核心元件。
1. 了解可編輯範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 了解範本和頁面的連結方式

## 您要建置的 {#what-build}

在本教學課程的本節中，您會建立新的「文章頁面範本」，以用來建立文章頁面並與通用結構對齊。 「文章頁面範本」以Adobe XD中產生的設計和UI套件為基礎。 本章僅側重於構建模板的結構或骨架。 未實作樣式，但範本和頁面可運作。

![文章頁面設計與未設定樣式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD規劃UI {#adobexd}

規劃新網站通常從模型和靜態設計開始。 [Adobe XD](https://helpx.adobe.com/support/xd.html) 是建立使用者體驗的設計工具。 接下來，我們來檢查UI套件和模型，以協助規劃文章頁面範本的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**下載 [WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 通用 [AEM核心元件UI套件也已推出](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) 作為自訂專案的起點。

## 建立文章頁面範本

建立頁面時，您必須選取範本，以作為建立頁面的基礎。 範本會定義產生的頁面、初始內容及允許的元件結構。

有三個主要領域 [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **結構**  — 定義屬於範本一部分的元件。 內容作者無法編輯這些內容。
1. **初始內容**  — 定義範本開頭的元件，內容作者可以編輯和/或刪除這些元件
1. **原則**  — 定義元件行為設定，以及作者有哪些選項。

接下來，在AEM中建立符合模型結構的範本。 這會發生在AEM的本機例項中。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上述影片的高階步驟：

### 結構配置

1. 使用 **頁面範本類型**，命名 **文章頁面**.
1. 切換至 **結構** 模式。
1. 新增 **體驗片段** 要充當 **標題** 的上方。
   * 設定元件以指向 `/content/experience-fragments/wknd/us/en/site/header/master`.
   * 將策略設定為 **頁首** 確保 **預設元素** 設為 `header`. 此 `header`元素會在下一章中以CSS定位。
1. 新增 **體驗片段** 要充當 **頁尾** 在範本底部。
   * 設定元件以指向 `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * 將策略設定為 **頁尾** 確保 **預設元素** 設為 `footer`. 此 `footer` 元素會在下一章中以CSS定位。
1. 鎖定 **main** 初次建立範本時包含的容器。
   * 將策略設定為 **頁面主要** 確保 **預設元素** 設為 `main`. 此 `main` 元素會在下一章中以CSS定位。
1. 新增 **影像** 元件 **main** 容器。
   * 解鎖 **影像** 元件。
1. 新增 **階層連結** 元件下方 **影像** 元件。
   * 為 **階層連結** 元件已命名 **文章頁面 — 階層連結**. 設定 **導覽開始層級** to **4**.
1. 新增 **容器** 元件下方 **階層連結** 元件和內部 **main** 容器。 這就像 **內容容器** ，以取得Advertising Cloud的說明。
   * 解鎖 **內容** 容器。
   * 將策略設定為 **頁面內容**.
1. 添加其他 **容器** 元件下方 **內容容器**. 這就像 **側邊欄** 容器。
   * 解鎖 **側邊欄** 容器。
   * 建立已命名的策略 **文章頁面 — 側邊欄**.
   * 設定 **允許的元件** 在 **WKND Sites專案 — 內容** 要包括： **按鈕**, **下載**, **影像**, **清單**, **分隔符號**, **社交媒體分享**, **文字**，和 **標題**.
1. 更新「頁面根」容器的原則。 這是範本上最外層的容器。 將策略設定為 **頁面根**.
   * 在 **容器設定**，請設定 **版面** to **回應式格線**.
1. 接合佈局模式 **內容容器**. 從右到左拖動手柄，將容器縮小為八列寬。
1. 接合佈局模式 **側邊欄容器**. 從右到左拖動手柄，並將容器縮小為四列寬。 然後，從左到右拖曳左側控點一欄，使容器3欄變寬，並在容器3欄之間留有1欄的間隙 **內容容器**.
1. 開啟移動模擬器並切換到移動斷點。 再次接合佈局模式，並建立 **內容容器** 和 **側邊欄容器** 頁面的全寬。 這會在移動斷點中垂直堆疊容器。
1. 更新 **文字** 元件(位於 **內容容器**.
   * 將策略設定為 **內容文字**.
   * 在 **外掛程式** > **段落樣式**，檢查 **啟用段落樣式** 確保 **報價塊** 啟用。

### 初始內容設定

1. 切換至 **初始內容** 模式。
1. 新增 **標題** 元件 **內容容器**. 這可做為文章標題。 當保留為空白時，會自動顯示目前頁面的標題。
1. 新增秒數 **標題** 元件。
   * 使用文字設定元件：「作者」。 這是文字預留位置。
   * 將類型設定為 `H4`.
1. 新增 **文字** 元件下方 **依作者** 標題元件。
1. 新增 **標題** 元件 **側邊欄容器**.
   * 使用文字設定元件：「分享此故事」。
   * 將類型設定為 `H5`.
1. 新增 **社交媒體分享** 元件下方 **分享此故事** 標題元件。
1. 新增 **分隔符號** 元件下方 **社交媒體分享** 元件。
1. 新增 **下載** 元件下方 **分隔符號** 元件。
1. 新增 **清單** 元件下方 **下載** 元件。
1. 更新 **初始頁面屬性** ，以取得Advertising Cloud的說明。
   * 在 **社交媒體** > **社交媒體分享**，檢查 **Facebook** 和 **Pinterest**

### 啟用範本並新增縮圖

1. 導覽至 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **啟用** 文章頁面範本。
1. 編輯「文章頁面」範本的屬性並上傳下列縮圖，以快速識別使用「文章頁面」範本建立的頁面：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用體驗片段更新頁首與頁尾 {#experience-fragments}

建立全域內容（例如頁首或頁尾）時，常見的作法是使用 [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). 體驗片段，可讓使用者結合多個元件，以建立單一、可參考的元件。 體驗片段的優點是可支援多網站管理和 [本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

AEM專案原型會產生頁首和頁尾。 接下來，更新體驗片段以符合模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上述影片的高階步驟：

1. 下載範例內容套件 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. 使用封裝管理器上傳和安裝內容封裝(位於 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 更新網頁變異範本，此範本是用於的體驗片段，位於 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * 將策略更新為 **容器** 元件。
   * 將策略設定為 **XF根**.
   * 在底下， **允許的元件** 選擇元件組 **WKND Sites專案 — 結構** 包含 **語言導覽**, **導覽**，和 **快速搜尋** 元件。

### 更新標題體驗片段

1. 開啟在轉譯標題的體驗片段 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 設定根目錄 **容器** 片段。 這是最外面的 **容器**.
   * 設定 **版面** to **回應式格線**
1. 新增 **WKND深色徽標** 作為影像 **容器**. 此標誌包含在先前步驟中安裝的套件中。
   * 修改 **WKND深色徽標** 為 **two** 欄寬。 從右到左拖動控點。
   * 使用 **替代文字** 「WKND徽標」。
   * 將標誌配置為 **連結** to `/content/wknd/us/en` 首頁。
1. 設定 **導覽** 已放置在頁面上的元件。
   * 設定 **排除根層級** to **1**.
   * 設定 **導覽結構深度** to **1**.
   * 修改 **導覽** 元件 **八** 欄寬。 從右到左拖動控點。
1. 移除 **語言導覽** 元件。
1. 修改 **搜尋** 元件 **two** 欄寬。 從右到左拖動控點。 現在，所有元件應在單一列上水準對齊。

### 更新頁尾體驗片段

1. 開啟轉譯頁尾的體驗片段 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 設定根目錄 **容器** 片段。 這是最外面的 **容器**.
   * 設定 **版面** to **回應式格線**
1. 新增 **WKND Light徽標** 作為影像 **容器**. 此標誌包含在先前步驟中安裝的套件中。
   * 修改 **WKND Light徽標** 為 **two** 欄寬。 從右到左拖動控點。
   * 使用 **替代文字** 「WKND標誌燈」。
   * 將標誌配置為 **連結** to `/content/wknd/us/en` 首頁。
1. 新增 **導覽** 元件。 設定 **導覽** 元件：
   * 設定 **排除根層級** to **1**.
   * 取消選中 **收集所有子頁**.
   * 設定 **導覽結構深度** to **1**.
   * 修改 **導覽** 元件 **八** 欄寬。 從右到左拖動控點。

## 建立文章頁面

接下來，使用「文章頁面」範本建立頁面。 製作頁面內容以符合網站模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上述影片的高階步驟：

1. 導覽至網站主控台(位於 [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. 在下方建立頁面 **WKND** > **US** > **EN** > **雜誌**.
   * 選擇 **文章頁面** 範本。
   * 在 **屬性** 設定 **標題** 《LA Skateparks終極指南》
   * 設定 **名稱** &quot;指南 — 拉 — 滑板公園&quot;
1. 取代 **依作者** 標題，文字為&quot;Stacey Roswells&quot;
1. 更新 **文字** 包含要填入文章之段落的元件。 您可以使用下列文字檔案作為副本： [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. 添加其他 **文字** 元件。
   * 更新元件以包括報價：「沒有比洛杉磯更好的地方可以粉碎。」
   * 以全螢幕模式編輯RTF編輯器，並修改上述引號以使用 **報價塊** 元素。
1. 繼續填入文章內文，以符合模型。
1. 設定 **下載** 元件，以使用文章的PDF版本。
   * 在 **下載** > **屬性**，請按一下核取方塊以 **從DAM資產取得標題**.
   * 設定 **說明** 至：「全文」。
   * 設定 **動作文字** 至：&quot;下載PDF&quot;。
1. 設定 **清單** 元件。
   * 在 **清單設定** > **使用**，選取 **子頁面**.
   * 設定 **上層頁面** to `/content/wknd/us/en/magazine`.
   * 在底下， **項目設定** check **連結項目** 檢查 **顯示日期**.

## Inspect節點結構 {#node-structure}

此時，文章頁面會顯示為未設定樣式。 但基本結構已經到位。 接下來，檢查文章頁面的節點結構，以便更清楚了解範本、頁面和元件的角色。

在本機AEM例項上使用CRXDE-Lite工具來檢視基礎節點結構。

1. 開啟 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 並使用樹狀導覽來導覽 `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. 按一下 `jcr:content` 節點下方 `la-skateparks` 頁面並檢視屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   請注意 `cq:template`，指向 `/conf/wknd/settings/wcm/templates/article-page`，則會是先前建立的「文章頁面範本」 。

   另請注意 `sling:resourceType`，指向 `wknd/components/page`. 這是由AEM專案原型建立的頁面元件，負責根據範本轉譯頁面。

1. 展開 `jcr:content` 節點下方 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 並查看節點層次結構：

   ![JCR內容LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   您應能將每個節點鬆散地對應至已撰寫的元件。 查看您是否能透過檢查帶有前置詞的節點來識別所使用的不同「版面容器」 `container`.

1. 下一步，在檢查頁面元件 `/apps/wknd/components/page`. 在CRXDE Lite中查看元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   只有兩個HTL指令碼， `customfooterlibs.html` 和 `customheaderlibs.html` 頁面元件下方。 *那麼，此元件如何呈現頁面？*

   此 `sling:resourceSuperType` 屬性指向 `core/wcm/components/page/v2/page`. 此屬性允許WKND的頁面元件繼承 **all** 核心元件頁面元件的功能。 這是第一個 [代理元件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). 如需詳細資訊，請參閱 [此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect WKND元件中的另一個元件，即 `Breadcrumb` 元件來源： `/apps/wknd/components/breadcrumb`. 請注意 `sling:resourceSuperType` 屬性可以找到，但這次它指向 `core/wcm/components/breadcrumb/v2/breadcrumb`. 這是使用Proxy元件模式來包含核心元件的另一個範例。 事實上，WKND程式碼基底中的所有元件都是AEM核心元件的Proxy（自訂示範HelloWorld元件除外）。 最佳實務是盡可能重複使用核心元件的功能 *befor* 撰寫自訂程式碼。

1. 下一步檢查核心元件頁面： `/libs/core/wcm/components/page/v2/page` 使用CRXDE Lite:

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心元件位於 `/apps/core/wcm/components`. 在中，AEMas a Cloud Service，核心元件位於 `/libs` 和會自動更新。

   ![核心元件頁面](assets/pages-templates/core-page-component-properties.png)

   請注意，此頁面下麵包含許多指令碼檔案。 核心元件頁面包含許多功能。 此功能已細分為多個指令碼，以方便維護及閱讀。 您可以借由開啟 `page.html` 和尋找 `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   將HTL分割為多個指令碼的另一個原因是，可允許Proxy元件覆寫個別指令碼，以實作自訂業務邏輯。 HTL指令碼 `customfooterlibs.html`，和 `customheaderlibs.html`，是為了透過實作專案覆寫的明確目的而建立。

   您可以進一步了解可編輯範本如何影響轉譯的 [閱讀本文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect另一個核心元件，例如的階層連結 `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. 檢視 `breadcrumb.html` 指令碼，以了解如何最終產生階層連結元件的標籤。

## 將配置保存到原始碼控制 {#configuration-persistence}

通常，尤其是在AEM專案開始時，將配置（如模板和相關內容策略）保留到原始碼控制是非常有價值的。 這可確保所有開發人員針對相同的內容和設定組合工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟程度，管理模板的做法就可以交給特定的一組電源用戶。


目前，範本的處理方式與其他程式碼片段相同，並同步 **文章頁面範本** 作為項目的一部分。
直到現在，程式碼會從AEM專案推送至AEM的本機例項。 此 **文章頁面範本** 是直接在AEM的本機例項上建立，因此需要 **匯入** 範本放入AEM專案。 此 **ui.content** 模組會包含在AEM專案中，以用於此特定用途。

接下來的幾個步驟將在VSCode IDE中使用 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 外掛程式。 但他們可能使用您已配置的任何IDE **匯入** 或從本機AEM例項匯入內容。

1. 在中，VSCode會開啟 `aem-guides-wknd` 專案。

1. 展開 **ui.content** 模組。 展開 `src` 資料夾和導覽至 `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL 按一下右鍵] the `templates` 資料夾和選取 **從AEM伺服器匯入**:

   ![VSCode匯入範本](assets/pages-templates/vscode-import-templates.png)

   此 `article-page` 應匯入，而 `page-content`, `xf-web-variation` 範本也應更新。

   ![更新的範本](assets/pages-templates/updated-templates.png)

1. 重複匯入內容的步驟，但選取 **原則** 資料夾 `/conf/wknd/settings/wcm/policies`.

   ![VSCode導入策略](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` 檔案 `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   此 `filter.xml` 檔案負責標識隨軟體包一起安裝的節點的路徑。 請注意 `mode="merge"` 在指示不要修改現有內容的每個篩選器上，僅新增新內容。 由於內容作者可能更新這些路徑，因此程式碼部署必須確實更新 **not** 覆寫內容。 請參閱 [FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html) 以取得使用篩選元素的詳細資訊。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解每個模組管理的不同節點。

   >[!WARNING]
   >
   > 為了確保WKND參考網站的一致部署，已設定項目的某些分支，以便 `ui.content` 覆寫JCR中的任何變更。 這是根據設計（即解決方案分支），因為程式碼/樣式是針對特定原則所編寫。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager Sites建立範本和頁面。

### 後續步驟 {#next-steps}

此時，文章頁面會顯示為未設定樣式。 關注 [用戶端程式庫和前端工作流程](client-side-libraries.md) 教學課程，了解包含CSS和JavaScript以將全域樣式套用至網站並整合專用前端組建的最佳實務。

在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本機的Git分支檢閱並部署程式碼 `tutorial/pages-templates-solution`.

1. 複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/pages-templates-solution` 分支。
