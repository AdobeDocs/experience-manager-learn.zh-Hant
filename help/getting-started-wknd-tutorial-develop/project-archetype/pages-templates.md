---
title: AEM Sites快速入門 — 頁面和範本
description: 瞭解基礎頁面元件和可編輯範本之間的關係。 瞭解如何將核心元件代理至專案。 瞭解可編輯範本的進階原則設定，以根據Adobe XD的模型建置結構良好的文章頁面範本。
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
workflow-type: tm+mt
source-wordcount: '2855'
ht-degree: 0%

---

# 頁面和範本 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

在本章中，讓我們探索基礎頁面元件與可編輯範本之間的關係。 瞭解如何根據[Adobe XD](https://helpx.adobe.com/support/xd.html)的某些模型建置無樣式的文章範本。 在建置範本的過程中，將涵蓋「可編輯範本」的核心元件和進階原則設定。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

檢視教學課程建置的基底程式碼：

1. 從[GitHub](https://github.com/adobe/aem-guides-wknd)檢視`tutorial/pages-templates-start`分支

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. 使用您的Maven技能將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`設定檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution)上檢視完成的程式碼，或切換至分支`tutorial/pages-templates-solution`在本機簽出程式碼。

## 目標

1. Inspect是在Adobe XD中建立的頁面設計，並對應至核心元件。
1. 瞭解可編輯範本的詳細資料，以及如何使用原則來強制實施頁面內容的精細控制。
1. 瞭解範本和頁面的連結方式

## 您即將建置的內容 {#what-build}

在本教學課程的這個部分，您將建立新的文章頁面範本，此範本可用來建立文章頁面，並與通用結構保持一致。 文章頁面範本以Adobe XD中的設計和產生的UI套件為基礎。 本章只著重於建置範本的結構或骨架。 未實作任何樣式，但範本和頁面運作正常。

![文章頁面設計和未設定樣式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD進行UI規劃 {#adobexd}

通常，規劃新網站會從模型和靜態設計開始。 [Adobe XD](https://helpx.adobe.com/support/xd.html)是建立使用者體驗的設計工具。 接下來，讓我們檢查UI套件和模型，協助規劃文章頁面範本的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**下載[WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 一般[AEM核心元件UI Kit也可以](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)作為自訂專案的起點。

## 建立文章頁面範本

建立頁面時，您必須選取範本，作為建立頁面的基礎。 範本會定義結果頁面的結構、初始內容和允許的元件。

[可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)有三個主要區域：

1. **結構** — 定義屬於範本一部分的元件。 內容作者無法編輯這些內容。
1. **初始內容** — 定義範本開始使用的元件，這些元件可由內容作者編輯和/或刪除
1. **原則** — 定義元件行為方式及作者選項設定。

接下來，在AEM中建立符合模型結構的範本。 這會在AEM的本機執行個體中發生。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上述影片的高層級步驟：

### 結構設定

1. 使用&#x200B;**頁面範本型別**，名為&#x200B;**文章頁面**&#x200B;來建立範本。
1. 切換至&#x200B;**結構**&#x200B;模式。
1. 新增&#x200B;**體驗片段**&#x200B;元件，以做為範本頂端的&#x200B;**Header**。
   * 設定元件以指向`/content/experience-fragments/wknd/us/en/site/header/master`。
   * 將原則設定為&#x200B;**Page Header**，並確定&#x200B;**預設元素**&#x200B;已設定為`header`。 `header`元素在下一個章節中會以CSS作為目標。
1. 新增&#x200B;**體驗片段**&#x200B;元件，以做為範本底部的&#x200B;**頁尾**。
   * 設定元件以指向`/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 將原則設定為&#x200B;**頁尾**，並確定&#x200B;**預設元素**&#x200B;已設定為`footer`。 `footer`元素在下一個章節中會以CSS作為目標。
1. 鎖定最初建立範本時包含的&#x200B;**主要**&#x200B;容器。
   * 將原則設定為&#x200B;**Page Main**，並確定&#x200B;**預設元素**&#x200B;已設定為`main`。 `main`元素在下一個章節中會以CSS作為目標。
1. 將&#x200B;**Image**&#x200B;元件新增至&#x200B;**主要**&#x200B;容器。
   * 解除鎖定&#x200B;**Image**&#x200B;元件。
1. 在主容器的&#x200B;**Image**&#x200B;元件下新增&#x200B;**階層連結**&#x200B;元件。
   * 為名為&#x200B;**文章頁面 — 階層連結**&#x200B;的&#x200B;**階層連結**&#x200B;元件建立原則。 將&#x200B;**導覽開始層級**&#x200B;設定為&#x200B;**4**。
1. 在&#x200B;**階層連結**&#x200B;元件下方和&#x200B;**主要**&#x200B;容器內新增&#x200B;**容器**&#x200B;元件。 這會作為範本的&#x200B;**內容容器**。
   * 解除鎖定&#x200B;**Content**&#x200B;容器。
   * 將原則設定為&#x200B;**頁面內容**。
1. 在&#x200B;**內容容器**&#x200B;下方新增另一個&#x200B;**容器**&#x200B;元件。 這會作為範本的&#x200B;**側邊欄**&#x200B;容器。
   * 解除鎖定&#x200B;**側邊欄**&#x200B;容器。
   * 建立名為&#x200B;**Article Page - Side Rail**&#x200B;的原則。
   * 設定&#x200B;**WKND Sites專案下的**&#x200B;允許的元件&#x200B;**— 內容**&#x200B;以包含： **按鈕**、**下載**、**影像**、**清單**、**分隔符號**、**社群媒體共用**、**文字**&#x200B;和&#x200B;**標題**。
1. 更新頁面根容器的原則。 這是範本最外側的容器。 將原則設定為&#x200B;**頁面根目錄**。
   * 在&#x200B;**容器設定**&#x200B;下，將&#x200B;**配置**&#x200B;設定為&#x200B;**回應式格線**。
1. 參與&#x200B;**內容容器**&#x200B;的佈局模式。 從右到左拖曳控點，並將容器縮小為八欄寬。
1. 參與&#x200B;**側邊欄容器**&#x200B;的佈局模式。 從右到左拖曳控點，並將容器縮小為四欄寬。 然後，從左到右拖曳左側控制代碼一欄，讓容器3欄變寬，並在&#x200B;**內容容器**&#x200B;之間留下1欄間隙。
1. 開啟行動模擬器並切換至行動中斷點。 再次使用版面配置模式，並將&#x200B;**內容容器**&#x200B;和&#x200B;**側邊欄容器**&#x200B;設為頁面的完整寬度。 這會將容器垂直棧疊在行動中斷點中。
1. 更新&#x200B;**內容容器**&#x200B;中&#x200B;**Text**&#x200B;元件的原則。
   * 將原則設定為&#x200B;**內容文字**。
   * 在&#x200B;**外掛程式** > **段落樣式**&#x200B;下，勾選&#x200B;**啟用段落樣式**，並確定已啟用&#x200B;**引號區塊**。

### 初始內容設定

1. 切換至&#x200B;**初始內容**&#x200B;模式。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**Content容器**。 這會當作文章標題。 當它留空時，它會自動顯示目前頁面的標題。
1. 在第一個Title元件下方新增第二個&#x200B;**Title**&#x200B;元件。
   * 使用文字設定元件：「By Author」。 這是文字預留位置。
   * 將型別設定為`H4`。
1. 在&#x200B;**By Author**&#x200B;標題元件下方新增&#x200B;**文字**&#x200B;元件。
1. 將&#x200B;**Title**&#x200B;元件新增至&#x200B;**側邊欄容器**。
   * 使用文字設定元件：「共用本文」。
   * 將型別設定為`H5`。
1. 在「**共用此劇本**」標題元件下方新增&#x200B;**社群媒體共用**&#x200B;元件。
1. 在&#x200B;**社群媒體共用**&#x200B;元件下新增&#x200B;**分隔符號**&#x200B;元件。
1. 在&#x200B;**分隔符號**&#x200B;元件下方新增&#x200B;**下載**&#x200B;元件。
1. 在&#x200B;**下載**&#x200B;元件下方新增&#x200B;**清單**&#x200B;元件。
1. 更新範本的&#x200B;**初始頁面屬性**。
   * 在&#x200B;**社群媒體** > **社群媒體分享**&#x200B;底下，檢查&#x200B;**Facebook**&#x200B;和&#x200B;**Pinterest**

### 啟用範本並新增縮圖

1. 導覽至[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)，在範本主控台中檢視範本
1. **啟用**&#x200B;文章頁面範本。
1. 編輯「文章頁面」範本的屬性，並上傳下列縮圖，以快速識別使用「文章頁面」範本建立的頁面：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

## 使用體驗片段更新頁首和頁尾 {#experience-fragments}

建立全域內容（例如頁首或頁尾）的常見作法是使用[體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 體驗片段，可讓使用者結合多個元件，以建立單一可參考的元件。 體驗片段的優點在於支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en)。

AEM專案原型產生了頁首和頁尾。 接下來，更新體驗片段以符合模型。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上述影片的高層級步驟：

1. 下載範例內容封裝&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**。
1. 在[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)使用封裝管理員上傳及安裝內容封裝
1. 更新Web Variation範本，這是在[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)用於體驗片段的範本
   * 更新範本上&#x200B;**Container**&#x200B;元件的原則。
   * 將原則設定為&#x200B;**XF Root**。
   * 在底下，**允許的元件**&#x200B;選取元件群組&#x200B;**WKND Sites專案 — 結構**&#x200B;以包含&#x200B;**語言導覽**、**導覽**&#x200B;和&#x200B;**快速搜尋**&#x200B;元件。

### 更新標題體驗片段

1. 開啟在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)轉譯標題的體驗片段
1. 設定片段的根&#x200B;**容器**。 這是最外層的&#x200B;**容器**。
   * 將&#x200B;**配置**&#x200B;設定為&#x200B;**回應式格線**
1. 將&#x200B;**WKND深色標誌**&#x200B;新增為影像至&#x200B;**容器**&#x200B;的頂端。 標誌包含在先前步驟中安裝的套件中。
   * 將&#x200B;**WKND深色標誌**&#x200B;的版面配置修改為&#x200B;**兩個**&#x200B;欄寬。 從右到左拖曳控點。
   * 使用&#x200B;**替代文字**&#x200B;設定標誌「WKND標誌」。
   * 將標誌設定為&#x200B;**連結**&#x200B;至`/content/wknd/us/en`首頁。
1. 設定已放置在頁面上的&#x200B;**Navigation**&#x200B;元件。
   * 將&#x200B;**排除根層級**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**導覽結構深度**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的配置修改為&#x200B;**8**&#x200B;欄寬。 從右到左拖曳控點。
1. 移除&#x200B;**語言導覽**&#x200B;元件。
1. 將&#x200B;**搜尋**&#x200B;元件的配置修改為&#x200B;**兩個**&#x200B;欄寬。 從右到左拖曳控點。 現在，所有元件應該水準對齊一列。

### 更新頁尾體驗片段

1. 開啟在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)呈現頁尾的體驗片段
1. 設定片段的根&#x200B;**容器**。 這是最外層的&#x200B;**容器**。
   * 將&#x200B;**配置**&#x200B;設定為&#x200B;**回應式格線**
