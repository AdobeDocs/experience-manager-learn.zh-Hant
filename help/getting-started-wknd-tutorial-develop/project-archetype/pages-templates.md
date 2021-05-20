---
title: AEM Sites快速入門 — 頁面和範本
seo-title: AEM Sites快速入門 — 頁面和範本
description: 了解基本頁面元件與可編輯範本之間的關係。 了解核心元件如何改寫至專案，並了解可編輯範本的進階政策設定，以根據Adobe XD的模型建立結構良好的文章頁面範本。
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心元件、可編輯的範本、頁面編輯器
topic: 內容管理、開發
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3106'
ht-degree: 0%

---


# 頁面和範本{#pages-and-template}

本章將探討基本頁面元件與可編輯範本之間的關係。 我們會根據[AdobeXD](https://www.adobe.com/products/xd.html)中的某些模型，建立未設定樣式的文章範本。 在建立範本的過程中，將介紹可編輯模板的核心元件和高級策略配置。

## 必備條件 {#prerequisites}

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/pages-templates-start`分支

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
   > 如果使用AEM 6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)上檢視完成的程式碼，或切換至分支`tutorial/pages-templates-solution`在本機檢出程式碼。

## 目標

1. Inspect是在Adobe XD中建立的頁面設計，並對應至核心元件。
1. 了解可編輯範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 了解範本和頁面的連結方式

## 您要建立的{#what-you-will-build}

在本教學課程的本節中，您將建立新的「文章頁面範本」，以用來建立新的文章頁面，並與通用結構對齊。 「文章頁面範本」將以AdobeXD中製作的設計和UI套件為基礎。 本章僅側重於構建模板的結構或骨架。 不會實作樣式，但範本和頁面將可運作。

![文章頁面設計與未設定樣式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD規劃UI {#adobexd}

在大多數情況下，規劃新網站的開始會是模型和靜態設計。 [Adobe](https://www.adobe.com/products/xd.html) XD是建立使用者體驗的設計工具。接下來，我們將檢查UI套件和模型，以協助規劃文章頁面範本的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下載WKND [文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)也提供通用[AEM核心元件UI套件作為自訂專案的起點。

## 建立文章頁面範本

建立頁面時，您必須選取範本，以作為建立新頁面的基礎。 範本會定義產生的頁面、初始內容及允許的元件結構。

[可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)有3個主要區域：

1. **結構**  — 定義屬於範本一部分的元件。內容作者將無法編輯這些內容。
1. **初始內容**  — 定義範本要開頭的元件，內容作者可以編輯和/或刪除這些元件
1. **原則**  — 定義元件行為設定，以及作者可使用的選項。

接下來，在AEM中建立符合模型結構的新範本。 這會發生在AEM的本機例項中。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

以下是影片的高階步驟：

### 結構配置

1. 使用&#x200B;**頁面範本類型**&#x200B;建立新範本，名為&#x200B;**文章頁面**。
1. 切換到&#x200B;**Structure**&#x200B;模式。
1. 新增&#x200B;**體驗片段**&#x200B;元件，作為範本頂端的&#x200B;**標題**。
   * 配置元件以指向`/content/experience-fragments/wknd/us/en/site/header/master`。
   * 將策略設定為&#x200B;**Page Header**，並確保將&#x200B;**Default Element**&#x200B;設定為`header`。 在下一章中，`header`元素將會以CSS定位。
1. 新增&#x200B;**體驗片段**&#x200B;元件，作為範本底部的&#x200B;**頁尾**。
   * 配置元件以指向`/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 將策略設定為&#x200B;**頁尾**，並確保&#x200B;**預設元素**&#x200B;設定為`footer`。 在下一章中，`footer`元素將會以CSS定位。
1. 鎖定最初建立範本時包含的&#x200B;**main**&#x200B;容器。
   * 將策略設定為&#x200B;**Page Main**，並確保將&#x200B;**Default Element**&#x200B;設定為`main`。 在下一章中，`main`元素將會以CSS定位。
1. 將&#x200B;**Image**&#x200B;元件新增至&#x200B;**main**&#x200B;容器。
   * 解鎖&#x200B;**Image**&#x200B;元件。
1. 在主容器的&#x200B;**Image**&#x200B;元件下方新增&#x200B;**階層連結**&#x200B;元件。
   * 為&#x200B;**Breadcrumb**&#x200B;元件建立名為&#x200B;**Article Page - Breadcrumb**&#x200B;的新策略。 將&#x200B;**導航起始級別**&#x200B;設定為&#x200B;**4**。
1. 在&#x200B;**Breadcrumb**&#x200B;元件下方和&#x200B;**main**&#x200B;容器內新增&#x200B;**容器**&#x200B;元件。 這會做為範本的&#x200B;**內容容器**。
   * 解鎖&#x200B;**Content**&#x200B;容器。
   * 將原則設為&#x200B;**Page Content**。
1. 在&#x200B;**內容容器**&#x200B;下添加另一個&#x200B;**容器**&#x200B;元件。 這會做為範本的&#x200B;**側邊欄**&#x200B;容器。
   * 解鎖&#x200B;**側邊欄**&#x200B;容器。
   * 建立名為&#x200B;**文章頁面 — 側邊欄**&#x200B;的新原則。
   * 在&#x200B;**WKND Sites Project - Content**&#x200B;下配置&#x200B;**允許的元件**&#x200B;以包括：**Button**、**Download**、**Image**、**List**、**Separator**、**社交媒體共用**、**Text**&#x200B;和&lt;a18/&lt;A19/>。****
1. 更新「頁面根」容器的原則。 這是範本上最外層的容器。 將策略設定為&#x200B;**Page Root**。
   * 在「**容器設定**」下，將「**版面**」設定為「**回應式格線**」。
1. 參與&#x200B;**內容容器**&#x200B;的佈局模式。 從右到左拖動手柄，並將容器縮小為8列寬。
1. 參與&#x200B;**側邊欄容器**&#x200B;的佈局模式。 從右到左拖動手柄，並將容器縮小為4列寬。 然後，從左到右拖動左手柄1列，使容器3列變寬，並在&#x200B;**內容容器**&#x200B;之間留有1列間隙。
1. 開啟移動模擬器並切換到移動斷點。 再次參與版面模式，並將&#x200B;**內容容器**&#x200B;和&#x200B;**側邊欄容器**&#x200B;設為頁面的全寬。 這會在行動斷點中垂直堆疊容器。
1. 更新&#x200B;**內容容器**&#x200B;中&#x200B;**Text**&#x200B;元件的策略。
   * 將策略設定為&#x200B;**內容文本**。
   * 在&#x200B;**Plugins** > **段落樣式**&#x200B;下，檢查&#x200B;**啟用段落樣式**&#x200B;並確保&#x200B;**引號塊**&#x200B;已啟用。

### 初始內容設定

1. 切換至&#x200B;**初始內容**&#x200B;模式。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**內容容器**。 這會做為文章標題。 當保留為空白時，會自動顯示目前頁面的標題。
1. 在第一個標題元件下添加第二個&#x200B;**標題**&#x200B;元件。
   * 使用文字設定元件：「作者」。 這將是文字預留位置。
   * 將類型設定為`H4`。
1. 在&#x200B;**By Author**&#x200B;標題元件下方新增&#x200B;**Text**&#x200B;元件。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**側欄容器**。
   * 使用文字設定元件：「分享此故事」。
   * 將類型設定為`H5`。
1. 在&#x200B;**共用此動態**&#x200B;標題元件下方新增&#x200B;**社交媒體共用**&#x200B;元件。
1. 在&#x200B;**社交媒體共用**&#x200B;元件下方新增&#x200B;**分隔符號**&#x200B;元件。
1. 在&#x200B;**Separator**&#x200B;元件下方新增&#x200B;**Download**&#x200B;元件。
1. 在&#x200B;**Download**&#x200B;元件下添加&#x200B;**List**&#x200B;元件。
1. 更新範本的&#x200B;**初始頁面屬性**。
   * 在「**社交媒體**」>「**社交媒體共用**」下，檢查&#x200B;**Facebook**&#x200B;和&#x200B;**Pinterest**

### 啟用範本並新增縮圖

1. 導覽至[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)，在範本主控台中檢視範本
1. **** 啟用「文章頁面」範本。
1. 編輯「文章頁面」範本的屬性並上傳下列縮圖，以快速識別使用「文章頁面」範本建立的頁面：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用體驗片段{#experience-fragments}更新頁首和頁尾

建立全域內容（例如頁首或頁尾）時，常見的作法是使用[體驗片段](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 體驗片段，可讓使用者結合多個元件，以建立單一、可參考的元件。 體驗片段的優點是支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

AEM專案原型會產生頁首和頁尾。 接下來，更新體驗片段以符合模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

以下是影片的高階步驟：

1. 下載範例內容套件&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**。
1. 使用套件管理器，在[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)上傳和安裝內容套件
1. 更新網頁變異範本，此範本是用於[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)的體驗片段的範本
   * 更新範本上的&#x200B;**容器**&#x200B;元件。
   * 將策略設定為&#x200B;**XF根**。
   * 在「**允許的元件**」下，選擇元件組&#x200B;**WKND Sites項目 — 結構**&#x200B;以包括&#x200B;**語言導航**、**導航**&#x200B;和&#x200B;**快速搜索**&#x200B;元件。

### 更新標題體驗片段

1. 開啟在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)轉譯標題的體驗片段
1. 設定片段的根&#x200B;**容器**。 這是最外面的&#x200B;**容器**。
   * 將&#x200B;**Layout**&#x200B;設定為&#x200B;**回應式格線**
1. 將&#x200B;**WKND深色標誌**&#x200B;添加為影像到&#x200B;**容器**&#x200B;的頂部。 此標誌包含在先前步驟中安裝的套件中。
   * 將&#x200B;**WKND Dark Logo**&#x200B;的佈局修改為寬&#x200B;**2**&#x200B;列。 從右到左拖動控點。
   * 使用「WKND Logo」的&#x200B;**替代文字**&#x200B;來設定標誌。
   * 將徽標配置為首頁的&#x200B;**連結**&#x200B;到`/content/wknd/us/en`。
1. 配置已放在頁面上的&#x200B;**Navigation**&#x200B;元件。
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**導航結構深度**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的佈局修改為寬&#x200B;**8**&#x200B;列。 從右到左拖動控點。
1. 移除&#x200B;**語言導覽**&#x200B;元件。
1. 將&#x200B;**Search**&#x200B;元件的佈局修改為寬&#x200B;**2**&#x200B;列。 從右到左拖動控點。 現在，所有元件應在單一列上水準對齊。

### 更新頁尾體驗片段

1. 開啟在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)轉譯頁尾的體驗片段
1. 設定片段的根&#x200B;**容器**。 這是最外面的&#x200B;**容器**。
   * 將&#x200B;**Layout**&#x200B;設定為&#x200B;**回應式格線**
1. 將&#x200B;**WKND Light Logo**&#x200B;添加為影像到&#x200B;**Container**&#x200B;的頂部。 此標誌包含在先前步驟中安裝的套件中。
   * 將&#x200B;**WKND Light Logo**&#x200B;的佈局修改為寬&#x200B;**2**&#x200B;列。 從右到左拖動控點。
   * 使用「WKND Logo Light」的&#x200B;**替代文字**&#x200B;設定標誌。
   * 將徽標配置為首頁的&#x200B;**連結**&#x200B;到`/content/wknd/us/en`。
1. 在標誌下方新增&#x200B;**Navigation**&#x200B;元件。 配置&#x200B;**Navigation**&#x200B;元件：
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**收集所有子頁**。
   * 將&#x200B;**導航結構深度**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的佈局修改為寬&#x200B;**8**&#x200B;列。 從右到左拖動控點。

## 建立文章頁面

接下來，使用「文章頁面」範本建立新頁面。 製作頁面內容以符合網站模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

以下是影片的高階步驟：

1. 導覽至Sites主控台，網址為[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)。
1. 在&#x200B;**WKND** > **US** > **EN** > **Magazine**&#x200B;下建立新頁面。
   * 選擇&#x200B;**文章頁面**&#x200B;範本。
   * 在&#x200B;**Properties**&#x200B;下，將&#x200B;**Title**&#x200B;設定為「LA Skateparks的Ultimate Guide」
   * 將&#x200B;**Name**&#x200B;設定為&quot;guide-la-skateparks&quot;
1. 將&#x200B;**By Author**&#x200B;標題替換為文字「By Stacey Roswells」。
1. 更新&#x200B;**Text**&#x200B;元件以包含要填入文章的段落。 您可以使用下列文字檔案作為副本：[la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加另一個&#x200B;**Text**&#x200B;元件。
   * 更新元件以包括報價：「沒有比這更好的地方去粉碎洛杉磯。」
   * 以全螢幕模式編輯RTF編輯器，並修改上述引號以使用&#x200B;**Quote Block**&#x200B;元素。
1. 繼續填入文章內文，以符合模型。
1. 設定&#x200B;**Download**&#x200B;元件以使用文章的PDF版本。
   * 在「**下載** > **屬性**」下，按一下「從DAM資產&#x200B;**取得標題」核取方塊。**
   * 將&#x200B;**Description**&#x200B;設定為：「全文」。
   * 將&#x200B;**動作文字**&#x200B;設為：「下載PDF」。
1. 配置&#x200B;**List**&#x200B;元件。
   * 在「**清單設定** > **使用**&#x200B;建立清單」下，選擇「**子頁**」。
   * 將&#x200B;**上層頁面**&#x200B;設為`/content/wknd/us/en/magazine`。
   * 在「**項目設定**」下，檢查「**連結項目**」並檢查「**顯示日期**」。

## Inspect節點結構{#node-structure}

此時，文章頁面的樣式明顯不同。 但基本結構已經到位。 接下來，檢查文章頁面的節點結構，以便更好地了解範本、頁面和元件的角色。

在本機AEM例項上使用CRXDE-Lite工具來檢視基礎節點結構。

1. 開啟[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)並使用樹導航導航導航到`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下`la-skateparks`頁面下方的`jcr:content`節點，並檢視屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   請注意`cq:template`的值，此值指向先前建立的`/conf/wknd/settings/wcm/templates/article-page`文章頁面範本。

   另請注意`sling:resourceType`的值，它指向`wknd/components/page`。 這是由AEM專案原型建立的頁面元件，負責根據範本轉譯頁面。

1. 展開`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`下方的`jcr:content`節點，並檢視節點階層：

   ![JCR內容LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   您應能將每個節點鬆散地對應至已撰寫的元件。 查看您是否可以識別透過檢查帶有`container`前置詞的節點所使用的不同版面容器。

1. 接下來，在`/apps/wknd/components/page`檢查頁面元件。 在CRXDE Lite中查看元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   請注意，頁面元件下方只有2個HTL指令碼，分別為`customfooterlibs.html`和`customheaderlibs.html`。 *那麼，此元件如何呈現頁面？*

   `sling:resourceSuperType`屬性指向`core/wcm/components/page/v2/page`。 此屬性可讓WKND的頁面元件繼承核心元件頁面元件的&#x200B;**all**&#x200B;功能。 這是[代理元件模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)的第一個示例。 如需詳細資訊，請參閱[此處。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html)。

1. Inspect WKND元件中的另一個元件（`Breadcrumb`元件）位於：`/apps/wknd/components/breadcrumb`。 請注意，可以找到相同的`sling:resourceSuperType`屬性，但這次它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 這是使用Proxy元件模式來包含核心元件的另一個範例。 事實上，WKND程式碼基底中的所有元件都是AEM核心元件的代理（我們著名的HelloWorld元件除外）。 在&#x200B;*編寫自訂程式碼之前，最佳作法是盡量*&#x200B;重複使用核心元件的功能。

1. 接下來，使用CRXDE Lite在`/libs/core/wcm/components/page/v2/page`檢查核心元件頁面：

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心元件位於`/apps/core/wcm/components`下。 在AEM as aCloud Service中，核心元件位於`/libs`下，並會自動更新。

   ![核心元件頁面](assets/pages-templates/core-page-component-properties.png)

   請注意，此頁面下方還包含更多指令碼。 核心元件頁面包含許多功能。 此功能已細分為多個指令碼，以方便維護及閱讀。 您可以開啟`page.html`並尋找`data-sly-include`來追蹤包含的HTL指令碼：

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

   將HTL分割為多個指令碼的另一個原因是，可允許Proxy元件覆寫個別指令碼，以實作自訂業務邏輯。 HTL指令碼`customfooterlibs.html`和`customheaderlibs.html`的建立目的，是為了透過實作專案來覆寫明確的目的。

   您可以閱讀本文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)，進一步了解可編輯範本如何影響[內容頁面的轉譯。

1. Inspect是另一個核心元件，例如位於`/libs/core/wcm/components/breadcrumb/v2/breadcrumb`的階層連結。 檢視`breadcrumb.html`指令碼，了解如何最終產生階層連結元件的標籤。

## 將配置保存到原始碼控制項{#configuration-persistence}

在許多情況下，尤其是在AEM專案開始時，最好將設定（例如範本和相關內容原則）保留到原始碼控制。 這可確保所有開發人員針對相同的內容和設定組合工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟程度，管理模板的做法就可以交給特定的一組電源用戶。

目前，我們將像處理其他程式碼一樣處理範本，並將&#x200B;**文章頁面範本**&#x200B;向下同步為專案的一部分。 直到目前為止，我們的AEM專案中都有&#x200B;**推送**&#x200B;程式碼，會傳送至AEM的本機例項。 **文章頁面範本**&#x200B;是直接在AEM的本機例項上建立，因此我們需要將範本&#x200B;**匯入**&#x200B;至我們的AEM專案。 針對此特定用途，AEM專案中包含&#x200B;**ui.content**&#x200B;模組。

接下來的幾個步驟將使用VSCode IDE進行，該IDE使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)插件，但可能使用配置為&#x200B;**import**&#x200B;的任何IDE執行，或從AEM的本地實例導入內容。

1. 在VSCode中，開啟`aem-guides-wknd`專案。

1. 展開「專案總管」中的&#x200B;**ui.content**&#x200B;模組。 展開`src`資料夾並導覽至`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 按一下右] 鍵資料 `templates` 夾，然後選 **取「從AEM伺服器匯入」**:

   ![VSCode匯入範本](assets/pages-templates/vscode-import-templates.png)

   應匯入`article-page`，也應更新`page-content`、`xf-web-variation`範本。

   ![更新的範本](assets/pages-templates/updated-templates.png)

