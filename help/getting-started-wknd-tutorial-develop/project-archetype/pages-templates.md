---
title: AEM Sites 快速入門 - 頁面和範本
description: 了解基礎頁面元件和可編輯範本之間的關係。了解如何透過 Proxy 將核心元件帶入專案。了解可編輯範本的進階原則設定，以便根據取自 Adobe XD 的模型建置結構良好的文章頁面範本。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# 頁面和範本 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

在本章中，我們來探索基礎頁面元件和可編輯範本之間的關係。了解如何根據取自 [Adobe XD](https://helpx.adobe.com/tw/support/xd.html) 的一些模型來建置無樣式的文章範本。在建立範本的過程中會涵蓋核心元件和可編輯範本的進階原則設定。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具與指示。

### 入門專案

>[!NOTE]
>
> 若已成功完成上一章，您可以重複使用該專案並略過摸索入門專案的步驟。

查看作為本教學課程基礎內容的基準程式碼：

1. 查看來自 [GitHub](https://github.com/adobe/aem-guides-wknd) 的 `tutorial/pages-templates-start` 分支

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

您隨時可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 上檢視完成的程式碼，或透過切換到分支 `tutorial/pages-templates-solution` 在本機查看程式碼。

## 目標

1. 檢查在 Adobe XD 中建立的頁面設計並將其對應到核心元件。
1. 了解可編輯範本的詳細資訊以及如何使用原則對頁面內容執行精細控制。
1. 了解範本與頁面如何連結

## 您將要建置的內容 {#what-build}

在教學課程的這一部分，您會建置一個新的文章頁面範本，並可使用該範本來建立文章頁面並與通用結構保持一致。文章頁面範本是以 Adobe XD 中製作的設計和 UI 套件為基礎。本章的重點僅在於建置範本的結構或框架。沒有實施任何樣式，但範本和頁面皆可正常運作。

![文章頁面設計和無樣式版本](assets/pages-templates/what-you-will-build.png)

## 使用 Adobe XD 進行 UI 規劃 {#adobexd}

新網站的規劃通常從模型和靜態設計開始。[Adobe XD](https://helpx.adobe.com/tw/support/xd.html) 是一款建置使用者體驗的設計工具。接下來，我們來檢查 UI 套件和各個模型，協助規劃文章頁面範本的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**下載 [WKND 文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 我們也[提供通用的 AEM 核心元件 UI 套件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=zh-Hant)，可用作自訂專案的起點。

## 建立文章頁面範本

建立頁面時您必須選取範本，以用作建立頁面的基礎。範本定義所產生頁面的結構、初始內容和允許使用的元件。

[可編輯範本](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hant)包含三個主要部分：

1. **結構** - 定義屬於範本一部分的元件。內容作者無法編輯這些元件。
1. **初始內容** - 定義範本開始時使用的元件，而內容作者可以編輯和/或刪除這些元件
1. **原則** - 定義各個元件的行為方式以及作者可以使用哪些選項的設定。

接下來，在 AEM 中建立一個與模型結構相符的範本。這個動作會在 AEM 本機實例中進行。請按照以下影片的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上述影片的概括性步驟：

### 結構設定

1. 使用&#x200B;**「頁面」範本類型**&#x200B;建立一個範本，為取名為「**文章頁面**」
1. 切換到「**結構**」模式。
1. 新增&#x200B;**體驗片段**&#x200B;元件，放在範本頂部作為&#x200B;**頁首**。
   * 設定元件以便指向 `/content/experience-fragments/wknd/us/en/site/header/master`。
   * 將原則設定為「**頁面頁首**」並確保「**預設元素**」設為「`header`」。在下一章，CSS 會針對 `header` 元素進行設定。
1. 新增&#x200B;**體驗片段**&#x200B;元件，放在範本底部作為&#x200B;**頁尾**。
   * 設定元件以便指向 `/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 將原則設定為「**頁面頁尾**」並確保「**預設元素**」設為「`footer`」。在下一章，CSS 會針對 `footer` 元素進行設定。
1. 鎖定在初次建立範本時所包含的&#x200B;**主要**&#x200B;容器。
   * 將原則設定為「**頁面主要內容**」並確保「**預設元素**」設為「`main`」。在下一章，CSS 會針對 `main` 元素進行設定。
1. 新增&#x200B;**影像**&#x200B;元件到&#x200B;**主要**&#x200B;容器。
   * 將&#x200B;**影像**&#x200B;元件解除鎖定。
1. 在主要容器中的&#x200B;**影像**&#x200B;元件之下，新增&#x200B;**階層連結**&#x200B;元件。
   * 為&#x200B;**階層連結**&#x200B;元件建立一個原則，名為「**文章頁面 - 階層連結**」。將「**導覽開始層級**」設為「**4**」。
1. 在&#x200B;**階層連結**&#x200B;元件之下以及在&#x200B;**主要**&#x200B;容器內，新增&#x200B;**容器**&#x200B;元件。此元件可以用作範本的&#x200B;**元件容器**。
   * 解除鎖定&#x200B;**內容**&#x200B;容器。
   * 將原則設定為「**頁面內容**」。
1. 在&#x200B;**內容容器**&#x200B;之下新增另一個&#x200B;**容器**&#x200B;元件。此元件可用作範本的&#x200B;**側邊欄**&#x200B;容器。
   * 解除鎖定&#x200B;**側邊欄**&#x200B;容器。
   * 建立一個原則，名為「**文章頁面 - 側邊欄**」。
   * 設定「**WKND 網站專案 - 內容**」之下的「**允許元件**」，以便包含：**按鈕**、**下載**、**影像**、**清單**、**分隔符號**、**社交媒體分享**、**文字**&#x200B;和&#x200B;**標題**。
1. 更新頁面根容器的原則。這是範本最外層的容器。將原則設定為「**頁面根目錄**」。
   * 在「**容器設定**」下，將「**版面**」設為「**回應式格線**」。
1. 啟動&#x200B;**內容容器**&#x200B;的版面模式。將控制點從右拖到左，並將容器縮小至八欄寬度。
1. 啟動&#x200B;**側邊欄容器**&#x200B;的版面模式。將控制點從右拖到左，並將容器縮小至四欄寬度。接著把左側控制點從左往右拖曳一欄，讓容器變成三欄寬度並與&#x200B;**內容容器**&#x200B;保留一欄空隙。
1. 開啟行動模擬器並切換到行動中斷點。再次啟動版面模式，讓&#x200B;**內容容器**&#x200B;和&#x200B;**側邊欄容器**&#x200B;變成全頁的寬度。這樣做會讓容器在行動中斷點垂直堆疊。
1. 更新&#x200B;**內容容器**&#x200B;中&#x200B;**文字**&#x200B;元件的原則。
   * 將原則設定為「**內容文字**」。
   * 在「**外掛程式** > **段落樣式**」下，選取「**啟用段落樣式**」並確保已啟用「**引述區塊**」。

### 初始內容設定

1. 切換到&#x200B;**初始內容**&#x200B;模式。
1. 將&#x200B;**標題**&#x200B;元件新增到&#x200B;**內容容器**。此元件可用作文章標題。若此元件留空，會自動顯示目前頁面的標題。
1. 在第一個標題元件下方新增第二個&#x200B;**標題**&#x200B;元件。
   * 使用文字「作者」來設定元件。這是文字預留位置。
   * 將類型設定為「`H4`」。
1. 在&#x200B;**作者**&#x200B;標題元件下方新增&#x200B;**文字**&#x200B;元件。
1. 將&#x200B;**標題**&#x200B;元件新增到&#x200B;**側邊欄容器**。
   * 使用文字「分享這個故事」來設定元件。
   * 將類型設定為「`H5`」。
1. 在&#x200B;**分享這個故事**&#x200B;標題元件下方，新增&#x200B;**社交媒體分享**&#x200B;元件。
1. 在&#x200B;**社交媒體分享**&#x200B;元件下方新增&#x200B;**分隔符號**&#x200B;元件。
1. 在&#x200B;**分隔符號**&#x200B;元件下方新增&#x200B;**下載**&#x200B;元件。
1. 在&#x200B;**下載**&#x200B;元件下方新增&#x200B;**清單**&#x200B;元件。
1. 更新範本的&#x200B;**初始頁面屬性**。
   * 在「**社交媒體** > **社交媒體分享**」之下，勾選「**Facebook**」和「**Pinterest**」

### 啟用範本並新增縮圖

1. 導覽至 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)，可在範本控制台中檢視範本
1. **啟用**&#x200B;文章頁面範本。
1. 編輯文章頁面範本的屬性並上傳以下縮圖，以便迅速確認使用文章頁面範本建立的頁面：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用體驗片段更新頁首和頁尾 {#experience-fragments}

在建立全域內容 (例如頁首或頁尾) 時，常見的做法是使用[體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=zh-Hant)。使用者可以利用體驗片段，將多個元件結合起來建立成單一可參考的元件。體驗片段的優點在於支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=zh-hant)。

AEM 專案原型已產生頁首和頁尾。接下來，更新體驗片段以和模型相符。請按照以下影片的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上述影片的概括性步驟：

1. 下載範例內容套件 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**。
1. 使用封裝管理員在 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp) 上傳並安裝內容套件
1. 更新網頁變化版本範本，這是在 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html) 的體驗片段使用的範本
   * 更新範本上&#x200B;**容器**&#x200B;元件的原則。
   * 將原則設定為「**XF Root**」。
   * 在「**允許的元件**」下，選取元件群組「**WKND 網站專案 - 結構**」，以便包含&#x200B;**語言導覽**、**導覽**&#x200B;和&#x200B;**快速搜尋**&#x200B;元件。

### 更新頁首體驗片段

1. 開啟在以下位置轉譯頁首的體驗片段：[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 設定片段的根&#x200B;**容器**。這是最外層的&#x200B;**容器**。
   * 將「**版面**」設為「**回應式格線**」
1. 在&#x200B;**容器**&#x200B;上方新增 **WKND 深色標誌**&#x200B;的影像。該標誌包含在上一步安裝的封裝中。
   * 將 **WKND 深色標誌**&#x200B;的版面修改為&#x200B;**兩**&#x200B;欄寬。將控制點從右往左拖曳。
   * 設定標誌的「**替代文字**」為「WKND 標誌」。
   * 設定標誌的「**連結**」至 `/content/wknd/us/en`，即首頁。
1. 設定已經放置在頁面上的&#x200B;**導覽**&#x200B;元件。
   * 將「**排除根層級**」設定為「**1**」。
   * 將「**導覽結構深度**」設定為「**1**」。
   * 將&#x200B;**導覽**&#x200B;元件的版面修改為&#x200B;**八**&#x200B;欄寬。將控制點從右往左拖曳。
1. 移除&#x200B;**語言導覽**&#x200B;元件。
1. 將&#x200B;**搜尋**&#x200B;元件的版面修改為&#x200B;**兩**&#x200B;欄寬。將控制點從右往左拖曳。現在所有元件應該在同一列上水平對齊。

### 更新頁尾體驗片段

1. 開啟在以下位置轉譯頁首的體驗片段：[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 設定片段的根&#x200B;**容器**。這是最外層的&#x200B;**容器**。
   * 將「**版面**」設為「**回應式格線**」
1. 在&#x200B;**容器**&#x200B;上方新增 **WKND 淺色標誌**&#x200B;的影像。該標誌包含在上一步安裝的封裝中。
   * 將 **WKND 淺色標誌**&#x200B;的版面修改為&#x200B;**兩**&#x200B;欄寬。將控制點從右往左拖曳。
   * 設定標誌的「**替代文字**」為「WKND 標誌淺色」。
   * 設定標誌的「**連結**」至 `/content/wknd/us/en`，即首頁。
1. 在標誌下方新增&#x200B;**導覽**&#x200B;元件。設定&#x200B;**導覽**&#x200B;元件：
   * 將「**排除根層級**」設定為「**1**」。
   * 取消勾選「**收集所有子頁面**」。
   * 將「**導覽結構深度**」設定為「**1**」。
   * 將&#x200B;**導覽**&#x200B;元件的版面修改為&#x200B;**八**&#x200B;欄寬。將控制點從右往左拖曳。

## 建立文章頁面

接下來，使用文章頁面範本建立一個頁面。編寫頁面內容以和網站模型相符。請按照以下影片的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上述影片的概括性步驟：

1. 導覽至 Sites 控制台，位於 [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)。
1. 在「**WKND** > **US** > **EN** > **雜誌**」之下建立一個頁面。
   * 選擇&#x200B;**文章頁面**&#x200B;範本。
   * 在「**屬性**」之下，將「**標題**」設為「洛杉磯滑板場終極指南」
   * 將「**名稱**」設定為「guide-la-skateparks」
1. 將「**作者**」標題用文字「Stacey Roswells 著」取代。
1. 更新&#x200B;**文字**&#x200B;元件以便包含可填入文章中的段落。您可以使用以下文字檔案當作文案：[ la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 新增另一個&#x200B;**文字**&#x200B;元件。
   * 更新元件以便包含引言：「洛杉磯是最適合滑行炫技的地方。」
   * 在全螢幕模式下編輯 RTF 編輯器並修改上述引言，以便使用&#x200B;**引述區塊**&#x200B;元素。
1. 繼續填入文章的正文以和模型相符。
1. 設定&#x200B;**下載**&#x200B;元件以便使用文章的 PDF 版本。
   * 在「**下載** > **屬性**」之下，按一下「**從 DAM 資產取得標題**」核取方塊。
   * 將「**說明**」設定為「閱讀全文報導」。
   * 將「**動作文字**」設定為「下載 PDF」。
1. 設定&#x200B;**清單**&#x200B;元件。
   * 在「**清單設定** > **清單建置基礎**」之下，選取「**子頁面**」。
   * 將「**主版頁面**」設為「`/content/wknd/us/en/magazine`」。
   * 在「**項目設定**」之下，勾選「**連結項目**」再勾選「**顯示日期**」。

## 檢查節點結構 {#node-structure}

在這個時候，文章頁面顯然未設定樣式。但是已經設定好基本結構。接下來，檢查文章頁面的節點結構，以便更加了解範本、頁面和元件的角色。

在本機 AEM 實例上使用 CRXDE-Lite 工具檢視基礎節點結構。

1. 開啟 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 並使用導覽樹狀結構導覽至 `/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下 `la-skateparks` 頁面下方的 `jcr:content` 節點並檢視屬性：

   ![JCR 內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   注意 `cq:template` 的值，其指向先前建立的文章頁面範本 `/conf/wknd/settings/wcm/templates/article-page`。

   亦請注意 `sling:resourceType`的值，指向 `wknd/components/page`。這是由 AEM 專案原型建立的頁面元件，負責根據範本轉譯頁面。

1. 展開 `jcr:content` 下方的 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 節點並檢視節點階層：

   ![JCR 內容洛杉磯滑板場](assets/pages-templates/page-jcr-structure.png)

   您應該可以大致將每個節點對應到所製作的元件。您可以檢查前置詞為 `container` 的節點，找出所使用的不同的版面容器。

1. 接下來檢查 `/apps/wknd/components/page` 之下的頁面元件。檢視 CRXDE Lite 中的元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   頁面元件下方只有兩個 HTL 指令碼，`customfooterlibs.html` 和 `customheaderlibs.html`。*這個元件是如何轉譯頁面？*

   `sling:resourceSuperType` 屬性指向 `core/wcm/components/page/v2/page`。此屬性允許 WKND 的頁面元件繼承&#x200B;**所有**&#x200B;核心元件頁面元件的功能。這是所謂的 [Proxy 元件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hant#ProxyComponentPattern)的第一個範例。如需更多資訊，請參閱[這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hant)。

1. 檢查 WKND 元件中的另一個元件，即來自 `/apps/wknd/components/breadcrumb` 的 `Breadcrumb` 元件。請注意，您可以找到相同的 `sling:resourceSuperType` 屬性，但這次其指向 `core/wcm/components/breadcrumb/v2/breadcrumb`。這是使用 Proxy 元件模式來包含核心元件的另一個範例。事實上，WKND 程式碼基底中所有元件都是 AEM 核心元件的 Proxy (自訂示範 HellowWorld 元件除外)。在編寫自訂程式碼&#x200B;*之前*，儘可能重複使用核心元件的功能，是最佳做法。

1. 接下來使用 CRXDE Lite 檢查位在 `/libs/core/wcm/components/page/v2/page` 的核心元件頁面：

   >[!NOTE]
   >
   > 在 AEM 6.5/6.4 中，核心元件位於 `/apps/core/wcm/components` 下。在 AEM as a Cloud Service 中，核心元件位於 `/libs` 之下並會自動更新。

   ![核心元件頁面](assets/pages-templates/core-page-component-properties.png)

   請注意，此頁面下方包含許多指令碼檔案。核心元件頁面包含許多功能。此功能會拆分成多個指令碼，使維護更輕鬆，可讀性也更高。開啟 `page.html` 並搜尋 `data-sly-include`，即可追蹤 HTL 指令碼的內容：

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

   將 HTL 拆分為多個指令碼的另一個原因，是為了讓 Proxy 元件覆寫個別指令碼以實施自訂商業邏輯。建立 HTL 指令碼 `customfooterlibs.html`和 `customheaderlibs.html` 具有明確的目的，就是要由實施中的專案覆寫。

   [閱讀這篇文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hant)，您便可以更加了解可編輯的範本如何影響內容頁面的轉譯。

1. 檢查另一個核心元件，例如位於 `/libs/core/wcm/components/breadcrumb/v2/breadcrumb` 的階層連結。檢視 `breadcrumb.html` 指令碼以了解最終如何產生階層連結元件的標記。

## 將設定儲存至原始碼控制 {#configuration-persistence}

通常，尤其是在 AEM 專案開始時，將設定 (例如範本和相關內容原則) 保留在原始碼控制中是非常重要的事。這樣可確保所有開發人員工作時均使用同一組內容和設定，並可提高不同環境之間的一致性。專案達到特定成熟度後，即可將管理範本的做法移交給特殊的進階使用者群組。


目前，範本視同其他程式碼片段，而且作為專案的一部分，將&#x200B;**文章頁面範本**同步下載下來。
到目前為止，程式碼已從 AEM 專案推送到 AEM 的本機實例。 **文章頁面範本**&#x200B;是直接在 AEM 本機實例上建立的，因此需要把範本&#x200B;**匯入**&#x200B;到 AEM 專案中。AEM 專案中包含 **ui.content** 模組便是為了此一特定目的。

接下來的幾個步驟會在 VSCode IDE 中使用 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview) 外掛程式來完成。不過這些步驟也可以在您設定好要從 AEM 本機實例中&#x200B;**匯入**&#x200B;內容的任何 IDE 中進行。

1. 在 VSCode 中開啟 `aem-guides-wknd` 專案。

1. 在專案總管中展開 **ui.content** 模組。展開 `src` 資料夾並導覽至 `/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 用右鍵按一下] `templates` 資料夾並選取「**從 AEM 伺服器匯入**」：

   ![VSCode 匯入範本](assets/pages-templates/vscode-import-templates.png)

   應該匯入 `article-page`，也應該更新 `page-content`、`xf-web-variation` 範本。

   ![已更新的範本](assets/pages-templates/updated-templates.png)

1. 重複上述步驟匯入內容，但從 `/conf/wknd/settings/wcm/policies` 中選取「**原則**」資料夾。

   ![VSCode 匯入原則](assets/pages-templates/policies-article-page-template.png)

1. 檢查來自 `filter.xml` 的 `ui.content/src/main/content/META-INF/vault/filter.xml` 檔案。

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

   `filter.xml` 檔案負責識別與封裝一起安裝的節點之路徑。請注意每個篩選器上的 `mode="merge"`，表示現有內容不會被修改，只會增加新內容。由於內容作者可能會更新這些路徑，因此務必確保程式碼部署&#x200B;**不會**&#x200B;覆寫內容。關於使用篩選器元素的詳細資訊，請參閱 [FileVault 文件](https://jackrabbit.apache.org/filevault/filter.html)。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 來了解每個模組管理的不同節點。

   >[!WARNING]
   >
   > 為了確保 WKND 參考網站的部署維持一致性，一些專案分支會設定為 `ui.content` 會覆寫 JCR 中的任何變更。例如 Solution Branches 的原始設計便是如此，因為程式碼或樣式是針對特定原則而編寫的。

## 恭喜！ {#congratulations}

恭喜，您已使用 Adobe Experience Manager Sites 建立新範本和頁面。

### 後續步驟 {#next-steps}

在這個時候，文章頁面顯然未設定樣式。依照[用戶端程式庫與前端工作流程](client-side-libraries.md)教學課程一起操作，了解包含 CSS 和 JavaScript 以將全域樣式套用至網站並與專屬的前端建置整合的最佳做法。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上檢視已完成的程式碼，或在 Git 分支 `tutorial/pages-templates-solution` 上本機檢閱與部署程式碼。

1. 原地複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/pages-templates-solution` 分支。
