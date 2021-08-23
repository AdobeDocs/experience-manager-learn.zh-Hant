---
title: 與風格體系一起發展
seo-title: 與風格體系一起發展
description: 了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋使用樣式系統開發，以使用品牌專屬CSS和範本編輯器的進階政策設定來擴充核心元件。
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心元件，樣式系統
topic: 內容管理、開發
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 0%

---


# 與風格體系一起發展 {#developing-with-the-style-system}

了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋使用樣式系統開發，以使用品牌專屬CSS和範本編輯器的進階政策設定來擴充核心元件。

## 必備條件 {#prerequisites}

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

建議您檢閱[用戶端程式庫和前端工作流程](client-side-libraries.md)教學課程，以了解用戶端程式庫的基本知識，以及AEM專案內建的各種前端工具。

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/style-system-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)上檢視完成的程式碼，或切換至分支`tutorial/style-system-solution`在本機檢出程式碼。

## 目標

1. 了解如何使用樣式系統將品牌專屬的CSS套用至AEM核心元件。
1. 了解BEM記號，以及如何用來仔細調整樣式。
1. 使用可編輯的模板應用高級策略配置。

## 您將建置的 {#what-you-will-build}

在本章中，我們將使用[樣式系統功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)來建立「文章」頁面上使用的&#x200B;**Title**&#x200B;和&#x200B;**Text**&#x200B;元件的變體。

![標題可用樣式](assets/style-system/styles-added-title.png)

*可用於標題元件的下划線樣式*

## 背景 {#background}

[樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html)允許開發人員和模板編輯器建立元件的多個視覺變化。 然後，作者可決定在撰寫頁面時要使用哪種樣式。 在教學課程的其餘部分，我們會運用樣式系統來達成數種獨特的樣式，同時以低程式碼的方式運用核心元件。

樣式系統的一般概念是，作者可選擇元件外觀的各種樣式。 「styles」由會插入至元件外部div的其他CSS類別作為後盾。 在用戶端程式庫中，會根據這些樣式類來新增CSS規則，以便元件變更外觀。