1. 將&#x200B;**WKND淺色標誌**&#x200B;新增為影像至&#x200B;**容器**&#x200B;的頂端。 標誌包含在先前步驟中安裝的套件中。
   * 將&#x200B;**WKND Light Logo**&#x200B;的版面配置修改為&#x200B;**兩個**&#x200B;欄寬。 從右到左拖曳控點。
   * 使用&#x200B;**替代文字** （WKND標誌燈）設定標誌。
   * 將標誌設定為&#x200B;**連結**&#x200B;至`/content/wknd/us/en`首頁。
1. 在標誌下方新增&#x200B;**Navigation**&#x200B;元件。 設定&#x200B;**導覽**&#x200B;元件：
   * 將&#x200B;**排除根層級**&#x200B;設定為&#x200B;**1**。
   * 取消勾選&#x200B;**收集所有子頁面**。
   * 將&#x200B;**導覽結構深度**&#x200B;設定為&#x200B;**1**。
   * 將&#x200B;**Navigation**&#x200B;元件的配置修改為&#x200B;**8**&#x200B;欄寬。 從右到左拖曳控點。

## 建立文章頁面

接下來，使用「文章頁面」範本建立頁面。 編寫頁面內容以符合網站模型。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上述影片的高層級步驟：

1. 瀏覽至[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)的Sites主控台。
1. 在&#x200B;**WKND** > **US** > **EN** > **雜誌**&#x200B;下方建立頁面。
   * 選擇&#x200B;**文章頁面**&#x200B;範本。
   * 在「**屬性**」下，將「**標題**」設定為「LA滑板公園最終指南」
   * 將&#x200B;**Name**&#x200B;設定為&quot;guide-la-skateparks&quot;