1. 重複匯入內容的步驟，但選取位於`/conf/wknd/settings/wcm/policies`的&#x200B;**policys**&#x200B;資料夾。

   ![VSCode導入策略](assets/pages-templates/policies-article-page-template.png)

1. Inspect位於`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`檔案。

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

   `filter.xml`檔案負責標識將隨軟體包一起安裝的節點的路徑。 請注意每個篩選器上的`mode="merge"` ，指出不會修改現有內容，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此程式碼部署必須&#x200B;**不**&#x200B;覆寫內容。 有關使用篩選元素的詳細資訊，請參閱[FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html)。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每個模組所管理的不同節點。

   >[!WARNING]
   >
   > 為了確保WKND參考站點的一致部署，已設定項目的某些分支，使`ui.content`將覆蓋JCR中的任何更改。 這是根據設計（即解決方案分支），因為將針對特定策略編寫代碼/樣式。

## 恭喜！ {#congratulations}

恭喜，您剛使用Adobe Experience Manager Sites建立新範本和頁面。

### 後續步驟{#next-steps}

此時，文章頁面的樣式明顯不同。 請依照[用戶端程式庫和前端工作流程](client-side-libraries.md)教學課程，了解包含CSS和Javascript的最佳實務，以將全域樣式套用至網站並整合專用的前端組建。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git列`tutorial/pages-templates-solution`上檢閱並將程式碼部署於本機。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`tutorial/pages-templates-solution`分支。
