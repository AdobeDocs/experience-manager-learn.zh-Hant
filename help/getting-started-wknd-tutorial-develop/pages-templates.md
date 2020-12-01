---
title: AEM Sites快速入門——頁面和範本
seo-title: AEM Sites快速入門——頁面和範本
description: 瞭解基本頁面元件與可編輯範本之間的關係。 瞭解核心元件如何加入專案，並瞭解可編輯範本的進階政策設定，以根據Adobe XD的模型建立結構良好的文章頁面範本。
sub-product: sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 0%

---


# 頁面和範本{#pages-and-template}

在本章中，我們將探討基本頁面元件與可編輯模板之間的關係。 我們將根據[AdobeXD](https://www.adobe.com/products/xd.html)中的部分模型，建立未設定樣式的文章範本。 在建立範本的過程中，會涵蓋「可編輯範本」的核心元件和進階原則組態。

## 必備條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### Starter Project

查看教學課程所建立的基線程式碼：

1. 克隆[github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`pages-templates/start`分支。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. 使用您的Maven技巧，將程式碼庫部署至本機AEM實例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)上檢視完成的程式碼，或切換至分支`pages-templates/solution`，在本機檢出程式碼。

## 目標

1. 檢查在Adobe XD中建立的頁面設計，並將它對應至核心元件。
1. 瞭解可編輯範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 瞭解範本和頁面的連結方式

## 您將建立的{#what-you-will-build}

在本教學課程中，您將建立新的「文章頁面範本」，可用來建立新文章頁面，並與一般結構對齊。 「文章頁面範本」將以AdobeXD中的設計和UI套件為基礎。 本章僅針對構建模板的結構或骨架進行介紹。 不會實作任何樣式，但範本和頁面都能運作。

![文章頁面設計與未設定樣式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD {#adobexd}進行UI規劃

在大多數情況下，規劃新網站都從模型和靜態設計開始。 [Adobe ](https://www.adobe.com/products/xd.html) XD是建立使用者體驗的設計工具。接下來，我們將檢查UI套件和模型，以協助規劃「文章頁面範本」的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

下載[WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)。

## 使用體驗片段建立頁首和頁尾{#experience-fragments}

建立全域內容（例如頁首或頁尾）時的常見做法是使用「體驗片段」[。 ](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)體驗片段，允許我們結合多個元件，以建立單一、可參考的元件。 「體驗片段」的優點是支援多網站管理，並可讓我們管理每個地區設定的不同頁首／頁尾。

接下來，我們將更新要用作頁首和頁尾的體驗片段，以新增WKND標誌。

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> 您的體驗片段在視訊中的外觀是否不同？ 請嘗試刪除它們，然後重新安裝啟動程式專案程式碼庫。

以下是上述視訊中執行的高階步驟。

1. 更新位於[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)的體驗片段標題，加入WKND Dark標誌。

   ![WKND Dark標誌](assets/pages-templates/wknd-logo-dk.png)

   *WKND Dark logo*

1. 更新位於[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)的體驗片段標題，加入WKND Light標誌。

   ![WKND Light Logo](assets/pages-templates/wknd-logo-light.png)

   *WKND Light logo*

## 建立文章頁面範本

建立頁面時，您必須選取範本，此範本將用作建立新頁面的基礎。 範本會定義產生頁面的結構、初始內容和允許的元件。

[可編輯範本](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html)有3個主要區域：

1. **結構** -定義屬於模板一部分的元件。內容作者將無法編輯這些內容。
1. **初始內容** -定義範本將開頭的元件，內容作者可編輯和／或刪除這些元件
1. **Policys**  —— 定義元件的行為方式以及作者將提供哪些選項的配置。

接下來，我們將建立「文章頁面範本」。 這將發生在AEM的本機例項中。

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 導航到「WKND站點模板」資料夾：**工具** > **一般** > **範本** > **WKND網站**
1. 使用標題為&#x200B;**文章頁面範本**&#x200B;的&#x200B;**WKND網站空白頁面**&#x200B;範本類型建立新範本
1. 在&#x200B;**Structure**&#x200B;模式中，將模板配置為包含以下元素：

   * 體驗片段標題
   * 影像
   * 階層連結
   * 容器- 8欄寬案頭、12欄寬平板電腦、行動裝置
   * 容器- 4欄寬、12欄寬平板電腦、行動裝置
   * 體驗片段頁尾

   ![結構模式文章頁面範本](assets/pages-templates/article-page-template-structure.png)

   *結構——文章頁面範本*

1. 切換至&#x200B;**初始內容**&#x200B;模式，並新增下列元件作為起始內容：

   * **主容器**
      * Title - H1的預設大小
      * Title - *&quot;By Author Name&quot;*，大小為H4
      * 文字——空白
   * **側容器**
      * Title - *&quot;Share this Story&quot;*，大小為H5
      * 社交媒體分享
      * 分隔符號
      * 下載
      * 清單

   ![初始內容模式文章頁面範本](assets/pages-templates/article-page-template-initialcontent.png)

   *初始內容——文章頁面範本*

1. 更新&#x200B;**初始頁面屬性**&#x200B;以啟用&#x200B;**Facebook**&#x200B;和&#x200B;**Pinterest**&#x200B;的使用者共用。
1. 將影像上傳至&#x200B;**文章頁面範本的**&#x200B;屬性，以便輕鬆識別：

   ![文章頁面範本縮圖](assets/pages-templates/article-page-template-thumbnail.png)

   *文章頁面範本縮圖*

1. 在[WKND網站範本資料夾](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates)中啟用&#x200B;**文章頁面範本**。

## 建立文章頁面

既然我們有範本，我們就使用該範本建立新頁面。

1. 下載下列zip套件[WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip)，並透過[CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp)進行安裝。

   上述套件將在`/content/dam/wknd/en/magazine/la-skateparks`下方安裝數個影像和資產，以便在後續步驟中填入文章頁面。

   *Unsplash.com提供上述套件中的影像和資產免 [費授權](https://unsplash.com/)。*

   ![範例DAM資產](assets/pages-templates/sample-assets-la-skatepark.png)

1. 在&#x200B;**WKND** > **US** > **en**&#x200B;下方建立新頁面，名為&#x200B;**Magazine**。 使用&#x200B;**內容頁面**&#x200B;範本。

   本頁面將新增一些結構至我們的網站，並讓我們檢視Breadcrumb元件的實際運作。

1. 接著在&#x200B;**WKND** > **US** > **en** > **Magazine**&#x200B;下建立新頁面。 使用&#x200B;**文章頁面**&#x200B;範本。 使用&#x200B;**LA Skateparks的Ultimate guide**&#x200B;標題和&#x200B;**guide-la-skateparks**&#x200B;名稱。

   ![最初建立的文章頁面](assets/pages-templates/create-article-page-nocontent.png)

1. 在頁面中填入內容，以符合在[UI Planning中檢查的模型與AdobeXD](#adobexd)部分。 範例文章文字可以在[這裡下載](assets/pages-templates/la-skateparks-copy.txt)。 您應該可以建立類似的項目：

   ![已填入文章頁面](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > 頁面頂端的「影像」元件可以編輯，但無法移除。 階層連結元件會顯示在頁面上，但無法編輯或移除。

## 檢查節點結構{#node-structure}

此時，文章頁面顯然沒有樣式。 但是，基本結構已經到位。 接下來，我們將檢視文章頁面的節點結構，以便更好地瞭解範本的角色以及頁面元件負責轉換內容的角色。

我們可以在本機AEM例項上使用CRXDE-Lite工具來完成此作業。

1. 開啟[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)，然後使用樹狀導覽導覽至`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 按一下`la-skateparks`頁面下的`jcr:content`節點並查看屬性：

   ![JCR內容屬性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   請注意`cq:template`的值，此值指向我們先前建立的「文章頁面範本」(`/conf/wknd/settings/wcm/templates/article-page`)。

   另請注意`sling:resourceType`的值，它指向`wknd/components/structure/page`。 這是由AEM專案原型建立的頁面元件，負責根據範本呈現頁面。

1. 展開`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`下的`jcr:content`節點，並查看節點層次結構：

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   您應該能夠將每個節點鬆散地映射到已編寫的元件。 查看您是否可以透過檢查前置有`responsivegrid`的節點來識別使用的不同配置容器。

1. 接下來檢查位於`/apps/wknd/components/structure/page`的頁面元件。 在CRXDE Lite中檢視元件屬性：

   ![頁面元件屬性](assets/pages-templates/page-component-properties.png)

   請注意，頁面元件位於名為&#x200B;**structure**&#x200B;的資料夾下方。 這是與「範本編輯器」結構模式相對應的慣例，用來指出頁面元件並非作者將直接與之互動的項目。

   請注意，頁面元件下方只有2個HTL指令碼，`customfooterlibs.html`和`customheaderlibs.html`。 那麼，這個元件如何呈現頁面？

   請注意`sling:resourceSuperType`屬性和`core/wcm/components/page/v2/page`的值。 此屬性允許WKND的頁元件繼承核心元件頁元件的所有功能。 這是[Proxy元件模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)的第一個範例。 如需詳細資訊，請參閱[這裡。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html)。

1. 檢查WKND元件中的另一個元件，`Breadcrumb`元件位於：`/apps/wknd/components/content/breadcrumb`。 請注意，您可以找到相同的`sling:resourceSuperType`屬性，但此時它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 這是使用Proxy元件模式來包含核心元件的另一個範例。 事實上，WKND程式碼庫中的所有元件都是AEM Core Components的proxy（我們著名的HelloWorld元件除外）。 在&#x200B;*編寫自訂程式碼之前，請盡可能*&#x200B;重複使用核心元件的功能，這是最佳實務。

1. 接下來，使用CRXDE Lite檢查`/apps/core/wcm/components/page/v2/page`的核心元件頁面：

   ![「核心元件」頁](assets/pages-templates/core-page-component-properties.png)

   請注意，本頁下方還包含許多指令碼。 「核心元件」頁面包含許多功能。 此功能可分割為多個指令碼，以方便維護和閱讀。 您可以開啟`page.html`並尋找`data-sly-include`，以追蹤HTL指令碼的包含：

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   將HTL分割為多個指令碼的另一個原因是，允許代理元件覆寫個別指令碼，以實作自訂商業邏輯。 HTL指令碼`customfooterlibs.html`和`customheaderlibs.html`的建立目的是為了透過實施專案來覆寫。

   您可以閱讀本文章](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages)，進一步瞭解可編輯範本如何影響到呈現[內容頁面。

1. 檢查另一個核心元件，如`/apps/core/wcm/components/breadcrumb/v2/breadcrumb`的Breadcrumb。 檢視`breadcrumb.html`指令碼，瞭解如何最終產生Breadcrumb元件的標籤。

## 將配置保存到原始碼控制{#configuration-persistence}

在許多情況下，尤其是在AEM專案開始時，將設定（例如範本和相關內容原則）保留至來源控制非常有用。 這可確保所有開發人員針對相同的內容和組態進行工作，並可確保環境之間的額外一致性。 一旦項目達到一定的成熟度，管理模板的做法就可以交給一組特殊的超級用戶。

目前，我們將像對待其他程式碼一樣對待範本，並將&#x200B;**文章頁面範本**&#x200B;向下同步化為專案的一部分。 到目前為止，我們已將&#x200B;**推送**&#x200B;程式碼從AEM專案傳送至AEM的本機例項。 **文章頁面範本**&#x200B;是直接在AEM的本機例項上建立，因此我們需要&#x200B;**pull**&#x200B;或將範本匯入我們的AEM專案。 **ui.content**&#x200B;模組已包含在AEM專案中，以利做此特定用途。

接下來的幾個步驟將使用Eclipse IDE進行，但可能是使用您已設定為&#x200B;**pull**&#x200B;的任何IDE，或從AEM的本機例項匯入內容。

1. 在Eclipse IDE中，請確定已啟動將AEM開發人員工具外掛程式連接至AEM本機例項的伺服器，以及已將&#x200B;**ui.content**&#x200B;模組新增至伺服器組態。

   ![Eclipse Server連線](assets/pages-templates/eclipse-server-started.png)

1. 展開「項目瀏覽器」中的&#x200B;**ui.content**&#x200B;模組。 展開`src`資料夾（帶小全局表徵圖的資料夾）並導航至`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 右鍵+單] 擊節 `templates` 點並選擇 **從伺服器導入……**:

   ![Eclipse匯入範本](assets/pages-templates/eclipse-import-templates.png)

   確認「從儲存庫導入」對話框，然後按一下「完成」。 ********&#x200B;您現在應該會在`templates`資料夾下看到`article-page-template`。

1. 重複這些步驟以導入內容，但選擇位於`/conf/wknd/settings/wcm/policies`的&#x200B;**policys**&#x200B;節點。

   ![Eclipse匯入政策](assets/pages-templates/policies-article-page-template.png)

1. 檢查位於`src/main/content/META-INF/vault/filter.xml`的`filter.xml`檔案。

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

恭喜您，您剛使用Adobe Experience Manager Sites建立了新的範本和頁面。

### 後續步驟{#next-steps}

此時，文章頁面顯然沒有樣式。 請依照[用戶端程式庫和前端工作流程](client-side-libraries.md)教學課程，瞭解包含CSS和Javascript的最佳實務，以便將全域樣式套用至網站並整合專用的前端建置。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在`pages-templates/solution`的Git位置上檢視並部署程式碼。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`pages-templates/solution`分支。
