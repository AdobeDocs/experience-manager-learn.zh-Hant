---
title: 瞭解如何編寫AEM Style系統的程式碼
description: 在本影片中，我們將檢視使用樣式系統來設定Adobe Experience Manage核心標題元件樣式的CSS（或更少）和JavaScript，以及這些樣式如何套用至HTML和DOM。
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 0%

---


# 瞭解如何編寫樣式系統的代碼{#understanding-how-to-code-for-the-aem-style-system}

在此影片中，我們將檢視CSS（或[!DNL LESS]）和JavaScript的剖析結構，這些CSS（或&lt;a0/>）和JavaScript是使用樣式系統來設定Experience Manage的核心標題元件的樣式，以及這些樣式如何套用至HTML和DOM。

>[!NOTE]
>
>AEM Style System隨[AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)推出。
>
>視訊假設We.Retail Title元件已更新為繼承[核心元件v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)。

## 瞭解如何編寫樣式系統的代碼{#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

提供的AEM套件(**technical-review.sites.style-system-1.0.0.zip**)會安裝範例標題樣式、We.Retail Layout Container和Title元件的範例原則，以及範例頁面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是位於的示例樣式的[!DNL LESS]定義：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

對於偏好使用CSS的使用者，此程式碼片段下方是此[!DNL LESS]編譯的CSS。

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

上述[!DNL LESS]是由Experience Manager以原生方式編譯為下列CSS。

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

當範例樣式套用至標題元件時，下列JavaScript會收集目前頁面的上次修改日期和時間，並將其插入標題文字下方。

jQuery的使用是可選的，也是使用的命名慣例。

以下是位於的示例樣式的[!DNL LESS]定義：

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

## 開發最佳實務{#development-best-practices}

### HTML最佳範例{#html-best-practices}

* HTML（透過HTL產生）應盡可能具有結構語義；避免元素的不必要分組／巢狀。
* HTML元素應可透過BEM樣式的CSS類別定址。

**Good**  —— 元件中的所有元素都可通過BEM記號定址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**錯誤** -清單和清單元素只能依元素名稱定址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 揭露更多資料和隱藏資料比揭露太少的資料要好得多，這需要未來後端開發才能揭露資料。

   * 實作可編輯的內容切換有助於保持此HTML的精簡性，讓作者能夠選擇要寫入HTML的內容元素。 當將影像寫入HTML時，並非所有樣式都會使用，這一點尤其重要。
   * 此規則的例外情況是，當昂貴的資源（例如影像）依預設公開時，因為CSS隱藏的事件影像會不必要地擷取。

      * 現代影像元件通常會使用JavaScript來選取並載入最適合使用案例（檢視區）的影像。

### CSS最佳範例{#css-best-practices}

>[!NOTE]
>
>樣式系統與[BEM](https://en.bem.info/)有小的技術差異，因為`BLOCK`和`BLOCK--MODIFIER`不應用於[BEM](https://en.bem.info/)所指定的同一元素。
>
>而是由於產品限制，`BLOCK--MODIFIER`會套用至`BLOCK`元素的父項。
>
>[BEM](https://en.bem.info/)的所有其他租戶都應與一致。

* 使用預處理器，例如[LESS](https://lesscss.org/)（AEM本機支援）或[SCSS](https://sass-lang.com/)（需要自訂建置系統），以允許清楚的CSS定義和重複使用性。

* 保持選擇器的重量／特異性一致；這有助於避免和解決難以識別的CSS階層衝突。
* 將每種樣式組織成獨立的檔案。
   * 這些檔案可使用LESS/SCSS `@imports`組合，或如果需要原始CSS，則可透過HTML用戶端程式庫檔案內含項目或自訂前端資產建立系統。
* 避免混合許多複雜的樣式。
   * 一次套用至元件的樣式越多，排列的變化就越多。 這可能會變得難以維護／品質保證／確保品牌一致性。
* 請始終使用CSS類（遵循BEM注釋）來定義CSS規則。
   * 如果絕對需要選取不含CSS類別的元素（即裸元素），請在CSS定義中將它們移到較高位置，以清楚指出它們的特異性低於與具有可選CSS類別的元素的任何衝突。
* 請避免直接將`BLOCK--MODIFIER`樣式設為回應式格線。 變更此元素的顯示可能會影響回應式格線的轉換和功能，因此只有當想要變更回應式格線的行為時，才會在此層級設定樣式。
* 使用`BLOCK--MODIFIER`應用樣式範圍。 `BLOCK__ELEMENT--MODIFIERS`可用於元件中，但由於`BLOCK`代表元件，而元件是樣式，因此樣式是「已定義」，並通過`BLOCK--MODIFIER`確定範圍。

範例CSS選擇器結構應如下：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第一級選擇器</p> <p>塊——修飾符</p> </td> 
   <td valign="bottom"><p>第2級選擇器</p> <p>塊</p> </td> 
   <td valign="bottom"><p>3級選擇器</p> <p>塊_元素</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS選擇器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list-dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list-dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list_item {  </span></strong></p> <p><strong> 顏色：藍色；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> 顏色：紅色；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

對於巢狀元件，這些巢狀元件元素的CSS選取器深度將超過第3級選取器。 對嵌套元件重複相同的模式，但由父元件的`BLOCK`確定範圍。 換言之，在第3層級啟動巢狀元件的`BLOCK`，巢狀元件的`ELEMENT`將在第4選擇器層級。

### JavaScript最佳範例{#javascript-best-practices}

本節中定義的最佳實務與「style-JavaScript」或JavaScript有關，這些JavaScript專門用於為了文體而非功能目的而操縱元件。

* Style-JavaScript應審慎使用，是少數使用案例。
* Style-JavaScript應主要用於控制元件的DOM，以支援CSS的樣式。
* 如果元件在頁面上出現多次，請重新評估Javascript的使用，並瞭解計算／重新繪製成本。
* 如果Javascript在元件可能在頁面上多次顯示時，以非同步方式（透過AJAX）提取新資料／內容，請重新評估它的使用。
* 同時處理「發佈」和「編寫」體驗。
* 盡可能重新使用style-Javascript。
   * 例如，如果元件的多種樣式需要將其影像移至背景影像，則style-JavaScript可實作一次，並附加至多個`BLOCK--MODIFIERs`。
* 盡可能將style-JavaScript與功能JavaScript分開。
* 評估JavaScript與透過HTL直接在HTML中呈現這些DOM變更的成本。
   * 當使用style-JavaScript的元件需要伺服器端修改時，請評估JavaScript操作目前是否可進行，以及影響／影響元件的效能與支援性。

#### 效能考量事項{#performance-considerations}

* Style-JavaScript應保持輕薄。
* 為避免閃爍和不必要的重新繪製，請先透過`BLOCK--MODIFIER BLOCK`隱藏元件，並在JavaScript中的所有DOM操作完成時顯示元件。
* 樣式- JavaScript操作的效能類似於附加至DOMReady上並修改元素的基本jQuery外掛程式。
* 確保請求已壓縮，而且CSS和JavaScript已精簡。

## 其他資源 {#additional-resources}

* [樣式系統檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立AEM Client程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（塊元素修飾詞）文檔網站](https://getbem.com/)
* [LESS檔案網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
