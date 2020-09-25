---
title: 瞭解使用AEM Sites建立樣式系統的最佳實務
description: 詳細說明使用Adobe Experience Manager Sites實作樣式系統的最佳實務。
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1538'
ht-degree: 2%

---


# 瞭解樣式系統最佳實踐{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>請參閱「瞭解 [如何編寫樣式系統的程式碼](style-system-technical-video-understand.md)」中的內容，以確保瞭解AEM Style System使用的BEM類似慣例。

AEM Style System主要實作兩種風格：

* **版面樣式**
* **顯示樣式**

**版面樣式** (Layout styles)會影響元件的許多元素，以建立元件的定義良好且可識別的轉譯（設計和版面），通常會與特定可重複使用的品牌概念一致。 例如，Teaser元件可能以傳統的卡片式版面、水準的促銷樣式或影像上的Hero版面覆蓋文字呈現。

**顯示樣式** (Display Styles)用於影響「版面」樣式的細微變化，但是，它們不會變更「版面」樣式的基本性質或意圖。 例如，Hero版面樣式可能具有顯示樣式，可將顏色配置從主要品牌顏色配置變更為次要品牌顏色配置。

## 組織最佳範例樣式 {#style-organization-best-practices}

定義AEM作者可用的樣式名稱時，最好：

* 使用作者所瞭解的辭彙來命名樣式
* 將樣式選項的數量減到最少
* 僅公開品牌標準允許的樣式選項和組合
* 僅公開具有特效的樣式組合
   * 如果暴露了無效的組合，請確保它們至少不會產生不良效果

隨著AEM作者可使用的樣式組合數目增加，必須依據品牌標準進行QA和驗證的組合數目也會增加。 太多的選項也會混淆作者，因為可能不清楚需要哪種選項或組合才能產生所需的效果。

### 樣式名稱與CSS類別 {#style-names-vs-css-classes}

樣式名稱或呈現給AEM作者的選項，以及實作CSS類別名稱會在AEM中解耦。

這可讓AEM作者清楚瞭解的辭彙表來標示「樣式」選項，但可讓CSS開發人員以未來適用的語義方式命名CSS類別。 例如：

元件必須以品牌的主要和次要顏 **色加****色，但AEM作者知道顏色是** 綠色 **和******&#x200B;黃色，而不是主要和次要的設計語言。

AEM Style System可使用適合作者的標籤 **Green** 和 **Yellow**`.cmp-component--primary-color``.cmp-component--secondary-color` ，來公開這些色彩顯示樣式，同時允許CSS開發人員使用CSS的語義命名，並定義CSS中的實際樣式實作。

「綠色」的「樣式 **名稱** 」會映射 `.cmp-component--primary-color`為，「黃 **色** 」會 `.cmp-component--secondary-color`映射。

如果公司的品牌顏色未來會有所改變，則需要改變的只有和的單一實作 `.cmp-component--primary-color` 和 `.cmp-component--secondary-color`樣式名稱。

## Teaser元件為範例使用案例 {#the-teaser-component-as-an-example-use-case}

以下是設定Teaser元件樣式以具有數個不同版面和顯示樣式的範例使用案例。

這將探索Style名稱（對作者公開）的方式，以及後備CSS類的組織方式。

### 摘要元件樣式設定 {#component-styles-configuration}

下圖顯示摘要 [!UICONTROL 元件] (Teaser)的樣式設定，以取得使用案例中討論的變數。

「樣 [!UICONTROL 式群組] 」名稱、「版面」和「顯示」是偶然符合「顯示樣式」和「版面樣式」的一般概念，這些概念用來對本文中的樣式類型進行概念分類。

「樣 [!UICONTROL 式組] 」名稱和「樣式組」的數量應根據元件使用案例和項目特定的元件樣式約定進行定制。

例如，「顯 **示** 」樣式組名稱可能已命 **名為Colors**。

![顯示樣式組](assets/style-config.png)

### 樣式選擇菜單 {#style-selection-menu}

下圖顯示「樣式 [!UICONTROL 」(Style] )菜單作者與之互動，以選取元件的適當樣式。 請注意， [!UICONTROL 「樣式格線] 」(Style Grpi)名稱以及「樣式」(Style)名稱都對作者公開。

![樣式下拉式選單](assets/style-menu.png)

### Default style {#default-style}

預設樣式通常是元件最常使用的樣式，當新增至頁面時，預設的摘要未樣式檢視。

CSS可直接套用至（不含任何修飾元）或 `.cmp-teaser` 上，視預設樣式的通用性而定 `.cmp-teaser--default`。