您可以在此處](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html)找到樣式系統的[詳細文檔。 此外，還有一部很棒的[技術視頻，用於了解樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)。

## 下划線樣式 — 標題 {#underline-style}

[標題元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html)已作為&#x200B;**ui.apps**&#x200B;模組的一部分，複製到`/apps/wknd/components/title`下的專案。 已在&#x200B;**ui.frontend**&#x200B;模組中實作標題元素的預設樣式(`H1`、`H2`、`H3`...)。

[WKND文章設計](assets/pages-templates/wknd-article-design.xd)包含帶下划線的標題元件的唯一樣式。 「樣式系統」(Style System)可用於允許作者添加下划線樣式的選項，而不是建立兩個元件或修改元件對話框。

![下划線樣式 — 標題元件](assets/style-system/title-underline-style.png)

### Inspect標題標籤

作為前端開發人員，設計核心元件樣式的第一步是了解元件產生的標籤。

1. 開啟新瀏覽器，並在AEM核心元件程式庫網站上檢視標題元件：[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. 以下是Title元件的標籤：

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   標題元件的BEM記號：

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. Style系統將CSS類添加到元件周圍的外div中。 因此，我們要定位的標籤將類似以下內容：

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### 實作底線樣式 — ui.frontend

接下來，使用專案的&#x200B;**ui.frontend**&#x200B;模組實作底線樣式。 我們將使用與&#x200B;**ui.frontend**&#x200B;模組搭配的Webpack開發伺服器，在&#x200B;*部署至AEM的本機例項之前預覽樣式*。

1. 從&#x200B;**ui.frontend**&#x200B;模組內運行以下命令以啟動Webpack開發伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   這應該會在[http://localhost:8080](http://localhost:8080)開啟瀏覽器。

   >[!NOTE]
   >
   > 如果影像看起來損毀，請確定啟動程式專案已部署至AEM的本機例項（在連接埠4502上執行），且使用的瀏覽器也已登入本機AEM例項。

   ![Webpack開發伺服器](assets/style-system/static-webpack-server.png)

1. 在IDE中，開啟位於以下位置的檔案`index.html`:`ui.frontend/src/main/webpack/static/index.html`。 這是Webpack開發伺服器使用的靜態標籤。
1. 在`index.html`中，通過搜索文檔&#x200B;*cmp-title*，查找標題元件的實例以向添加下划線樣式。 選擇帶有文本&#x200B;*&quot;Vans of the Wall Skatepark&quot;*&#x200B;的標題元件（第218行）。 將類別`cmp-title--underline`新增至周圍的div:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. 返回瀏覽器，確認標籤中反映了額外的類別。
1. 返回&#x200B;**ui.frontend**&#x200B;模組並更新位於以下位置的檔案`title.scss`:`ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >將樣式一律嚴格限定至目標元件，即視為最佳作法。 這可確保額外的樣式不會影響頁面的其他區域。
   >
   >所有核心元件都遵循&#x200B;**[BEM標籤法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。 為元件建立預設樣式時，最佳作法是定位外部CSS類別。 另一個最佳實務是鎖定核心元件BEM標籤法所指定的類別名稱，而非HTML元素。

1. 再次返回瀏覽器，您應該會看到新增的底線樣式：

   ![Webpack開發伺服器中可見的下划線樣式](assets/style-system/underline-implemented-webpack.png)

1. 停止Webpack開發伺服器。

### 添加標題策略

接下來，我們需要為「標題」元件添加新策略，以允許內容作者選擇要應用於特定元件的「下划線」樣式。 這是使用AEM中的範本編輯器來完成。

1. 使用您的Maven技能，將程式碼基底部署至本機AEM執行個體：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至位於以下位置的&#x200B;**文章頁面**&#x200B;範本：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在&#x200B;**Structure**&#x200B;模式中，在主&#x200B;**佈局容器**&#x200B;中，選擇&#x200B;***允許的元件*下所列的Title**&#x200B;元件旁的&#x200B;**Policy**&#x200B;表徵圖：

   ![標題策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 為標題元件建立新策略，其值如下：

   *策略標題**: **WKND標題**

   *屬性*  >樣 *式標籤*  >  *新增樣式*

   **底線** :  `cmp-title--underline`

   ![標題的樣式策略配置](assets/style-system/title-style-policy.png)

   按一下&#x200B;**Done**&#x200B;以保存對Title策略的更改。

   >[!NOTE]
   >
   > 值`cmp-title--underline`符合在&#x200B;**ui.frontend**&#x200B;模組中開發時我們先前鎖定的CSS類別。

### 應用下划線樣式

最後，作為作者，我們可以選擇將底線樣式套用至特定標題元件。

1. 導覽至AEM Sites編輯器中的&#x200B;**La Skateparks**&#x200B;文章，網址為：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**Edit**&#x200B;模式中，選擇標題元件。 按一下&#x200B;**畫筆**&#x200B;表徵圖並選擇&#x200B;**下划線**&#x200B;樣式：

   ![應用下划線樣式](assets/style-system/apply-underline-style-title.png)

   作為作者，您應該可以開啟/關閉樣式。

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示> **檢視為已發佈**&#x200B;以檢查AEM編輯器外部的頁面。

   ![以已發佈狀態檢視](assets/style-system/view-as-published.png)

   使用瀏覽器開發人員工具驗證標題元件周圍的標籤有套用至外部div的CSS類別`cmp-title--underline`。

## 報價塊樣式 — 文本 {#text-component}

接下來，重複類似步驟，將唯一樣式應用到[文本元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)。 已將文字元件代理至`/apps/wknd/components/text`下的專案，作為&#x200B;**ui.apps**&#x200B;模組的一部分。 在&#x200B;**ui.frontend**&#x200B;中已實作段落元素的預設樣式。

[WKND文章設計](assets/pages-templates/wknd-article-design.xd)包含帶引號塊的Text元件的唯一樣式：

![報價塊樣式 — 文本元件](assets/style-system/quote-block-style.png)

### Inspect文字元件標籤

我們將再次檢查Text元件的標籤。

1. 查看Text元件的標籤位置：[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. 以下是Text元件的標籤：

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   文字元件的BEM記號：

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. Style系統將CSS類添加到元件周圍的外div中。 因此，我們要定位的標籤將類似以下內容：

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### 實作引號區塊樣式 — ui.frontend

接下來，我們將使用專案的&#x200B;**ui.frontend**&#x200B;模組實作報價區塊樣式。

1. 從&#x200B;**ui.frontend**&#x200B;模組內運行以下命令以啟動Webpack開發伺服器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. 在IDE中，開啟位於以下位置的檔案`index.html`:`ui.frontend/src/main/webpack/static/index.html`。
1. 在`index.html`中，通過搜索文本&#x200B;*&quot;Jacob Wester&quot;*（第210行）來查找文本元件的實例。 將類別`cmp-text--quote`新增至周圍的div:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. 更新位於以下位置的檔案`text.scss`:`ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > 在此案例中，原始HTML元素會由樣式定位。 這是因為「文字」元件為內容作者提供RTF編輯器。 針對RTE內容直接建立樣式時應謹慎操作，而將樣式嚴格限制則更為重要。

1. 再次返回瀏覽器，您應該會看到新增的報價區塊樣式：

   ![Webpack開發伺服器中可見的報價塊樣式](assets/style-system/quoteblock-implemented-webpack.png)

1. 停止Webpack開發伺服器。

### 添加文本策略

接下來，為Text元件添加新策略。

1. 使用您的Maven技能，將程式碼基底部署至本機AEM執行個體：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至&#x200B;**文章頁面範本**，位於：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html))。

1. 在&#x200B;**Structure**&#x200B;模式中，在主&#x200B;**佈局容器**&#x200B;中，選擇&#x200B;**Text**&#x200B;元件（列在&#x200B;*允許的元件*&#x200B;下）旁的&#x200B;**Policy**&#x200B;表徵圖：

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本元件策略：

   *策略標題**: **內容文字**

   *外掛程式*  >段 *落樣式*  >啟 *用段落樣式*

   *樣式標籤*  >  *新增樣式*

   **報價塊** :  `cmp-text--quote`

   ![文本元件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本元件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   按一下&#x200B;**Done**&#x200B;以保存對Text策略的更改。

### 應用引號塊樣式

1. 導覽至AEM Sites編輯器中的&#x200B;**La Skateparks**&#x200B;文章，網址為：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**編輯**&#x200B;模式中，選擇文本元件。 編輯元件以包含報價元素：

   ![文字元件設定](assets/style-system/configure-text-component.png)

1. 選擇文本元件，然後按一下&#x200B;**刷子**&#x200B;表徵圖並選擇&#x200B;**引號塊**&#x200B;樣式：

   ![應用引號塊樣式](assets/style-system/quote-block-style-applied.png)

   作為作者，您應該可以開啟/關閉樣式。

## 固定寬度 — 容器（額外） {#layout-container}

容器元件可用來建立「文章頁面範本」的基本結構，並提供內容作者可在頁面上新增內容的放置區。 容器也可運用樣式系統，為內容作者提供更多配置設計選項。

「文章頁面」範本的&#x200B;**主容器**&#x200B;包含兩個可製作的容器，且寬度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章頁面範本中的主容器*。

**主容器**&#x200B;的策略將預設元素設定為`main`:

![主容器策略](assets/style-system/main-container-policy.png)

已修正&#x200B;**主容器**&#x200B;的CSS設定於&#x200B;**ui.frontend**&#x200B;模組的`ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

「樣式系統」可用來建立&#x200B;**固定寬度**&#x200B;樣式，而不是定位`main` HTML元素，作為「容器」策略的一部分。 樣式系統可讓使用者選擇切換&#x200B;**固定寬度**&#x200B;和&#x200B;**流體寬度**&#x200B;容器。

1. **額外挑戰**  — 使用先前練習中的教訓，並使用樣式系統來為容器元 **件實** 作固定 **寬度** 和流動寬度樣式。

## 恭喜！ {#congratulations}

恭喜，文章頁面幾乎已完整樣式，而且您使用AEM樣式系統獲得實作體驗。

### 後續步驟 {#next-steps}

了解端對端步驟，建立可顯示對話方塊中撰寫內容的[自訂AEM元件](custom-component.md)，並探索開發Sling模型以封裝填入元件HTL的商業邏輯。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git列`tutorial/style-system-solution`上檢閱並將程式碼部署於本機。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`tutorial/style-system-solution`分支。