1. 將&#x200B;**由Author**&#x200B;標題取代為&quot;By Stacey Roswells&quot;文字。
1. 更新&#x200B;**Text**&#x200B;元件以包含段落以填入文章。 您可以使用下列文字檔做為復本： [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 新增其他&#x200B;**文字**&#x200B;元件。
   * 更新元件以包含引述：「沒有比洛杉磯更適合清除的地方了。」
   * 以全熒幕模式編輯RTF編輯器，並修改上述引號以使用&#x200B;**引號區塊**&#x200B;元素。
1. 繼續填入文章內文以符合模型。
1. 設定&#x200B;**Download**&#x200B;元件以使用文章的PDF版本。
   * 在&#x200B;**下載** > **屬性**&#x200B;下方，按一下核取方塊以&#x200B;**從DAM資產取得標題**。
   * 將&#x200B;**Description**&#x200B;設為：「取得完整劇本」。
   * 將&#x200B;**動作文字**&#x200B;設為：「下載PDF」。
1. 設定&#x200B;**清單**&#x200B;元件。
   * 在&#x200B;**清單設定** > **使用**&#x200B;建立清單，選取&#x200B;**子頁面**。
   * 將&#x200B;**父頁面**&#x200B;設定為`/content/wknd/us/en/magazine`。
   * 在下方，**專案設定**&#x200B;檢查&#x200B;**連結專案**&#x200B;並檢查&#x200B;**顯示日期**。