如果預設樣式規則比不適用於所有變化更頻繁，則最好將其用作預設樣式的 `.cmp-teaser` CSS類，因為所有變化都應隱式繼承它們，假定遵循BEM類慣例。 否則，應透過預設修飾元套用，例如 `.cmp-teaser--default`[](#component-styles-configuration) ，而此修飾元又需新增至元件的樣式設定的「預設CSS類別」欄位，否則，每個變數中都必須覆寫這些樣式規則。

您甚至可以指定「命名」樣式作為預設樣式，例如下面定義的Hero樣式，但更清楚的是，要針對 `(.cmp-teaser--hero)` 或 `.cmp-teaser``.cmp-teaser--default` CSS類別實作來實作預設樣式。

>[!NOTE]
>
>請注意，「預設版面樣式」沒有「顯示樣式」名稱，不過，作者將可以在「AEM樣式系統」選取工具中選取「顯示」選項。
>
>這違反了最佳做法：
>
>**僅公開具有特效的樣式組合**
>
>如果作者選取「綠色」的「顯示」 **樣式** ，則不會發生任何事。
>
>在此使用案例中，我們將承認此違規，因為所有其他版面樣式都必須使用品牌顏色可上色。
>
>在下方 **的促銷（對齊右側）** ，我們將瞭解如何防止不想要的樣式組合。

![預設樣式](assets/default.png)

* **版面樣式**
   * 預設
* **顯示樣式**
   * 無
* **有效的CSS類別**: `.cmp-teaser--promo` 或 `.cmp-teaser--default`

### 促銷風格 {#promo-style}

促銷 **版面樣式用於促銷網站上的高價值內容** ，水準排版可佔用網頁上的一段空間，且必須以品牌顏色為樣式，預設的促銷版面樣式是使用黑色文字。

為了達到此目 **的** , **Promo的排版樣式** ，以及Teaser元件的AAEM Style系統中 **的Green Green Style********** 和Yellow Yellow Style System的顯示樣式。

#### 促銷預設值

![promo default](assets/promo-default.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 無
* **有效的CSS類別**: `.cmp-teaser--promo`

#### 促銷主要

![promo primary](assets/promo-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類別**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### 促銷次要

![促銷次要](assets/promo-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類別**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### 促銷版右對齊樣式 {#promo-r-align}

促銷 **** 右對齊版面樣式是促銷樣式的變化，該樣式會反向影像和文字的位置（影像在右側，文字在左側）。

其核心是顯示樣式的正確對齊方式，可以將它輸入AEM Style System中，做為與促銷版面樣式搭配選取的顯示樣式。 這違反了以下最佳做法：

**僅公開具有特效的樣式組合**

..已在預設樣式中違 [反該規則](#default-style)。

由於正確的對齊方式只會影響促銷版面樣式，而不會影響其他2種版面樣式：預設和英雄，我們可以建立新的版面樣式Promo（對齊右側），其中包含CSS類別，以對齊Promo版面樣式內容： `cmp -teaser--alternate`.

將多種樣式結合為單一樣式項目，也有助於減少可用樣式和樣式排列的數目，最好將其降到最低。

請注意，CSS類別的名 `cmp-teaser--alternate`稱不必與「對齊右」的作者友好命名法相符。

#### 促銷活動右對齊預設值

![促銷活動右對齊](assets/promo-alternate-default.png)

* **版面樣式**
   * 樣式名稱： **促銷活動（右對齊）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 無
* **有效的CSS類別**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### 促銷右對齊主

![促銷右對齊主](assets/promo-alternate-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷活動（右對齊）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類別**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 促銷右對齊次級

![促銷右對齊次級](assets/promo-alternate-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷活動（右對齊）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類別**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 英雄風格 {#hero-style}

「英雄」版面樣式會以背景顯示元件的影像，並顯示標題和連結。 Hero版面樣式（如促銷版面樣式）必須以品牌顏色著色。

若要使用品牌顏色來為Hero版面樣式加色，您可運用與促銷版面樣式相同的顯示樣式。

根據元件，樣式名稱會對應至一組CSS類別，這表示為促銷版面樣式背景配色的CSS類別名稱，必須為Hero版面樣式的文字和連結加上顏色。

這可透過略過CSS規則來達成，但是，這需要CSS開發人員瞭解如何在AEM上制定這些組合。

CSS，用主（綠）色 **標示** Promote版面樣式的背景：

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS，用主（綠色）顏 **色為Hero** 版面樣式的文字上色：

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Default

![英雄風格](assets/hero.png)

* **版面樣式**
   * 樣式名稱： **英雄**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 無
* **有效的CSS類別**: `.cmp-teaser--hero`

#### Hero Primary

![Hero Primary](assets/hero-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類別**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secondary

![Hero Secondary](assets/hero-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類別**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## 其他資源 {#additional-resources}

* [樣式系統檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立AEM Client程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（塊元素修飾詞）文檔網站](https://getbem.com/)
* [LESS檔案網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
