---
title: 瞭解如何為樣式系AEM統編碼
description: 在此視頻中，我們將瞭解CSS（或LESS）和JavaScript的結構，以及這些樣式如何應用於HTML和DOM。
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 0%

---

# 瞭解如何為樣式系統編碼{#understanding-how-to-code-for-the-aem-style-system}

在此視頻中，我們將瞭解CSS(或 [!DNL LESS])和JavaScript，它們用於使用樣式系統對Experience Manage的核心標題元件進行樣式化，以及這些樣式如何應用於HTML和DOM。


## 瞭解如何為樣式系統編碼 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供的AEM包(**technical review.sites.style-system-1.0.0.zip**)安裝示例標題樣式、We.Retail Layout Container和Title元件的示例策略以及示例頁。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是 [!DNL LESS] 在以下位置找到示例樣式的定義：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

對於喜歡CSS的用戶，此代碼段下面是此代碼段的CSS [!DNL LESS] 編譯到。

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

以上 [!DNL LESS] 由Experience Manager本機編譯到以下CSS。

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### JavaScript {#example-javascript}

當將示例樣式應用到標題元件時，以下JavaScript將收集並彈出當前頁面的上次修改日期和時間，並將其放在標題文本下。

jQuery的使用是可選的，也是使用的命名約定。

以下是 [!DNL LESS] 在以下位置找到示例樣式的定義：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## 制定最佳做法 {#development-best-practices}

### HTML最佳做法 {#html-best-practices}

* HTML（通過HTL生成）應盡可能具有結構語義；避免元素的不必要的分組/嵌套。
* HTML元素應通過BEM樣式的CSS類可定址。

**好**  — 元件中的所有元素都可通過邊界元表示法定址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**壞**  — 清單和清單元素只能按元素名稱定址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 暴露更多資料和隱藏資料比暴露太少的資料更好，因為太少的資料需要將來的後端開發才能公開。

   * 實施可作者的內容切換有助於保持此HTML的精簡，從而作者能夠選擇將哪些內容元素寫入HTML。 在將影像寫入HTML時，不能用於所有樣式時，可能特別重要。
   * 此規則的例外情況是，預設情況下，昂貴的資源（例如，影像）會被暴露，因為CSS隱藏的事件影像會被不必要地提取。

      * 現代映像元件通常使用JavaScript來選擇並載入最適合用例（視區）的映像。

### CSS最佳做法 {#css-best-practices}

>[!NOTE]
>
>風格系統使得技術與 [邊界元](https://en.bem.info/)中 `BLOCK` 和 `BLOCK--MODIFIER` 未應用於指定的同一元素 [邊界元](https://en.bem.info/)。
>
>相反，由於產品約束， `BLOCK--MODIFIER` 應用於 `BLOCK` 的子菜單。
>
>所有其他租戶 [邊界元](https://en.bem.info/) 應與一致。

* 使用預處理器，如 [減](https://lesscss.org/) (由本AEM機支援)或 [SCSS](https://sass-lang.com/) （需要自定義生成系統），以允許清除CSS定義和重新使用。

* 保持選擇器重量/特異性一致；這有助於避免和解決難以識別的CSS級聯衝突。
* 將每個樣式組織成一個離散檔案。
   * 這些檔案可以使用LESS/SCSS組合 `@imports` 或者，如果需要原始CSS，請通過HTML客戶端庫檔案包含或自定義前端資產構建系統。
* 避免混合許多複雜的樣式。
   * 一次可以應用於元件的樣式越多，排列的變化就越多。 這可能會變得難以維護/QA/確保品牌協調。
* 始終使用CSS類（遵循BEM符號）定義CSS規則。
   * 如果絕對需要選擇沒有CSS類的元素（即裸元素），請在CSS定義中將其移到更高位置，以明確它們與確實具有可選CSS類的此類元素的衝突相比具有更低的特異性。
* 避免造型 `BLOCK--MODIFIER` 直接連接到響應網格。 更改此元素的顯示可能會影響響應網格的渲染和功能，因此只有當意圖更改響應網格的行為時，才會在此級別上使用樣式。
* 使用 `BLOCK--MODIFIER`。 的 `BLOCK__ELEMENT--MODIFIERS` 可在元件中使用，但 `BLOCK` 表示「元件」，「元件」是樣式，「樣式」是「定義的」，範圍通過 `BLOCK--MODIFIER`。

CSS選擇器結構示例應如下所示：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1級選擇器</p> <p>塊 — 修飾符</p> </td> 
   <td valign="bottom"><p>第2級選擇器</p> <p>阻止</p> </td> 
   <td valign="bottom"><p>3級選擇器</p> <p>塊_元素</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS選擇器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — 深色</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — 深色</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list_item { </span></strong></p> <p><strong> 顏色：藍色；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp影像 — 英雄</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp影像 — 英雄</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> 顏色：紅色；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

對於嵌套元件，這些嵌套的「元件」元素的CSS選擇器深度將超過第3級選擇器。 對嵌套元件重複相同的模式，但由父元件的 `BLOCK`。 換句話說，啟動嵌套元件 `BLOCK` 在第3級，嵌套的元件 `ELEMENT` 位於第4個選擇器級別。

### JavaScript最佳做法 {#javascript-best-practices}

本節中定義的最佳做法與「style-JavaScript」或JavaScript有關，JavaScript專門用於操作元件以進行風格操作，而不是用於功能目的。

* Style-JavaScript應當使用得當，而且是少數使用案例。
* Style-JavaScript應主要用於操作元件的DOM以支援CSS的樣式。
* 如果元件在頁面上出現多次，請重新評估Javascript的使用，並瞭解計算/重新繪製成本。
* 如果Javascript在元件可能多次出現在頁面上時非同步(通過AJAX)導入新資料/內容，請重新評估Javascript的使用。
* 處理發佈和創作體驗。
* 請盡可能重新使用style-Javascript。
   * 例如，如果元件的多個樣式要求將其影像移動到背景影像，則style-JavaScript可以實現一次並附加到多個 `BLOCK--MODIFIERs`。
* 盡可能將style-JavaScript與功能性的JavaScript分開。
* 評估JavaScript的成本與通過HTL直接在HTML中表示這些DOM更改的成本。
   * 當使用style-JavaScript的元件需要伺服器端修改時，請評估此時是否可以引入JavaScript操作，以及該元件的效能和支援性會受到哪些影響/影響。

#### 效能注意事項 {#performance-considerations}

* Style-JavaScript應保持輕巧和精簡。
* 為避免閃爍和不必要的重繪，最初將元件隱藏在 `BLOCK--MODIFIER BLOCK`，並在JavaScript中的所有DOM操作完成時顯示。
* style-JavaScript操作的效能類似於附加到DOMReady上的元素並修改這些元素的基本jQuery插件。
* 確保對請求進行壓縮，並且CSS和JavaScript被微型化。

## 其他資源 {#additional-resources}

* [樣式系統文檔](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立客AEM戶端庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（塊元素修改量）文檔網站](https://getbem.com/)
* [LESS文檔網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
