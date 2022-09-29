---
title: 了解如何為AEM樣式系統編碼
description: 在此影片中，我們將了解使用樣式系統來設定Adobe Experience Manager核心標題元件樣式的CSS（或LESS）和JavaScript，以及這些樣式如何套用至HTML和DOM。
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 0%

---

# 了解如何編寫樣式系統的代碼{#understanding-how-to-code-for-the-aem-style-system}

在此影片中，我們將審視CSS(或 [!DNL LESS])和JavaScript，以及這些樣式如何套用至HTML和DOM，來設定使用樣式系統來設定Experience Manager核心標題元件的樣式。


## 了解如何編寫樣式系統的代碼 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=12&learn=on)

提供的AEM套件(**technical-review.sites.style-system-1.0.0.zip**)會安裝範例標題樣式、We.Retail配置容器和標題元件的範例原則，以及範例頁面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是 [!DNL LESS] 在以下位置找到的示例樣式的定義：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

對於偏好CSS的使用者，此程式碼片段下方為此的CSS [!DNL LESS] 編譯為。

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

上述 [!DNL LESS] 由Experience Manager以原生方式編譯為下列CSS。

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

將範例樣式套用至標題元件時，下列JavaScript會收集並插入目前頁面的上次修改日期和時間。

jQuery的使用是可選的，也是使用的命名慣例。

以下是 [!DNL LESS] 在以下位置找到的示例樣式的定義：

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

### HTML最佳實務 {#html-best-practices}

* HTML（透過HTL產生）應盡可能具有結構語義；避免元素的不必要分組/巢狀。
* HTML元素應可透過BEM樣式的CSS類別定址。

**好**  — 元件中的所有元素都可透過BEM記號定址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**壞**  — 清單和清單元素只能依元素名稱定址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公開更多資料並隱藏它比公開太少的資料要好，這些資料需要未來的後端開發才能公開它。

   * 實作可供作者切換的內容有助於保持此HTML精簡，讓作者能夠選取要寫入HTML的內容元素。 將影像寫入HTML時，可能不會用於所有樣式時，可能特別重要。
   * 此規則的例外情況是，昂貴的資源（例如影像）預設會公開，因為CSS隱藏的事件影像在此情況下會不必要地擷取。

      * 現代化的影像元件通常會使用JavaScript來選取並載入使用案例（檢視區）最適當的影像。

### CSS最佳作法 {#css-best-practices}

>[!NOTE]
>
>風格體系使得技術上與 [BEM](https://en.bem.info/)，在 `BLOCK` 和 `BLOCK--MODIFIER` 不會套用至相同的元素，如所指定 [BEM](https://en.bem.info/).
>
>相反地，由於產品限制， `BLOCK--MODIFIER` 會套用至的父項 `BLOCK` 元素。
>
>其他租戶 [BEM](https://en.bem.info/) 應與一致。

* 使用前置處理器，例如 [較少](https://lesscss.org/) (由AEM原生支援)或 [SCSS](https://sass-lang.com/) （需要自訂建置系統），以允許清除CSS定義和重新可用性。

* 保持選擇器重量/特異性一致；這有助於避免和解決難以識別的CSS階層衝突。
* 將每個樣式組織為離散檔案。
   * 可使用LESS/SCSS組合這些檔案 `@imports` 或者，如果需要原始CSS，可透過「HTML用戶端程式庫」檔案包含，或自訂前端資產建置系統。
* 避免混合許多複雜的樣式。
   * 一次可套用至元件的樣式越多，排列的變化就越多。 這可能會變得難以維護/QA/確保品牌一致性。
* 請一律使用CSS類別（遵循BEM標籤法）來定義CSS規則。
   * 如果絕對需要選取不含CSS類的元素（即裸元素），請在CSS定義中將其移到較高的位置，以清楚表明這些元素的特異性比與該類型的元素（具有可選的CSS類）的任何衝突低。
* 避免使用樣式 `BLOCK--MODIFIER` 直接在回應式格線上。 更改此元素的顯示可能會影響響應網格的呈現和功能，因此，當更改響應網格的行為時，只有在此級別上的樣式。
* 使用應用樣式範圍 `BLOCK--MODIFIER`. 此 `BLOCK__ELEMENT--MODIFIERS` 可用於元件，但由於 `BLOCK` 代表元件，元件是已設定樣式的元件，樣式是「已定義」且範圍是透過 `BLOCK--MODIFIER`.

範例CSS選取器結構應如下：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第一級選擇器</p> <p>塊 — 修飾符</p> </td> 
   <td valign="bottom"><p>第2級選擇器</p> <p>區塊</p> </td> 
   <td valign="bottom"><p>第3級選擇器</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS選取器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list-dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list-dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> 顏色：藍色；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> 顏色：紅色；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

如果是巢狀元件，這些巢狀元件元素的CSS選取器深度將超過第3級選取器。 對巢狀元件重複相同的模式，但由父元件限定範圍 `BLOCK`. 換句話說，啟動巢狀元件的 `BLOCK` 在第3層，且巢狀元件的 `ELEMENT` 位於第4個選取器層級。

### JavaScript最佳實務 {#javascript-best-practices}

本節中定義的最佳實務涉及「style-JavaScript」，或專門用於以風格而非功能目的操控元件的JavaScript。

* Style-JavaScript應審慎使用，且為少數使用案例。
* Style-JavaScript主要用於控制元件的DOM，以支援CSS的樣式。
* 如果元件在頁面上出現多次，請重新評估Javascript的使用，並了解計算/重新繪製成本。
* 如果元件在頁面上出現多次，Javascript以非同步方式(透過AJAX)提取新資料/內容，請重新評估使用。
* 同時處理發佈和製作體驗。
* 盡可能重複使用style-Javascript。
   * 例如，如果元件的多個樣式要求將其影像移至背景影像，則style-JavaScript可實作一次，並附加至多個 `BLOCK--MODIFIERs`.
* 盡可能將style-JavaScript與功能JavaScript分開。
* 直接透過HTL評估HTML中JavaScript與顯現這些DOM變更的成本。
   * 當使用style-JavaScript的元件需要伺服器端修改時，請評估此時是否可以導入JavaScript操作，以及影響/影響元件的效能和支援性。

#### 效能考量 {#performance-considerations}

* Style-JavaScript應保持輕薄。
* 為了避免閃爍和不必要的重繪，最初通過 `BLOCK--MODIFIER BLOCK`，並在JavaScript中的所有DOM操作完成時顯示。
* style-JavaScript操作的效能類似於附加到和修改DOMReady上元素的基本jQuery外掛程式。
* 確保請求已壓縮，且CSS和JavaScript已縮制。

## 其他資源 {#additional-resources}

* [樣式系統文檔](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立AEM Client程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（區塊元素修飾詞）檔案網站](https://getbem.com/)
* [LESS文檔網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
