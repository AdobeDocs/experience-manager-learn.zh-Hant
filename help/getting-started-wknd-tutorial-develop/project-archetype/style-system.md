---
title: 與風格體系一起發展
seo-title: Developing with the Style System
description: 了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋使用樣式系統開發，以使用品牌專屬CSS和範本編輯器的進階政策設定來擴充核心元件。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '1678'
ht-degree: 0%

---

# 與風格體系一起發展 {#developing-with-the-style-system}

了解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋使用樣式系統開發，以使用品牌專屬CSS和範本編輯器的進階政策設定來擴充核心元件。

## 必備條件 {#prerequisites}

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

建議您檢閱 [用戶端程式庫和前端工作流程](client-side-libraries.md) 教學課程，了解用戶端程式庫的基本知識，以及AEM專案內建的各種前端工具。

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重複使用項目，並跳過簽出起始項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看 `tutorial/style-system-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，請附加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) 或切換到分支以本地簽出代碼 `tutorial/style-system-solution`.

## 目標

1. 了解如何使用樣式系統將品牌專屬的CSS套用至AEM核心元件。
1. 了解BEM記號，以及如何用來仔細調整樣式。
1. 使用可編輯的模板應用高級策略配置。

## 您要建置的 {#what-build}

本章使用 [樣式系統功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) 建立變數 **標題** 和 **文字** 「文章」頁面上使用的元件。

![標題可用樣式](assets/style-system/styles-added-title.png)

*可用於標題元件的下划線樣式*

## 背景 {#background}

此 [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) 可讓開發人員和範本編輯器建立元件的多個視覺變數。 然後，作者可決定在撰寫頁面時要使用哪種樣式。 在教學課程的其餘部分中，都會使用樣式系統來達成數種獨特的樣式，同時以低程式碼方式使用核心元件。

樣式系統的一般概念是，作者可選擇元件外觀的各種樣式。 「styles」由會插入至元件外部div的其他CSS類別作為後盾。 在用戶端程式庫中，會根據這些樣式類來新增CSS規則，以便元件變更外觀。

您可以找到 [此處提供樣式系統的詳細文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html). 還有一個很棒的 [了解樣式系統的技術影片](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## 下划線樣式 — 標題 {#underline-style}

此 [標題元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) 已被推廣至 `/apps/wknd/components/title` 作為 **ui.apps** 模組。 標題元素的預設樣式(`H1`, `H2`, `H3`...已在 **ui.frontend** 模組。

此 [WKND文章設計](assets/pages-templates/wknd-article-design.xd) 包含帶下划線的Title元件的唯一樣式。 「樣式系統」(Style System)可用於允許作者添加下划線樣式的選項，而不是建立兩個元件或修改元件對話框。

![下划線樣式 — 標題元件](assets/style-system/title-underline-style.png)

### 添加標題策略

現在為「標題」元件新增原則，讓內容作者可以選擇要套用至特定元件的「底線」樣式。 這是使用AEM中的範本編輯器來完成。

1. 導覽至 **文章頁面** 範本來源： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在 **結構** 模式，主 **版面容器**，請選取 **原則** 表徵圖 **標題** 列於 *允許的元件*:

   ![標題策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 使用以下值為Title元件建立策略：

   *策略標題&#42;*: **WKND標題**

   *屬性* > *樣式標籤* > *新增樣式*

   **畫底線** : `cmp-title--underline`

   ![標題的樣式策略配置](assets/style-system/title-style-policy.png)

   按一下 **完成** 保存對「標題」策略的更改。

   >[!NOTE]
   >
   > 值 `cmp-title--underline` 在元件HTML標籤的外部div上填入CSS類別。

### 應用下划線樣式

身為作者，我們將底線樣式套用至特定標題元件。

1. 導覽至 **拉斯卡特帕克斯** AEM Sites編輯的文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 模式，選擇標題元件。 按一下 **彩刷** 圖示並選取 **畫底線** 樣式：

   ![應用下划線樣式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此時，不會發生顯示變更，因為 `underline` 樣式尚未實施。 在下一個練習中，將實施此樣式。

1. 按一下 **頁面資訊** 圖示> **檢視為已發佈** 在AEM編輯器外部檢查頁面。
1. 使用您的瀏覽器開發人員工具來驗證標題元件周圍的標籤是否有CSS類別 `cmp-title--underline` 套用至外部div。

   ![已應用下划線類的DIV](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 實作底線樣式 — ui.frontend

接下來，使用 **ui.frontend** AEM專案的模組。 與 **ui.frontend** 預覽樣式的模組 *befor* 會使用部署至AEM的本機例項。

1. 啟動 `watch` 從 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   這會啟動監控 `ui.frontend` 模組，並將變更同步至AEM例項。


1. 返回IDE並開啟檔案 `_title.scss` 從： `ui.frontend/src/main/webpack/components/_title.scss`.
1. 導入以 `cmp-title--underline` 類別：

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
   >將樣式一律嚴格限定至目標元件，即視為最佳作法。 這可確保額外的樣式不會影響頁面的其他區域。
   >
   >所有核心元件均符合 **[BEM標籤法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. 為元件建立預設樣式時，最佳作法是定位外部CSS類別。 另一個最佳實務是鎖定核心元件BEM標籤法所指定的類別名稱，而非HTML元素。

1. 返回瀏覽器和AEM頁面。 您應該會看到已新增底線樣式：

   ![Webpack開發伺服器中可見的下划線樣式](assets/style-system/underline-implemented-webpack.png)

1. 在AEM編輯器中，您現在應該可以開啟和關閉 **畫底線** 樣式，並查看變更以視覺化方式反映。

## 報價塊樣式 — 文本 {#text-component}

接下來，重複類似步驟，將唯一樣式套用至 [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). Text元件已複製至 `/apps/wknd/components/text` 作為 **ui.apps** 模組。 段落元素的預設樣式已在 **ui.frontend**.

此 [WKND文章設計](assets/pages-templates/wknd-article-design.xd) 包含帶引號塊的Text元件的唯一樣式：

![報價塊樣式 — 文本元件](assets/style-system/quote-block-style.png)

### 添加文本策略

接下來，為文本元件添加策略。

1. 導覽至 **文章頁面範本** 從： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. 在 **結構** 模式，主 **版面容器**，請選取 **原則** 表徵圖 **文字** 列於 *允許的元件*:

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本元件策略：

   *策略標題&#42;*: **內容文字**

   *外掛程式* > *段落樣式* > *啟用段落樣式*

   *樣式標籤* > *新增樣式*

   **報價塊** : `cmp-text--quote`

   ![文本元件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本元件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   按一下 **完成** 保存對文本策略的更改。

### 應用引號塊樣式

1. 導覽至 **拉斯卡特帕克斯** AEM Sites編輯的文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 模式，選擇文本元件。 編輯元件以包含報價元素：

   ![文字元件設定](assets/style-system/configure-text-component.png)

1. 選取文字元件，然後按一下 **彩刷** 圖示並選取 **報價塊** 樣式：

   ![應用引號塊樣式](assets/style-system/quote-block-style-applied.png)

1. 使用瀏覽器的開發人員工具來檢查標籤。 您應該會看到類別名稱 `cmp-text--quote` 已新增至元件的外部div:

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

接下來，我們使用 **ui.frontend** AEM專案的模組。

1. 如果尚未執行，請啟動 `watch` 從 **ui.frontend** 模組：

   ```shell
   $ npm run watch
   ```

1. 更新檔案 `text.scss` 從： `ui.frontend/src/main/webpack/components/_text.scss`:

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
   > 在此案例中，原始HTML元素會由樣式定位。 這是因為「文字」元件為內容作者提供RTF編輯器。 針對RTE內容直接建立樣式時應謹慎操作，而將樣式嚴格限制則更為重要。

1. 再次返回瀏覽器，您應該會看到已新增報價區塊樣式：

   ![可見報價塊樣式](assets/style-system/quoteblock-implemented.png)

1. 停止Webpack開發伺服器。

## 固定寬度 — 容器（額外） {#layout-container}

容器元件可用來建立「文章頁面範本」的基本結構，並提供內容作者可在頁面上新增內容的放置區。 容器也可使用樣式系統，為內容作者提供更多版面設計選項。

此 **主容器** 「文章頁面」範本包含兩個可作者的容器，且寬度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章頁面範本中的主容器*.

本 **主容器** 將預設元素設為 `main`:

![主容器策略](assets/style-system/main-container-policy.png)

建立 **主容器** 已在 **ui.frontend** 模組於 `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

而非定位 `main` HTML元素，可以使用樣式系統來建立 **固定寬度** 樣式（容器策略的一部分）。 樣式系統可讓使用者選擇切換 **固定寬度** 和 **流體寬度** 容器。

1. **獎金挑戰**  — 利用從以前練習中吸取的經驗教訓，並使用樣式系統來實施 **固定寬度** 和 **流體寬度** 容器元件的樣式。

## 恭喜！ {#congratulations}

恭喜，文章頁面已接近樣式，而且您使用AEM樣式系統獲得實作體驗。

### 後續步驟 {#next-steps}

了解建立 [自訂AEM元件](custom-component.md) 可顯示對話方塊中撰寫的內容，並探索開發Sling模型來封裝商業邏輯，以填入元件的HTL。

在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本機的Git分支檢閱並部署程式碼 `tutorial/style-system-solution`.

1. 複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/style-system-solution` 分支。
