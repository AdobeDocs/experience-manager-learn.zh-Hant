---
title: 開始使用AEM Sites-頁面和範本
seo-title: 開始使用AEM Sites-頁面和範本
description: 瞭解基本頁面元件與可編輯範本之間的關係。 瞭解核心元件如何加入專案，並瞭解可編輯範本的進階政策設定，以根據Adobe XD的模型建立結構良好的文章頁面範本。
sub-product: Sites
feature: 核心元件、可編輯範本
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
topic: 內容管理、開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: b5b43ae8231bf23e0c53777b1e9c16bcc3fc188a
workflow-type: tm+mt
source-wordcount: '3106'
ht-degree: 0%

---


# 頁面和範本{#pages-and-template}

在本章中，我們將探討基本頁面元件與可編輯模板之間的關係。 我們將根據[AdobeXD](https://www.adobe.com/products/xd.html)中的部分模型，建立未設定樣式的文章範本。 在建立範本的過程中，會涵蓋「可編輯範本」的核心元件和進階原則組態。

## 必備條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### Starter Project

>[!NOTE]
>
> 如果您成功完成上一章，可以重新使用項目，並跳過簽出起始項目的步驟。

查看教學課程所建立的基線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/pages-templates-start`分支

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. 使用您的Maven技巧，將程式碼AEM庫部署至本機執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)上檢視完成的程式碼，或切換至分支`tutorial/pages-templates-solution`，在本機檢出程式碼。

## 目標

1. Inspect在Adobe XD建立的頁面設計，並對應至核心元件。
1. 瞭解可編輯範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 瞭解範本和頁面的連結方式

## 您將建立的{#what-you-will-build}

在本教學課程中，您將建立新的「文章頁面範本」，可用來建立新文章頁面，並與一般結構對齊。 「文章頁面範本」將以AdobeXD中的設計和UI套件為基礎。 本章僅針對構建模板的結構或骨架進行介紹。 不會實作任何樣式，但範本和頁面都能運作。

![文章頁面設計與未設定樣式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD{#adobexd}進行UI規劃

在大多數情況下，規劃新網站都從模型和靜態設計開始。 [Adobe](https://www.adobe.com/products/xd.html) XD是建立使用體驗的設計工具。接下來，我們將檢查UI套件和模型，以協助規劃「文章頁面範本」的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下載 [WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> [核心元AEM件UI套件也提供](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)做為自訂專案的起點。

## 建立文章頁面範本

建立頁面時，您必須選取範本，此範本將用作建立新頁面的基礎。 範本會定義產生頁面的結構、初始內容和允許的元件。

[可編輯範本](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)有3個主要區域：

1. **結構** -定義屬於模板一部分的元件。內容作者將無法編輯這些內容。
1. **初始內容** -定義範本將開頭的元件，內容作者可編輯和／或刪除這些元件
1. **Policys**  —— 定義元件的行為方式以及作者將提供哪些選項的配置。

接著，在中建立符合AEM模型結構的新範本。 這將發生在的本地實例AEM中。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

以下視訊的高階步驟：

### 結構配置

1. 使用&#x200B;**頁面範本類型**&#x200B;建立新範本，名為&#x200B;**文章頁面**。
1. 切換到&#x200B;**Structure**&#x200B;模式。
1. 新增&#x200B;**體驗片段**&#x200B;元件，作為範本頂端的&#x200B;**標題**。
   * 將元件配置為指向`/content/experience-fragments/wknd/us/en/site/header/master`。
   * 將策略設定為&#x200B;**頁首**&#x200B;並確保&#x200B;**預設元素**&#x200B;設定為`header`。 在下一章中，`header`元素將會以CSS定位。
1. 新增&#x200B;**體驗片段**&#x200B;元件，作為範本底部的&#x200B;**頁尾**。
   * 將元件配置為指向`/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 將策略設定為&#x200B;**頁尾**&#x200B;並確保&#x200B;**預設元素**&#x200B;設定為`footer`。 在下一章中，`footer`元素將會以CSS定位。
1. 鎖定初次建立範本時包含的&#x200B;**main**&#x200B;容器。
   * 將策略設定為&#x200B;**Page Main**，並確保&#x200B;**Default Element**&#x200B;設定為`main`。 在下一章中，`main`元素將會以CSS定位。
1. 將&#x200B;**Image**&#x200B;元件新增至&#x200B;**main**&#x200B;容器。
   * 解鎖&#x200B;**Image**&#x200B;元件。
1. 在主容器的&#x200B;**Image**&#x200B;元件下方新增&#x200B;**Breadcrumb**&#x200B;元件。
   * 為名為&#x200B;**文章頁面——階層連結**&#x200B;的&#x200B;**階層連結**&#x200B;元件建立新原則。 將&#x200B;**導航起始級別**&#x200B;設定為&#x200B;**4**。
1. 在&#x200B;**Breadcrumb**&#x200B;元件下方和&#x200B;**main**&#x200B;容器內新增&#x200B;**Container**&#x200B;元件。 這將作為範本的&#x200B;**內容容器**。
   * 解除鎖定&#x200B;**Content**&#x200B;容器。
   * 將策略設定為&#x200B;**頁面內容**。
1. 在&#x200B;**內容容器**&#x200B;下方新增另一個&#x200B;**容器**&#x200B;元件。 這將充當範本的&#x200B;**側欄**&#x200B;容器。
   * 解鎖&#x200B;**側軌**&#x200B;容器。
   * 建立名為&#x200B;**文章頁面——側邊欄**&#x200B;的新原則。
   * 將&#x200B;**** WKND Sites Project - Content **下的允許的元件**&#x200B;配置為包括：**Button**、**下載**、**Image**、**List**、**Separator**、**社交媒體分享**、**Text**&#x200B;和&#x200B;**Title**。
1. 更新「頁面根」容器的原則。 這是範本上最外層的容器。 將策略設定為&#x200B;**Page Root**。
   * 在「**容器設定**」下方，將「**版面配置**」設為「回應式格線&#x200B;**」。**
1. 參與&#x200B;**內容容器**&#x200B;的版面模式。 從右到左拖曳控點，將容器縮小為8欄寬。
1. 接合&#x200B;**側軌容器**&#x200B;的版面模式。 從右到左拖曳控點，將容器縮小為4欄寬。 然後，將左控點從左至右1欄拖曳至容器3欄，使容器3欄變寬，並在&#x200B;**內容容器**&#x200B;之間留有1欄間隙。
1. 開啟行動模擬器並切換至行動中斷點。 再次接合版面模式，使&#x200B;**內容容器**&#x200B;和&#x200B;**側欄容器**&#x200B;成為頁面的全寬。 這會在行動中斷點中垂直堆疊容器。
1. 更新&#x200B;**內容容器**&#x200B;中&#x200B;**Text**&#x200B;元件的原則。
   * 將策略設定為&#x200B;**內容文本**。
   * 在&#x200B;**Plugins** > **段落樣式**&#x200B;下，選中&#x200B;**啟用段落樣式**&#x200B;並確保&#x200B;**引號塊**&#x200B;已啟用。

### 初始內容設定

1. 切換到&#x200B;**初始內容**&#x200B;模式。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**內容容器**。 這會當做文章標題。 如果保留為空白，則會自動顯示目前頁面的「標題」。
1. 在第一個標題元件下方添加第二個&#x200B;**標題**&#x200B;元件。
   * 使用文字設定元件：「作者」。 此為文字預留位置。
   * 將類型設定為`H4`。
1. 在&#x200B;**By Author** Title元件下添加&#x200B;**Text**&#x200B;元件。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**側軌容器**。
   * 使用文字設定元件：「分享這個故事」。
   * 將類型設定為`H5`。
1. 在&#x200B;**分享此動態**&#x200B;標題元件下方新增&#x200B;**社交媒體分享**&#x200B;元件。
1. 在&#x200B;**社交媒體分享**&#x200B;元件下方新增&#x200B;**分隔符號**&#x200B;元件。
1. 在&#x200B;**Separator**&#x200B;元件下方新增&#x200B;**Download**&#x200B;元件。
1. 在&#x200B;**Download**&#x200B;元件下方新增&#x200B;**List**&#x200B;元件。
1. 更新範本的&#x200B;**初始頁面屬性**。
   * 在「社交媒體&#x200B;**** > **社交媒體分享**」下，勾選「Facebook **」和「Pinterest**」****

### 啟用範本並新增縮圖

1. 導覽至[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)以在範本主控台中檢視範本
1. **啟** 用「文章頁面」範本。
1. 編輯「文章頁面」範本的屬性並上傳下列縮圖，以快速識別使用「文章頁面」範本建立的頁面：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用體驗片段{#experience-fragments}更新頁首和頁尾

建立全域內容（例如頁首或頁尾）時的常見做法是使用「體驗片段」[。 ](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)體驗片段，可讓使用者結合多個元件，以建立單一、可參考的元件。 體驗片段的優點是支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

「項AEM目原型」生成頁眉和頁腳。 接著，更新「體驗片段」以符合模型。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

以下視訊的高階步驟：

1. 下載範例內容套件&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**。
1. 使用位於[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的Package Manager上傳及安裝內容套件
1. 更新「Web變數」範本，此範本是[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)中用於體驗片段的範本
   * 更新範本上的&#x200B;**Container**&#x200B;元件原則。
   * 將策略設定為&#x200B;**XF Root**。
   * 在&#x200B;**允許的元件**&#x200B;下，選擇元件組&#x200B;**WKND站點項目——結構**&#x200B;以包含&#x200B;**語言導航**、**導航**&#x200B;和&#x200B;**快速搜索**&#x200B;元件。

### 更新標題體驗片段

1. 開啟在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)轉譯標題的體驗片段
1. 配置片段的根&#x200B;**Container**。 這是最外部&#x200B;**Container**。
   * 將&#x200B;**Layout**&#x200B;設為&#x200B;**回應式格線**
1. 將&#x200B;**WKND Dark Logo**&#x200B;作為影像添加到&#x200B;**Container**&#x200B;的頂部。 標誌包含在前一步驟中安裝的軟體包中。
   * 將&#x200B;**WKND Dark Logo**&#x200B;的版面修改為&#x200B;**2**&#x200B;欄寬。 從右到左拖動控點。
   * 使用「WKND Logo」的&#x200B;**Alternative Text**&#x200B;配置標誌。
   * 將標誌配置為首頁的&#x200B;**Link**&#x200B;到`/content/wknd/us/en`。
1. 配置已放置在頁面上的&#x200B;**Navigation**&#x200B;元件。
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**導覽結構深度**&#x200B;設為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的佈局修改為&#x200B;**8**&#x200B;欄寬。 從右到左拖動控點。
1. 刪除&#x200B;**Language Navigation**&#x200B;元件。
1. 將&#x200B;**Search**&#x200B;元件的版面配置修改為&#x200B;**2**&#x200B;欄寬。 從右到左拖動控點。 現在，所有元件都應在單一列上水準對齊。

### 更新頁尾體驗片段

1. 開啟轉譯頁尾於[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)的體驗片段
1. 配置片段的根&#x200B;**Container**。 這是最外部&#x200B;**Container**。
   * 將&#x200B;**Layout**&#x200B;設為&#x200B;**回應式格線**
1. 將&#x200B;**WKND Light Logo**&#x200B;作為影像添加到&#x200B;**Container**&#x200B;的頂部。 標誌包含在前一步驟中安裝的軟體包中。
   * 將&#x200B;**WKND Light Logo**&#x200B;的版面修改為&#x200B;**2**&#x200B;欄寬。 從右到左拖動控點。
   * 使用「WKND Logo Light」的「Alternative Text **」（替代文字）配置標誌。**
   * 將標誌配置為首頁的&#x200B;**Link**&#x200B;到`/content/wknd/us/en`。
1. 將&#x200B;**Navigation**&#x200B;元件加入標誌下方。 配置&#x200B;**Navigation**&#x200B;元件：
   * 將&#x200B;**排除根級別**&#x200B;設定為&#x200B;**1**。
   * 取消選中&#x200B;**收集所有子頁**。
   * 將&#x200B;**導覽結構深度**&#x200B;設為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的佈局修改為&#x200B;**8**&#x200B;欄寬。 從右到左拖動控點。

## 建立文章頁面

接著，使用「文章頁面」範本建立新頁面。 製作頁面內容以符合網站模型。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

以下視訊的高階步驟：

1. 導覽至位於[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)的網站主控台。
1. 在&#x200B;**WKND** > **US** > **EN** > **Magazine**&#x200B;下方建立新頁面。
   * 選擇&#x200B;**文章頁面**&#x200B;範本。
   * 在&#x200B;**Properties**&#x200B;下，將&#x200B;**Title**&#x200B;設為&quot;Ultimate Guide to LA Skateparks&quot;
   * 將&#x200B;**Name**&#x200B;設為&quot;guide-la-skateparks&quot;
1. 將&#x200B;**By Author**&#x200B;標題取代為&quot;By Stacey Roswells&quot;文字。
1. 更新&#x200B;**Text**&#x200B;元件以包含要填入文章的段落。 您可以使用下列文字檔案做為副本：[la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加另一個&#x200B;**Text**&#x200B;元件。
   * 更新元件以包括報價：「沒有比洛杉磯更好的地方可以粉碎。」
   * 以全螢幕模式編輯Rich Text Editor並修改上述引號，以使用&#x200B;**Quote Block**&#x200B;元素。
1. 繼續填入文章的正文以符合模型。
1. 設定&#x200B;**Download**&#x200B;元件以使用文章的PDF版本。
   * 在「**下載** > **屬性**」下方，按一下「從DAM資產&#x200B;**取得標題」核取方塊。**
   * 將&#x200B;**Description**&#x200B;設定為：「取得完整故事」。
   * 將&#x200B;**操作文本**&#x200B;設定為：「下載PDF」。
1. 配置&#x200B;**List**&#x200B;元件。
   * 在&#x200B;**清單設定** > **使用**&#x200B;構建清單下，選擇&#x200B;**子頁**。
   * 將&#x200B;**父頁**&#x200B;設定為`/content/wknd/us/en/magazine`。
   * 在&#x200B;**項目設定**&#x200B;下，選中&#x200B;**連結項目**&#x200B;並選中&#x200B;**顯示日期**。

## Inspect節點結構{#node-structure}

此時，文章頁面顯然沒有樣式。 但是，基本結構已經到位。 接著，檢查文章頁面的節點結構，以更好地瞭解範本、頁面和元件的角色。

在本地實例上使用CRXDE-Lite工AEM具查看基礎節點結構。

1. 開啟[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)，然後使用樹狀導覽導覽至`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下`la-skateparks`頁面下的`jcr:content`節點並查看屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   請注意`cq:template`的值，此值指向我們先前建立的「文章頁面範本」(`/conf/wknd/settings/wcm/templates/article-page`)。

   另請注意`sling:resourceType`的值，它指向`wknd/components/page`。 這是由專案原型建立的頁面元AEM件，負責根據範本來呈現頁面。

1. 展開`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`下的`jcr:content`節點，並查看節點層次結構：

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   您應該能夠將每個節點鬆散地映射到已編寫的元件。 查看您是否可以透過檢查前置有`container`的節點來識別使用的不同配置容器。

1. 接下來檢查位於`/apps/wknd/components/page`的頁面元件。 在CRXDE Lite中查看元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   請注意，頁面元件下方只有2個HTL指令碼，`customfooterlibs.html`和`customheaderlibs.html`。 *那麼，這個元件如何呈現頁面？*

   `sling:resourceSuperType`屬性指向`core/wcm/components/page/v2/page`。 此屬性允許WKND的頁元件繼承核心元件頁元件的&#x200B;**all**&#x200B;功能。 這是[Proxy元件模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)的第一個範例。 如需詳細資訊，請參閱[這裡。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html)。

1. InspectWKND元件中的另一個元件`Breadcrumb`元件位於：`/apps/wknd/components/breadcrumb`。 請注意，您可以找到相同的`sling:resourceSuperType`屬性，但此時它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 這是使用Proxy元件模式來包含核心元件的另一個範例。 事實上，WKND程式碼庫中的所有元件都是核心元AEM件的proxy（我們著名的HelloWorld元件除外）。 在&#x200B;*編寫自訂程式碼之前，請盡可能*&#x200B;重複使用核心元件的功能，這是最佳實務。

1. 接下來，使用CRXDE Lite檢查`/libs/core/wcm/components/page/v2/page`的「核心元件」頁：

   >[!NOTE]
   >
   > 在AEM6.5/6.4中，核心元件位於`/apps/core/wcm/components`下。 作AEM為Cloud Service，核心元件位於`/libs`下，並自動更新。

   ![「核心元件」頁](assets/pages-templates/core-page-component-properties.png)

   請注意，本頁下方還包含許多指令碼。 「核心元件」頁面包含許多功能。 此功能可分割為多個指令碼，以方便維護和閱讀。 您可以開啟`page.html`並尋找`data-sly-include`，以追蹤HTL指令碼的包含：

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

   將HTL分割為多個指令碼的另一個原因是，允許代理元件覆寫個別指令碼，以實作自訂商業邏輯。 HTL指令碼`customfooterlibs.html`和`customheaderlibs.html`的建立目的是為了透過實施專案來覆寫。

   您可以閱讀本文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)，進一步瞭解可編輯範本如何影響到呈現[內容頁面。

1. Inspect另一個核心元件，例如`/libs/core/wcm/components/breadcrumb/v2/breadcrumb`的Breadcrumb。 檢視`breadcrumb.html`指令碼，瞭解如何最終產生Breadcrumb元件的標籤。

## 將配置保存到原始碼控制{#configuration-persistence}

在許多情況下，特別是在項目開始AEM時，將配置（如模板和相關內容策略）保留到源控制非常有用。 這可確保所有開發人員針對相同的內容和組態進行工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟度，管理模板的做法就可以交給一組特殊的超級用戶。

目前，我們將像對待其他程式碼一樣對待範本，並將&#x200B;**文章頁面範本**&#x200B;向下同步化為專案的一部分。 到目前為止，我們已將&#x200B;**pushed**&#x200B;程式碼從專案傳送AEM至本機例項AEM。 **文章頁面範本**&#x200B;是直接在本機例項上建立的AEM，因此我們需要將範本匯入至我們的專AEM案。 ****&#x200B;專案中包含&#x200B;**ui.content**&#x200B;模組，以利AEM此特定用途。

使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)外掛程式的VSCode IDE會執行後續的幾個步驟，但可能是使用您已設定為&#x200B;**import**&#x200B;的任何IDE，或從本機例項匯入內容AEM。

1. 在VSCode中，開啟`aem-guides-wknd`項目。

1. 展開「項目瀏覽器」中的&#x200B;**ui.content**&#x200B;模組。 展開`src`資料夾並導航到`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL Right+按一] 下資料 `templates` 夾，然後選 **取「從伺AEM服器匯入」**:

   ![VSCode匯入範本](assets/pages-templates/vscode-import-templates.png)

   `article-page`應匯入，`page-content`、`xf-web-variation`範本也應更新。

   ![更新的範本](assets/pages-templates/updated-templates.png)

1. 重複這些步驟以導入內容，但選擇位於`/conf/wknd/settings/wcm/policies`的&#x200B;**policys**&#x200B;資料夾。

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

   `filter.xml`檔案負責標識將隨軟體包一起安裝的節點的路徑。 請注意，每個篩選器上的`mode="merge"`都表示不會修改現有內容，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此代碼部署&#x200B;**not**&#x200B;覆寫內容非常重要。 有關使用篩選器元素的詳細資訊，請參閱[FileVault文檔](https://jackrabbit.apache.org/filevault/filter.html)。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以瞭解每個模組管理的不同節點。

   >[!WARNING]
   >
   > 為確保WKND參考站點的部署一致，已設定項目的某些分支，使`ui.content`將覆蓋JCR中的任何更改。 這是根據設計（即解決方案分支），因為將針對特定策略編寫代碼／樣式。

## 恭喜！{#congratulations}

恭喜，您剛和Adobe Experience Manager Sites建立了新範本和頁面。

### 後續步驟{#next-steps}

此時，文章頁面顯然沒有樣式。 請依照[用戶端程式庫和前端工作流程](client-side-libraries.md)教學課程，瞭解包含CSS和Javascript的最佳實務，以便將全域樣式套用至網站並整合專用的前端建置。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在`tutorial/pages-templates-solution`的Git位置上檢視並部署程式碼。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`tutorial/pages-templates-solution`分支。