## Inspect節點結構 {#node-structure}

此時，文章頁面顯然未設定樣式。 不過，基本結構已準備就緒。 接下來，檢查文章頁面的節點結構，以更加瞭解範本、頁面和元件的角色。

在本機AEM例項上使用CRXDE-Lite工具來檢視基礎節點結構。

1. 開啟[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)，並使用樹狀導覽來導覽至`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下`la-skateparks`頁面下方的`jcr:content`節點並檢視屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   請注意`cq:template`的值，它指向先前建立的文章頁面範本`/conf/wknd/settings/wcm/templates/article-page`。

   也請注意指向`wknd/components/page`的`sling:resourceType`值。 這是由AEM專案原型建立的頁面元件，負責根據範本轉譯頁面。

1. 展開`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`底下的`jcr:content`節點並檢視節點階層：

   ![JCR Content LA滑板場](assets/pages-templates/page-jcr-structure.png)

   您應該能夠鬆散地將每個節點對應至已編寫的元件。 檢視您是否能透過檢查帶有`container`首碼的節點來識別所使用的不同配置容器。

1. 接著在`/apps/wknd/components/page`檢查頁面元件。 以CRXDE Lite檢視元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   頁面元件下方只有兩個HTL指令碼： `customfooterlibs.html`和`customheaderlibs.html`。 *這個元件如何呈現頁面？*

   `sling:resourceSuperType`屬性指向`core/wcm/components/page/v2/page`。 此屬性允許WKND的頁面元件繼承&#x200B;**所有**&#x200B;核心元件頁面元件的功能。 這是第一個稱為[Proxy元件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)的範例。 在[這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html)可找到更多資訊。

1. Inspect是WKND元件中的另一個元件，來自`/apps/wknd/components/breadcrumb`的`Breadcrumb`元件。 請注意，可以找到相同的`sling:resourceSuperType`屬性，但這次它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 這是使用Proxy元件模式來包含核心元件的另一個範例。 事實上，WKND程式碼基底中的所有元件都是AEM核心元件的代理程式（自訂示範HelloWorld元件除外）。 最佳實務是在&#x200B;*之前*&#x200B;寫入自訂程式碼之前，儘可能重複使用核心元件的功能。

1. 接下來，使用CRXDE Lite檢查位於`/libs/core/wcm/components/page/v2/page`的核心元件頁面：

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心元件位於`/apps/core/wcm/components`下方。 在AEM as a Cloud Service中，核心元件位於`/libs`下並自動更新。

   ![核心元件頁面](assets/pages-templates/core-page-component-properties.png)

   請注意，許多指令碼檔案包含在此頁面下方。 核心元件頁面包含多項功能。 此功能被分成多個指令碼，以方便維護和閱讀。 您可以透過開啟`page.html`並尋找`data-sly-include`來追蹤納入HTL指令碼：

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

   將HTL分成多個指令碼的另一個原因是為了允許Proxy元件覆寫個別指令碼，以實作自訂商業邏輯。 HTL指令碼`customfooterlibs.html`和`customheaderlibs.html`是為明確用途而建立的，以便透過實作專案來覆寫。

   您可以閱讀本文章[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)，深入瞭解可編輯範本如何影響內容頁面的轉譯。

1. Inspect是另一個核心元件，例如`/libs/core/wcm/components/breadcrumb/v2/breadcrumb`的階層連結。 檢視`breadcrumb.html`指令碼，瞭解最終產生階層連結元件標籤的方式。

## 將設定儲存至Source控制項 {#configuration-persistence}

通常，尤其是在開始AEM專案時，將設定（例如範本和相關內容原則）保留到原始檔控制中很有價值。 這可確保所有開發人員都針對相同的內容和設定集，且可確保環境之間有額外的一致性。 一旦專案達到一定的成熟度，管理範本的實務就可以交給特殊的超級使用者群組。


目前，範本會被視為其他程式碼片段，並將&#x200B;**文章頁面範本**&#x200B;同步處理為專案的一部分。
直到現在，程式碼才會從AEM專案推送到AEM的本機執行個體。 **文章頁面範本**&#x200B;是直接在AEM的本機執行個體上建立的，所以它需要&#x200B;**匯入**&#x200B;範本至AEM專案。 **ui.content**&#x200B;模組包含在AEM專案中，供此特定目的使用。

在VSCode IDE中使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)外掛程式完成後續幾個步驟。 但他們可以使用您設定為&#x200B;**匯入**&#x200B;或從AEM的本機執行個體匯入內容的任何IDE來執行。

1. 在中，VSCode開啟`aem-guides-wknd`專案。

1. 展開專案總管中的&#x200B;**ui.content**&#x200B;模組。 展開`src`資料夾並導覽至`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 按一下滑鼠右鍵] `templates`資料夾並選取&#x200B;**從AEM伺服器匯入**：

   ![VSCode匯入範本](assets/pages-templates/vscode-import-templates.png)

   應匯入`article-page`，也應更新`page-content`、`xf-web-variation`範本。

   ![已更新範本](assets/pages-templates/updated-templates.png)

