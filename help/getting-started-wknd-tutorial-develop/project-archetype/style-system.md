---
title: 使用樣式系統進行開發
description: 瞭解如何使用Experience Manager的樣式系統來實作個別樣式並重複使用核心元件。 本教學課程涵蓋樣式系統的開發，以使用範本編輯器的品牌特定CSS和進階原則設定來擴充核心元件。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# 使用樣式系統進行開發 {#developing-with-the-style-system}

瞭解如何使用Experience Manager的樣式系統來實作個別樣式並重複使用核心元件。 本教學課程涵蓋樣式系統的開發，以使用範本編輯器的品牌特定CSS和進階原則設定來擴充核心元件。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

也建議檢閱[使用者端資料庫和前端工作流程](client-side-libraries.md)教學課程，以瞭解使用者端資料庫的基礎知識以及內建在AEM專案中的各種前端工具。

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

檢視教學課程建置的基底程式碼：

1. 從[GitHub](https://github.com/adobe/aem-guides-wknd)檢視`tutorial/style-system-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)上檢視完成的程式碼，或切換至分支`tutorial/style-system-solution`在本機簽出程式碼。

## 目標

1. 瞭解如何使用樣式系統將品牌特定的CSS套用至AEM核心元件。
1. 瞭解BEM標籤法，以及如何使用它來仔細設定樣式的範圍。
1. 使用可編輯的範本套用進階原則設定。

## 您即將建置的內容 {#what-build}

本章使用[樣式系統功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)來建立文章頁面上所使用之&#x200B;**Title**&#x200B;和&#x200B;**Text**&#x200B;元件的變體。

標題](assets/style-system/styles-added-title.png)的![可用樣式

*標題元件可用的底線樣式*

## 背景 {#background}

[樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html)可讓開發人員和範本編輯器建立元件的多個視覺變體。 接著，作者就可以決定構成頁面時要使用的樣式。 樣式系統會在本教學課程的其餘部分中使用，以在低程式碼方法使用核心元件時獲得數個獨特樣式。

樣式系統的一般構想是，作者可以選擇元件的各種樣式。 「樣式」受到插入元件外部div的其他CSS類別支援。 在使用者端資料庫中，會根據這些樣式類別新增CSS規則，讓元件能夠變更外觀。

您可以在這裡找到[樣式系統的詳細檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html)。 還有一段很棒的[技術影片，可讓您瞭解樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)。

## 底線樣式 — 標題 {#underline-style}

[標題元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html)已代理至`/apps/wknd/components/title`下的專案，作為&#x200B;**ui.apps**&#x200B;模組的一部分。 標題元素(`H1`、`H2`、`H3`...)的預設樣式已在&#x200B;**ui.frontend**&#x200B;模組中實作。

[WKND文章設計](assets/pages-templates/wknd-article-design.xd)包含帶有底線的標題元件唯一樣式。 您可以使用「樣式系統」來允許作者加入底線樣式的選項，而不是建立兩個元件或修改元件對話方塊。

![底線樣式 — 標題元件](assets/style-system/title-underline-style.png)

### 新增標題原則

讓我們為Title元件新增一項原則，讓內容作者選擇要套用至特定元件的Underline樣式。 這是使用AEM中的範本編輯器完成。

1. 瀏覽至&#x200B;**文章頁面**&#x200B;範本，從： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在&#x200B;**結構**&#x200B;模式中，在主要的&#x200B;**配置容器**&#x200B;中，選取&#x200B;*允許的元件*&#x200B;下方所列的&#x200B;**標題**&#x200B;元件旁的&#x200B;**原則**&#x200B;圖示：

   ![標題原則設定](assets/style-system/article-template-title-policy-icon.png)

1. 使用下列值建立Title元件的原則：

   *原則標題&#42;*： **WKND標題**

   *屬性* > *樣式標籤* > *新增樣式*

   **底線** ： `cmp-title--underline`

   標題](assets/style-system/title-style-policy.png)的![樣式原則設定

   按一下&#x200B;**完成**&#x200B;儲存標題原則的變更。

   >[!NOTE]
   >
   > 值`cmp-title--underline`會填入元件HTML標籤的外部div上的CSS類別。

### 套用底線樣式

身為作者，我們可以將底線樣式套用至某些標題元件。

1. 導覽至AEM Sites編輯器中的&#x200B;**La Skateparks**&#x200B;文章，網址為： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**編輯**&#x200B;模式中，選擇標題元件。 按一下&#x200B;**繪圖筆刷**&#x200B;圖示並選取&#x200B;**底線**&#x200B;樣式：

   ![套用底線樣式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此時，不會發生可見的變更，因為`underline`樣式尚未實作。 在下一個練習中，會實作此樣式。

1. 按一下&#x200B;**頁面資訊**&#x200B;圖示> **以發佈的形式檢視**，以檢查AEM編輯器外部的頁面。
1. 使用您的瀏覽器開發人員工具，驗證Title元件周圍的標籤是否已將CSS類別`cmp-title--underline`套用至外部div。

   已套用底線類別的![Div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 實作底線樣式 — ui.frontend

接下來，使用AEM專案的&#x200B;**ui.frontend**&#x200B;模組實作Underline樣式。 使用&#x200B;**ui.frontend**&#x200B;模組隨附的Webpack開發伺服器，以在&#x200B;*部署到AEM的本機執行個體之前，預覽樣式*。

1. 從&#x200B;**ui.frontend**&#x200B;模組內啟動`watch`處理序：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   這會啟動監視`ui.frontend`模組變更並將變更同步到AEM執行個體的程式。


1. 傳回您的IDE並從下列位置開啟檔案`_title.scss`： `ui.frontend/src/main/webpack/components/_title.scss`。
1. 引入以`cmp-title--underline`類別為目標的新規則：

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
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
   >最佳實務就是一律將樣式嚴格限定為目標元件。 這樣可確保額外的樣式不會影響頁面的其他區域。
   >
   >所有核心元件都遵循&#x200B;**[BEM標籤法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。 建立元件的預設樣式時，最佳實務是鎖定外部CSS類別。 另一個最佳實務是鎖定核心元件BEM標籤法所指定的類別名稱，而非HTML元素。

1. 返回瀏覽器和AEM頁面。 您應該會看到底線樣式已新增：

   ![在webpack開發伺服器中顯示底線樣式](assets/style-system/underline-implemented-webpack.png)

1. 在AEM編輯器中，您現在應該能夠開啟或關閉&#x200B;**底線**&#x200B;樣式，並看到變更在視覺上反映。

## 引號區塊樣式 — 文字 {#text-component}

接著，重複類似的步驟，將唯一的樣式套用至[文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html)。 文字元件已代理至`/apps/wknd/components/text`下的專案，做為&#x200B;**ui.apps**&#x200B;模組的一部分。 段落元素的預設樣式已在&#x200B;**ui.frontend**&#x200B;中實作。

[WKND文章設計](assets/pages-templates/wknd-article-design.xd)包含具有引號區塊之文字元件的唯一樣式：

![引號區塊樣式 — 文字元件](assets/style-system/quote-block-style.png)

### 新增文字原則

接著，新增文字元件的原則。

1. 從[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)瀏覽至&#x200B;**文章頁面範本**。

1. 在&#x200B;**結構**&#x200B;模式中，在主要的&#x200B;**配置容器**&#x200B;中，選取&#x200B;*允許的元件*&#x200B;下列出的&#x200B;**文字**&#x200B;元件旁的&#x200B;**原則**&#x200B;圖示：

   ![文字原則設定](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文字元件原則：

   *原則標題&#42;*： **內容文字**

   *外掛程式* > *段落樣式* > *啟用段落樣式*

   *樣式標籤* > *新增樣式*

   **報價區塊**： `cmp-text--quote`

   ![文字元件原則](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文字元件原則2](assets/style-system/text-policy-enable-quotestyle.png)

   按一下&#x200B;**完成**&#x200B;以儲存文字原則的變更。

### 套用引號區塊樣式

1. 導覽至AEM Sites編輯器中的&#x200B;**La Skateparks**&#x200B;文章，網址為： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**編輯**&#x200B;模式中，選擇文字元件。 編輯元件以包含引號元素：

   ![文字元件設定](assets/style-system/configure-text-component.png)

1. 選取文字元件並按一下&#x200B;**繪圖筆刷**&#x200B;圖示，然後選取&#x200B;**報價區塊**&#x200B;樣式：

   ![套用引號區塊樣式](assets/style-system/quote-block-style-applied.png)

1. 使用瀏覽器的開發人員工具來檢查標籤。 您應該會看到類別名稱`cmp-text--quote`已新增至元件的外部div：

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 實作引號區塊樣式 — ui.frontend

接下來，讓我們使用AEM專案的&#x200B;**ui.frontend**&#x200B;模組來實作報價區塊樣式。

1. 如果尚未執行，請從&#x200B;**ui.frontend**&#x200B;模組內啟動`watch`處理序：

   ```shell
   $ npm run watch
   ```

1. 更新檔案`text.scss`，從： `ui.frontend/src/main/webpack/components/_text.scss`：

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
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
   > 在此情況下，樣式會鎖定原始HTML元素。 這是因為文字元件為內容作者提供RTF編輯器。 直接針對RTE內容建立樣式時，應謹慎進行，而且更重要的一點是應嚴格限定樣式的範圍。

1. 再次返回瀏覽器，您應該會看到已新增Quote區塊樣式：

   ![可見的引號區塊樣式](assets/style-system/quoteblock-implemented.png)

1. 停止webpack開發伺服器。

## 固定寬度 — 容器（額外優點） {#layout-container}

容器元件已用來建立文章頁面範本的基本結構，並提供放置區域，讓內容作者在頁面上新增內容。 容器也可以使用「樣式系統」，為內容作者提供設計版面的更多選項。

文章頁面範本的&#x200B;**主要容器**&#x200B;包含兩個可編寫的容器，且其寬度固定。

![主要容器](assets/style-system/main-container-article-page-template.png)

*文章頁面範本中的主要容器*。

**主要容器**&#x200B;的原則會將預設專案設定為`main`：

![主要容器原則](assets/style-system/main-container-policy.png)

將&#x200B;**主要容器**&#x200B;修正的CSS設定在`ui.frontend/src/main/webpack/site/styles/container_main.scss`的&#x200B;**ui.frontend**&#x200B;模組中：

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

樣式系統可用來建立&#x200B;**固定寬度**&#x200B;樣式，以做為容器原則的一部分，而非鎖定`main` HTML專案。 樣式系統可讓使用者選擇在&#x200B;**固定寬度**&#x200B;和&#x200B;**流動寬度**&#x200B;容器之間切換。

1. **額外挑戰** — 使用從先前練習中取得的課程，並使用樣式系統來實作Container元件的&#x200B;**固定寬度**&#x200B;和&#x200B;**流動寬度**&#x200B;樣式。

## 恭喜！ {#congratulations}

恭喜，文章頁面幾乎已設定樣式，而您透過AEM Style System獲得實作體驗。

### 後續步驟 {#next-steps}

瞭解建立[自訂AEM元件](custom-component.md) （顯示對話方塊中編寫的內容）的端對端步驟，並探索開發Sling模型以封裝填入元件HTL的業務邏輯。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/style-system-solution`上檢閱並部署本機的程式碼。

1. 複製[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存放庫。
1. 檢視`tutorial/style-system-solution`分支。
