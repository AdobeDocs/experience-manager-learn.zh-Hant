---
title: 瞭解如何為AEM樣式系統編碼
description: 在這段影片中，我們將瞭解CSS （或LESS）和JavaScript的剖析；這兩種樣式用於透過「樣式系統」設定Adobe Experience Manager的核心標題元件的樣式，以及如何將這些樣式套用至HTML和DOM。
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# 瞭解如何編寫樣式系統的程式碼{#understanding-how-to-code-for-the-aem-style-system}

在本影片中，我們將瞭解CSS （或[!DNL LESS]）和JavaScript的剖析，這些樣式用於透過「樣式系統」設定Experience Manager的核心標題元件的樣式，以及如何將這些樣式套用至HTML和DOM。


## 瞭解如何編寫樣式系統的程式碼 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供的AEM套件(**technical-review.sites.style-system-1.0.0.zip**)會安裝範例標題樣式、We.Retail配置容器和標題元件的範例原則，以及範例頁面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是範例樣式的[!DNL LESS]定義，發現位置為：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

若偏好使用CSS，此程式碼片段下方是此[!DNL LESS]編譯的CSS。

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

上述[!DNL LESS]由Experience Manager以原生方式編譯成下列CSS。

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

將範例樣式套用至標題元件時，下列JavaScript會收集並插入標題文字下目前頁面的上次修改日期和時間。

jQuery的使用是選用的，也可以選擇使用的命名慣例。

以下是範例樣式的[!DNL LESS]定義，發現位置為：

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

## 開發最佳實務 {#development-best-practices}

### HTML最佳作法 {#html-best-practices}

* HTML （透過HTL產生）應在結構上儘可能提供語意，避免對元素進行不必要的分組/巢狀。
* HTML元素應可透過BEM樣式CSS類別進行定址。

**良好** — 元件中的所有元素均可透過BEM標籤法來定址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**錯誤** — 清單和清單專案只能依專案名稱定址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公開更多資料並隱藏資料，總比公開太少的資料（需要未來的後端開發）來公開資料要好。

   * 實作可製作的內容切換可協助讓此HTML保持精簡狀態，讓作者能夠選取要將哪些內容元素寫入HTML。 在將影像寫入可能無法用於所有樣式的HTML時，尤其重要。
   * 此規則的例外情況是昂貴資源（例如影像）依預設會公開，因為在此情況下，CSS隱藏的事件影像會不必要地擷取。

      * 現代影像元件通常會使用JavaScript來選取並載入最適合使用案例（檢視區）的影像。

### CSS最佳作法 {#css-best-practices}

>[!NOTE]
>
>樣式系統與[BEM](https://en.bem.info/)有小的技術差異，因為`BLOCK`和`BLOCK--MODIFIER`未套用至由[BEM](https://en.bem.info/)指定的相同專案。
>
>由於產品限制，`BLOCK--MODIFIER`已套用至`BLOCK`專案的父項。
>
>[BEM](https://en.bem.info/)的所有其他租使用者都應該對齊。

* 使用前置處理器，例如[LESS](https://lesscss.org/) (由AEM原生支援)或[SCSS](https://sass-lang.com/) （需要自訂建置系統），以便能夠清除CSS定義且可重複使用。

* 保持選擇器的權重/特異性一致；這有助於避免和解決難以識別的CSS重疊顯示衝突。
* 將每個樣式組織成獨立檔案。
   * 這些檔案可以使用LESS/SCSS `@imports`合併，或者在需要原始CSS時，透過HTML使用者端資料庫檔案包含或自訂前端資產建置系統合併。
* 避免混合許多複雜的樣式。
   * 單一時間可套用至元件的樣式越多，排列的多樣性就越多。 這可能變得難以維護/QA/確保品牌一致性。
* 一律使用CSS類別（遵循BEM標籤法）來定義CSS規則。
   * 如果選取不含CSS類別的元素（即裸元素）是絕對必要的，請在CSS定義中將其移至較高位置，以清楚表明其特定性低於與該型別元素發生的可選CSS類別的所有衝突。
* 避免直接設定`BLOCK--MODIFIER`的樣式，因為這會附加至回應式格線。 變更此元素的顯示可能會影響回應式格線的演算和功能，因此只有在意圖變更回應式格線的行為時才會在此層級顯示樣式。
* 使用`BLOCK--MODIFIER`套用樣式範圍。 `BLOCK__ELEMENT--MODIFIERS`可以在元件中使用，但由於`BLOCK`代表元件，且元件是樣式化的，所以樣式是透過`BLOCK--MODIFIER`的「已定義」且已設定範圍。

CSS選取器結構範例應如下所示：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第一層選擇器</p> <p>區塊 — 修飾元</p> </td> 
   <td valign="bottom"><p>第二層級選擇器</p> <p>區塊</p> </td> 
   <td valign="bottom"><p>第3層選擇器</p> <p>區塊__素</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS選取器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — 深色</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — 深色</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> color： blue；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color： red；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

若是巢狀元件，這些巢狀「元件」元素的CSS選取器深度將超過第3層選取器。 對巢狀元件重複相同的模式，但範圍為父元件的`BLOCK`。 換句話說，從第3層級開始巢狀元件的`BLOCK`，而巢狀元件的`ELEMENT`則在第4層級選擇器。

### JavaScript最佳作法 {#javascript-best-practices}

本節中定義的最佳實務與「style-JavaScript」有關，或JavaScript專門為了文體而非功能目的而操控元件。

* Style-JavaScript應審慎使用，且為少數使用案例。
* Style-JavaScript應該主要用於操控元件的DOM，以支援CSS的樣式設定。
* 如果元件會在頁面上出現多次，請重新評估Javascript的使用，並瞭解計算/和重新繪製成本。
* 如果Javascript在元件可能在頁面上出現多次時非同步(透過AJAX)提取新資料/內容，請重新評估其使用。
* 處理發佈和編寫體驗。
* 儘可能重複使用style-Javascript。
   * 例如，如果元件的多個樣式需要將影像移至背景影像，style-JavaScript可以實作一次，並附加至多個`BLOCK--MODIFIERs`。
* 儘可能將樣式JavaScript與功能性JavaScript分開。
* 評估JavaScript的成本與直接透過HTL在HTML中顯示這些DOM變更的成本。
   * 當使用style-JavaScript的元件需要伺服器端修改時，請評估此時是否可以引入JavaScript操作，以及這對元件的效能和支援性有何影響/影響。

#### 效能考量事項 {#performance-considerations}

* Style-JavaScript應保持輕薄和纖薄。
* 若要避免忽隱忽現和不必要的重新繪製，請先透過`BLOCK--MODIFIER BLOCK`隱藏元件，並在JavaScript中的所有DOM操作完成時顯示它。
* style-JavaScript操作的效能類似於jQuery基本外掛程式，可附加及修改DOMReady上的元素。
* 確保請求已壓縮，且CSS和JavaScript已縮制。

## 其他資源 {#additional-resources}

* [樣式系統檔案](https://helpx.adobe.com/tw/experience-manager/6-5/sites/authoring/using/style-system.html)
* [正在建立AEM使用者端資料庫](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM （區塊元素修飾元）檔案網站](https://getbem.com/)
* [較少說明檔案網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