1. 重複步驟以匯入內容，但從`/conf/wknd/settings/wcm/policies`選取&#x200B;**原則**&#x200B;資料夾。

   ![VSCode匯入原則](assets/pages-templates/policies-article-page-template.png)

1. 從`ui.content/src/main/content/META-INF/vault/filter.xml`Inspect `filter.xml`檔案。

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

   `filter.xml`檔案負責識別隨套件安裝的節點路徑。 請注意每個篩選器上的`mode="merge"`，這表示現有內容不會被修改，只會新增內容。 由於內容作者可能正在更新這些路徑，因此程式碼部署&#x200B;**不會**&#x200B;覆寫內容非常重要。 請參閱[FileVault檔案](https://jackrabbit.apache.org/filevault/filter.html)，以取得使用篩選元素的詳細資訊。

   比較`ui.content/src/main/content/META-INF/vault/filter.xml`與`ui.apps/src/main/content/META-INF/vault/filter.xml`，瞭解每個模組所管理的不同節點。

   >[!WARNING]
   >
   > 為了確保WKND參考網站一致的部署，專案的一些分支已設定為`ui.content`覆寫JCR中的任何變更。 這是依設計（即針對解決方案分支）進行的，因為程式碼/樣式是為特定原則所撰寫。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager Sites建立範本和頁面。

### 後續步驟 {#next-steps}

此時，文章頁面顯然未設定樣式。 遵循[使用者端資料庫和前端工作流程](client-side-libraries.md)教學課程，瞭解包含CSS和JavaScript的最佳實務，以將全域樣式套用至網站並整合專屬的前端組建。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/pages-templates-solution`上檢閱並部署本機的程式碼。

1. 複製[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存放庫。
1. 檢視`tutorial/pages-templates-solution`分支。
