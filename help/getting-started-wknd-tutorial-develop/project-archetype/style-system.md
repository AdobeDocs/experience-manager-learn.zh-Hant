---
title: 以體制發展
seo-title: Developing with the Style System
description: 瞭解如何使用Experience Manager的樣式系統實施單個樣式並重新使用核心元件。 本教程介紹如何開發樣式系統以使用特定於品牌的CSS和模板編輯器的高級策略配置來擴展核心元件。
sub-product: sites
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
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# 以體制發展 {#developing-with-the-style-system}

瞭解如何使用Experience Manager的樣式系統實施單個樣式並重新使用核心元件。 本教程介紹如何開發樣式系統以使用特定於品牌的CSS和模板編輯器的高級策略配置來擴展核心元件。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

此外，還建議對 [客戶端庫和前端工作流](client-side-libraries.md) 教程，瞭解客戶端庫的基礎知識以及項目中內置的各種前端AEM工具。

### 入門項目

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出啟動程式項目的步驟。

檢查本教程所基於的基線代碼：

1. 查看 `tutorial/style-system-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) 或通過切換到分支本地檢出代碼 `tutorial/style-system-solution`。

## 目標

1. 瞭解如何使用樣式系統將特定於品牌的CSS應用於核AEM心元件。
1. 瞭解BEM記法以及如何使用它仔細確定樣式的範圍。
1. 使用可編輯模板應用高級策略配置。

## 您將構建的 {#what-you-will-build}

在本章中，我們將使用 [樣式系統功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) 建立 **標題** 和 **文本** 「文章」頁上使用的元件。

![可用於標題的樣式](assets/style-system/styles-added-title.png)

*可用於標題元件的下划線樣式*

## 背景 {#background}

的 [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) 允許開發人員和模板編輯器建立元件的多個可視變體。 然後，作者可以決定在合成頁面時使用哪種樣式。 在本教程的其餘部分中，我們將利用Style System實現幾種獨特的樣式，同時以低代碼方法利用核心元件。

Style System的一般思想是，作者可以選擇元件外觀的各種樣式。 「樣式」由注入到元件外div的其他CSS類作後盾。 在客戶端庫中，會根據這些樣式類添加CSS規則，以便元件更改外觀。

您可以找到 [此處是Style System的詳細文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html)。 還有一個 [瞭解Style系統的技術視頻](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)。

## 下划線樣式 — 標題 {#underline-style}

的 [標題元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) 已被代理到項目中 `/apps/wknd/components/title` 作為 **ui.apps** 中。 標題元素的預設樣式(`H1`。 `H2`。 `H3`..)已在 **ui.frontend** 中。

的 [WKND物品設計](assets/pages-templates/wknd-article-design.xd) 包含帶下划線的「標題」元件的唯一樣式。 「樣式系統」(Style System)不能建立兩個元件或修改元件對話框，而是允許作者使用選項添加下划線樣式。

![下划線樣式 — 標題元件](assets/style-system/title-underline-style.png)

### 添加標題策略

為「標題」元件添加新策略，以允許內容作者選擇「下划線」樣式以應用於特定元件。 這是使用中的模板編輯器完AEM成的。

1. 導航到 **文章頁** 模板位於： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在 **結構** 模式，在主 **佈局容器**，選擇 **策略** 表徵圖 **標題** 列出的元件 *允許的元件*:

   ![標題策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 為「標題」元件建立具有以下值的新策略：

   *策略標題**: **WKND標題**

   *屬性* > *樣式頁籤* > *添加新樣式*

   **下划線** : `cmp-title--underline`

   ![標題的樣式策略配置](assets/style-system/title-style-policy.png)

   按一下 **完成** 的子菜單。

   >[!NOTE]
   >
   > 值 `cmp-title--underline` 填充元件HTML標籤外div上的CSS類。

### 應用下划線樣式

作者將下划線樣式應用於某些標題元件。

1. 導航到 **拉滑球場** AEM Sites編輯： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 的子菜單。 按一下 **彩筆** 表徵圖，然後選擇 **下划線** 樣式：

   ![應用下划線樣式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此時，將不會發生可見的更改，因為 `underline` 尚未實現樣式。 在下次練習中，將實現此樣式。

1. 按一下 **頁面資訊** 表徵圖 **查看為已發佈** 檢查編輯器外的AEM頁面。
1. 使用瀏覽器開發人員工具驗證Title元件周圍的標籤是否具有CSS類 `cmp-title--underline` 應用於外div。

   ![應用下划線類的Div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 實施下划線樣式 — ui.frontend

接下來，使用 **ui.frontend** 項目模組。 我們將使用與 **ui.frontend** 預覽樣式的模組 *先* 部署到的本地實AEM例。

1. 啟動 `watch` 從內部處理 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   這將啟動監視 `ui.frontend` 模組並同步對實例的更AEM改。


1. 返回IDE並開啟檔案 `_title.scss` 位於： `ui.frontend/src/main/webpack/components/_title.scss`。
1. 引入針對 `cmp-title--underline` 類：

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
   >始終將樣式嚴格限定到目標元件是一種最佳做法。 這可確保額外樣式不會影響頁面的其他區域。
   >
   >所有核心元件都與 **[BEM表示法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。 為元件建立預設樣式時，最好將目標定在外部CSS類。 另一個最佳做法是將核心元件邊界元表示法指定的類名作為目標，而不是HTML元素。

1. 返回瀏覽器和AEM頁面。 您應看到添加的下划線樣式：

   ![Webpack dev伺服器中可見的下划線樣式](assets/style-system/underline-implemented-webpack.png)

1. 在編AEM輯器中，您現在應能 **下划線** 樣式，並查看直觀反映的更改。

## 報價塊樣式 — 文本 {#text-component}

接下來，重複類似步驟，將唯一樣式應用於 [文本元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)。 Text元件已被代理到項目 `/apps/wknd/components/text` 作為 **ui.apps** 中。 已在中實現段落元素的預設樣式 **ui.frontend**。

的 [WKND物品設計](assets/pages-templates/wknd-article-design.xd) 包含帶引號塊的Text元件的唯一樣式：

![報價塊樣式 — 文本元件](assets/style-system/quote-block-style.png)

### 添加文本策略

接下來為文本元件添加新策略。

1. 導航到 **文章頁面模板** 位於： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)。

1. 在 **結構** 模式，在主 **佈局容器**，選擇 **策略** 表徵圖 **文本** 列出的元件 *允許的元件*:

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本元件策略：

   *策略標題**: **內容文本**

   *插件* > *段落樣式* > *啟用段落樣式*

   *樣式頁籤* > *添加新樣式*

   **報價塊** : `cmp-text--quote`

   ![文本元件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本元件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   按一下 **完成** 的子菜單。

### 應用報價塊樣式

1. 導航到 **拉滑球場** AEM Sites編輯： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 的子菜單。 編輯元件以包括報價元素：

   ![文本元件配置](assets/style-system/configure-text-component.png)

1. 選擇文本元件，然後按一下 **彩筆** 表徵圖，然後選擇 **報價塊** 樣式：

   ![應用報價塊樣式](assets/style-system/quote-block-style-applied.png)

1. 使用瀏覽器的開發人員工具檢查標籤。 您應看到類名 `cmp-text--quote` 已添加到元件的外div中：

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 實施報價塊樣式 — ui.frontend

接下來，我們將使用 **ui.frontend** 項目模組。

1. 如果尚未運行，請啟動 `watch` 從內部處理 **ui.frontend** 模組：

   ```shell
   $ npm run watch
   ```

1. 更新檔案 `text.scss` 位於： `ui.frontend/src/main/webpack/components/_text.scss`:

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
   > 在這種情況下，原始HTML元素被樣式鎖定。 這是因為文本元件為內容作者提供了富格文本編輯器。 直接針對RTE內容建立樣式應謹慎進行，而更重要的是要嚴格地審查這些樣式。

1. 再次返回到瀏覽器，您應看到添加的報價塊樣式：

   ![報價塊樣式可見](assets/style-system/quoteblock-implemented.png)

1. 停止Webpack開發伺服器。

## 固定寬度 — 容器（附加） {#layout-container}

容器元件已用於建立文章頁面模板的基本結構，並為內容作者提供用於在頁面上添加內容的放置區域。 容器還可以利用「樣式系統」，為內容作者提供更多設計佈局的選項。

的 **主容器** 「文章頁」模板的「」(Article Page)中包含兩個可自動建立的容器，且寬度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章頁面模板中的主容器*。

的策略 **主容器** 將預設元素設定為 `main`:

![主容器策略](assets/style-system/main-container-policy.png)

建立 **主容器** 固定設定在 **ui.frontend** 模組 `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

而不是瞄準 `main` HTML元素，「樣式系統」可用於建立 **固定寬度** 樣式作為容器策略的一部分。 Style系統可讓用戶選擇在 **固定寬度** 和 **流體寬度** 容器。

1. **獎金挑戰**  — 使用從以前練習中吸取的經驗教訓，並使用樣式系統 **固定寬度** 和 **流體寬度** 樣式。

## 恭喜！ {#congratulations}

恭喜，文章頁面幾乎完全設計，您使用Style System獲得了實際操作AEM體驗。

### 後續步驟 {#next-steps}

瞭解建立 [自定義組AEM件](custom-component.md) 顯示在對話框中創作的內容，並探索開發Sling模型以封裝填充元件HTL的業務邏輯。

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git框上本地查看和部署代碼 `tutorial/style-system-solution`。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 儲存庫。
1. 查看 `tutorial/style-system-solution` 分支。
