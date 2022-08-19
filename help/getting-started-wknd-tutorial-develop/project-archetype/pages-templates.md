---
title: AEM Sites入門 — 頁面和模板
description: 瞭解基本頁元件與可編輯模板之間的關係。 瞭解如何將核心元件代理到項目中。 瞭解可編輯模板的高級策略配置，以基於Adobe XD的模型構建結構良好的文章頁面模板。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
source-git-commit: d49dbfae3292f93b7f63f424731966934dc6a5ba
workflow-type: tm+mt
source-wordcount: '3081'
ht-degree: 0%

---

# 頁面和模板 {#pages-and-template}

在本章中，我們將探討基本頁元件與可編輯模板之間的關係。 我們將根據來自 [AdobeXD](https://www.adobe.com/products/xd.html)。 在構建模板的過程中，將介紹「可編輯模板」的核心元件和高級策略配置。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

### 入門項目

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出啟動程式項目的步驟。

檢查本教程所基於的基線代碼：

1. 查看 `tutorial/pages-templates-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 或通過切換到分支本地檢出代碼 `tutorial/pages-templates-solution`。

## 目標

1. Inspect在Adobe XD建立的頁面設計，並將其映射到核心元件。
1. 瞭解可編輯模板的詳細資訊以及如何使用策略來實施對頁面內容的精細控制。
1. 瞭解模板和頁面的連結方式

## 您將構建的 {#what-you-will-build}

在本教程的這一部分中，您將構建一個新的「文章頁面模板」，該模板可用於建立新的文章頁面，並與通用結構對齊。 文章頁面模板將基於AdobeXD中生成的設計和UI工具包。 本章僅側重於構建模板的結構或骨架。 不會實施任何樣式，但模板和頁面將起作用。

![文章頁面設計和未設樣式的版本](assets/pages-templates/what-you-will-build.png)

## UI規劃與Adobe XD {#adobexd}

在大多數情況下，新網站的規劃都從模型和靜態設計開始。 [Adobe XD](https://www.adobe.com/products/xd.html) 是構建用戶體驗的設計工具。 接下來，我們將檢查UI工具包和模型，以幫助規劃文章頁面模板的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下載 [WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 泛型 [還AEM提供核心元件UI套件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) 作為自定義項目的起點。

## 建立文章頁模板

建立頁面時，必須選擇模板，該模板將用作建立新頁面的基礎。 模板定義結果頁面的結構、初始內容和允許的元件。

有3個主要區域 [可編輯模板](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **結構**  — 定義作為模板一部分的元件。 內容作者將不可編輯這些內容。
1. **初始內容**  — 定義模板將以開始的元件，內容作者可以編輯和/或刪除這些元件
1. **策略**  — 定義元件的行為方式以及作者將提供哪些選項的配置。

接下來，在與模AEM型結構匹配的新模板中。 這將在的本地實例中發AEM生。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

以上視頻的高級步驟：

### 結構配置

1. 使用 **頁面模板類型**，命名 **文章頁**。
1. 切換到 **結構** 的子菜單。
1. 添加 **體驗片段** 要用作的元件 **標題** 的下界。
   * 配置元件以指向 `/content/experience-fragments/wknd/us/en/site/header/master`。
   * 將策略設定為 **頁眉** 確保 **預設元素** 設定為 `header`。 的 `header`元素將在下一章中與CSS對應。
1. 添加 **體驗片段** 要用作的元件 **頁腳** 的下界。
   * 配置元件以指向 `/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 將策略設定為 **頁腳** 確保 **預設元素** 設定為 `footer`。 的 `footer` 元素將在下一章中與CSS對應。
1. 鎖定 **主** 建立模板時包含的容器。
   * 將策略設定為 **頁面首頁** 確保 **預設元素** 設定為 `main`。 的 `main` 元素將在下一章中與CSS對應。
1. 添加 **影像** 元件 **主** 容器。
   * 解鎖 **影像** 元件。
1. 添加 **麵包屑** 元件下方 **影像** 元件。
   * 為 **麵包屑** 元件命名 **文章頁 — Breadcrumb**。 設定 **導航起始級別** 至 **4**。
1. 添加 **容器** 元件下方 **麵包屑** 元件和內部 **主** 容器。 這將充當 **內容容器** 的子菜單。
   * 解鎖 **內容** 容器。
   * 將策略設定為 **頁面內容**。
1. 添加其他 **容器** 元件下方 **內容容器**。 這將充當 **側軌** 的下界。
   * 解鎖 **側軌** 容器。
   * 建立名為 **文章頁 — 側欄**。
   * 配置 **允許的元件** 在 **WKND站點項目 — 內容** 要包括： **按鈕**。 **下載**。 **影像**。 **清單**。 **分隔符**。 **社交媒體共用**。 **文本**, **標題**。
1. 更新「頁根」容器的策略。 這是模板上最外層的容器。 將策略設定為 **頁根**。
   * 下 **容器設定**&#x200B;的子菜單。 **佈局** 至 **響應網格**。
1. 為 **內容容器**。 從右到左拖動手柄，將容器縮小為8列寬。
1. 為 **側軌集裝箱**。 從右到左拖動手柄，將容器縮小為4列寬。 然後，將左手柄從左到右1列拖動，使容器3列變寬，並在 **內容容器**。
1. 開啟移動模擬器並切換到移動斷點。 再次激活佈局模式並 **內容容器** 和 **側軌集裝箱** 頁面的全寬。 這將在移動斷點中垂直堆疊容器。
1. 更新策略 **文本** 元件 **內容容器**。
   * 將策略設定為 **內容文本**。
   * 下 **插件** > **段落樣式**&#x200B;選中 **啟用段落樣式** 確保 **報價塊** 的子菜單。

### 初始內容配置

1. 切換到 **初始內容** 的子菜單。
1. 添加 **標題** 元件 **內容容器**。 這將作為文章標題。 當它為空時，它將自動顯示當前頁面的標題。
1. 添加秒 **標題** 元件。
   * 使用文本配置元件：「作者」。 這將是文本佔位符。
   * 將類型設定為 `H4`。
1. 添加 **文本** 元件下方 **按作者** 標題元件。
1. 添加 **標題** 元件 **側軌集裝箱**。
   * 使用文本配置元件：「分享此故事」。
   * 將類型設定為 `H5`。
1. 添加 **社交媒體共用** 元件下方 **共用此故事** 標題元件。
1. 添加 **分隔符** 元件下方 **社交媒體共用** 元件。
1. 添加 **下載** 元件下方 **分隔符** 元件。
1. 添加 **清單** 元件下方 **下載** 元件。
1. 更新 **初始頁屬性** 的子菜單。
   * 下 **社交媒體** > **社交媒體共用**&#x200B;選中 **Facebook** 和 **Pinterest**

### 啟用模板並添加縮略圖

1. 導航至 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **啟用** 文章頁面模板。
1. 編輯文章頁模板的屬性並上載以下縮略圖，以快速標識使用文章頁模板建立的頁面：

   ![文章頁面模板縮略圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用經驗片段更新頁眉和頁腳 {#experience-fragments}

建立全局內容（如頁眉或頁腳）時，通常使用 [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 體驗片段，允許用戶組合多個元件以建立單個可引用的元件。 經驗片段具有支援多站點管理和 [定位](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

「項AEM目原型」生成了頁眉和頁腳。 接下來，更新「體驗片段」以匹配模型。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

以上視頻的高級步驟：

1. 下載示例內容包 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**。
1. 使用Package Manager上載和安裝內容包 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 更新Web變體模板，該模板用於以下位置的體驗片段 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * 更新策略 **容器** 的下界。
   * 將策略設定為 **XF根**。
   * 下 **允許的元件** 選擇元件組 **WKND站點項目 — 結構** 包括 **語言導航**。 **導航**, **快速搜索** 元件。

### 更新標頭體驗片段

1. 開啟在以下位置呈現標題的體驗片段 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 配置根 **容器** 碎片。 這是最外層的 **容器**。
   * 設定 **佈局** 至 **響應網格**
1. 添加 **WKND Dark徽標** 作為影像 **容器**。 徽標包含在上一步驟中安裝的軟體包中。
   * 修改佈局 **WKND Dark徽標** 要 **2** 欄寬。 從右到左拖動手柄。
   * 配置徽標 **備選文本** &quot;WKND徽標&quot;
   * 將徽標配置為 **連結** 至 `/content/wknd/us/en` 的上界。
1. 配置 **導航** 已放在頁面上的元件。
   * 設定 **排除根級別** 至 **1**。
   * 設定 **導航結構深度** 至 **1**。
   * 修改佈局 **導航** 元件 **8** 欄寬。 從右到左拖動手柄。
1. 刪除 **語言導航** 元件。
1. 修改佈局 **搜索** 元件 **2** 欄寬。 從右到左拖動手柄。 現在，所有元件應在一行上水準對齊。

### 更新頁腳體驗片段

1. 開啟在以下位置呈現頁腳的體驗片段 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 配置根 **容器** 碎片。 這是最外層的 **容器**。
   * 設定 **佈局** 至 **響應網格**
1. 添加 **WKND Light徽標** 作為影像 **容器**。 徽標包含在上一步驟中安裝的軟體包中。
   * 修改佈局 **WKND Light徽標** 要 **2** 欄寬。 從右到左拖動手柄。
   * 配置徽標 **備選文本** &quot;WKND標識燈&quot;
   * 將徽標配置為 **連結** 至 `/content/wknd/us/en` 的上界。
1. 添加 **導航** 標誌下方的元件。 配置 **導航** 元件：
   * 設定 **排除根級別** 至 **1**。
   * 取消選中 **收集所有子頁**。
   * 設定 **導航結構深度** 至 **1**。
   * 修改佈局 **導航** 元件 **8** 欄寬。 從右到左拖動手柄。

## 「建立項目」頁

接下來，使用「文章頁面」模板建立新頁面。 編寫頁面內容以匹配網站模型。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

以上視頻的高級步驟：

1. 導航至「站點」控制台，位於 [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)。
1. 在下面建立新頁 **WKND** > **美國** > **EN** > **雜誌**。
   * 選擇 **文章頁** 的下界。
   * 下 **屬性** 設定 **標題** 《洛杉磯滑板終極指南》
   * 設定 **名稱** &quot;滑板導遊&quot;
1. 替換 **按作者** 標題，文字為&quot;Stacey Roswells&quot;
1. 更新 **文本** 元件，以包含要填充文章的段落。 可以使用以下文本檔案作為副本： [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加其他 **文本** 元件。
   * 更新元件以包括報價：「沒有比洛杉磯更好的碎片之地了。」
   * 在全屏模式下編輯RT編輯器，並修改上述引號以使用 **報價塊** 的子菜單。
1. 繼續填充文章正文以匹配模型。
1. 配置 **下載** 元件以使用項目的PDF版本。
   * 下 **下載** > **屬性**，按一下複選框 **從DAM資產獲取標題**。
   * 設定 **說明** 至：「Get the Full Story」（獲取完整故事）。
   * 設定 **操作文本** 至：&quot;下載PDF&quot;。
1. 配置 **清單** 元件。
   * 下 **清單設定** > **生成清單使用**&#x200B;選中 **子頁**。
   * 設定 **父頁** 至 `/content/wknd/us/en/magazine`。
   * 下 **項設定** 檢查 **連結項** 檢查 **顯示日期**。

## Inspect節點結構 {#node-structure}

此時，文章頁面顯然沒有樣式。 但基本結構已經到位。 接下來，檢查文章頁面的節點結構，以更好地瞭解模板、頁面和元件的作用。

在本地實例上使用CRXDE-Lite工AEM具查看基礎節點結構。

1. 開啟 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 並使用樹導航導航 `/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下 `jcr:content` 節點下 `la-skateparks` 頁並查看屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   注意 `cq:template`，指向 `/conf/wknd/settings/wcm/templates/article-page`，我們先前建立的文章頁面模板。

   另請注意 `sling:resourceType`，指向 `wknd/components/page`。 這是由項目原型建立的頁AEM面元件，負責根據模板呈現頁面。

1. 展開 `jcr:content` 節點 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 並查看節點層次結構：

   ![JCR內容LA滑板](assets/pages-templates/page-jcr-structure.png)

   您應該能夠鬆散地將每個節點映射到已創作的元件。 查看是否可以通過檢查以前固定的節點來標識不同的佈局容器 `container`。

1. 接下來，在 `/apps/wknd/components/page`。 在CRXDE Lite中查看元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   請注意，只有2個HTL指令碼， `customfooterlibs.html` 和 `customheaderlibs.html` 頁面元件下。 *那麼，這個元件如何呈現頁面？*

   的 `sling:resourceSuperType` 屬性點 `core/wcm/components/page/v2/page`。 此屬性允許WKND的頁元件繼承 **全部** 功能。 這是第一個例子 [代理元件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)。 可以找到更多資訊 [這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html)。

1. InspectWKND元件中的另一個元件， `Breadcrumb` 元件位於： `/apps/wknd/components/breadcrumb`。 注意 `sling:resourceSuperType` 可以找到屬性，但這次它指向 `core/wcm/components/breadcrumb/v2/breadcrumb`。 這是使用代理元件模式包括核心元件的另一個示例。 事實上，WKND代碼庫中的所有元件都是核心元件的代AEM理（我們著名的HelloWorld元件除外）。 最好嘗試並盡可能重複使用核心元件的功能 *先* 編寫自定義代碼。

1. 下一步，檢查「核心元件」頁，位於 `/libs/core/wcm/components/page/v2/page` 使用CRXDE Lite:

   >[!NOTE]
   >
   > 在AEM6.5/6.4中，核心元件位於 `/apps/core/wcm/components`。 在AEMas a Cloud Service中，核心元件位於 `/libs` 並自動更新。

   ![「核心元件」頁](assets/pages-templates/core-page-component-properties.png)

   請注意，此頁下麵包含了更多指令碼。 「核心元件」頁包含許多功能。 此功能被分成多個指令碼，以便更方便維護和可讀性。 通過開啟 `page.html` 尋找 `data-sly-include`:

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

   將HTL拆分為多個指令碼的另一個原因是允許代理元件覆蓋單個指令碼以實現自定義業務邏輯。 HTL指令碼， `customfooterlibs.html` 和 `customheaderlibs.html`，建立的目的是通過實施項目覆蓋。

   您可以瞭解有關可編輯模板因子如何呈現 [閱讀此文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)。

1. Inspect的另一個核心元件，如Breadcrumb `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`。 查看 `breadcrumb.html` 指令碼，瞭解如何最終生成Breadcrumb元件的標籤。

## 將配置保存到原始碼管理 {#configuration-persistence}

在許多情況下，特別是在項AEM目開始時，將配置（如模板和相關內容策略）保留到原始碼管理非常有價值。 這可確保所有開發人員都針對同一組內容和配置工作，並可確保環境之間的更多一致性。 一旦項目達到一定的成熟度，模板管理的做法就可以轉給特定的用戶群。

目前，我們將像處理其他代碼一樣處理模板並同步 **文章頁面模板** 作為項目的一部分。 直到現在 **推** 從我們的AEM項目到本地實例的代AEM碼。 的 **文章頁面模板** 直接建立於的本地實AEM例上，因此 **導入** 我們項目的模AEM板。 的 **ui.content** 模組包含在項AEM目中，用於此特定用途。

接下來的幾個步驟將使用VSCode IDE執行 [VSCode同AEM步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 插件，但可能正在使用配置為 **導入** 或從的本地實例導入內AEM容。

1. 在VSCode中，開啟 `aem-guides-wknd` 項目。

1. 展開 **ui.content** 的子菜單。 展開 `src` 資料夾和導航 `/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 按一下右鍵] 這樣 `templates` 資料夾和 **從伺服器導AEM入**:

   ![VSCode導入模板](assets/pages-templates/vscode-import-templates.png)

   的 `article-page` 應該是進口的， `page-content`。 `xf-web-variation` 模板也應更新。

   ![更新的模板](assets/pages-templates/updated-templates.png)

1. 重複導入內容的步驟，但選擇 **策略** 位於 `/conf/wknd/settings/wcm/policies`。

   ![VSCode導入策略](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` 位於 `ui.content/src/main/content/META-INF/vault/filter.xml`。

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

   的 `filter.xml` 檔案負責標識將隨軟體包一起安裝的節點的路徑。 注意 `mode="merge"` 在每個篩選器上，指示不會修改現有內容，只添加新內容。 由於內容作者可能正在更新這些路徑，因此代碼部署必須執行 **不** 覆蓋內容。 查看 [FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html) 的子菜單。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 瞭解各模組管理的不同節點。

   >[!WARNING]
   >
   > 為確保WKND參考站點的一致部署，項目的某些分支已設定為 `ui.content` 將覆蓋JCR中的任何更改。 這是按設計進行的，即針對解決方案分支，因為代碼/樣式將針對特定策略編寫。

## 恭喜！ {#congratulations}

恭喜，您剛剛與Adobe Experience Manager Sites建立了新模板和頁面。

### 後續步驟 {#next-steps}

此時，文章頁面顯然沒有樣式。 關注 [客戶端庫和前端工作流](client-side-libraries.md) 本教程旨在瞭解包括CSS和Javascript在內的最佳實踐，以將全局樣式應用到站點並整合專用的前端構建。

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git框上本地查看和部署代碼 `tutorial/pages-templates-solution`。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 儲存庫。
1. 查看 `tutorial/pages-templates-solution` 分支。
