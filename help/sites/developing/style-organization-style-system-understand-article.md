---
title: 透過AEM Sites了解樣式系統最佳作法
description: 說明使用Adobe Experience Manager Sites實作樣式系統的最佳實務的詳細文章。
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 2%

---

# 了解樣式系統最佳作法{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>請查看以下內容： [了解如何編寫樣式系統的代碼](style-system-technical-video-understand.md)，以確保了解AEM樣式系統使用的BEM式慣例。

為AEM樣式系統實作的主要風格有兩種：

* **版面樣式**
* **顯示樣式**

**版面樣式** 會影響元件的許多元素，以建立元件的定義和可識別的轉譯（設計和版面），通常會與特定可重複使用的品牌概念一致。 例如，預告元件可以以傳統的基於卡的佈局、水準的促銷樣式或以主圖佈局的形式顯示在影像上覆蓋文本。

**顯示樣式** 」(Layout styles)中的「佈局」(Layout)樣式的小變化，但它們不會更改「佈局」樣式的基本性質或意圖。 例如，主圖版面樣式可能具有顯示樣式，可將顏色配置從主品牌顏色配置更改為次品牌顏色配置。

## 樣式組織最佳實務 {#style-organization-best-practices}

定義AEM作者可用的樣式名稱時，最好：

* 使用作者所理解的辭匯來命名樣式
* 將樣式選項的數量減到最少
* 僅公開品牌標準允許的樣式選項和組合
* 僅顯示有效的樣式組合
   * 如果暴露了無效組合，請確保它們至少不會產生不良影響

隨著AEM作者可用的可能樣式組合數量增加，必須進行QA並根據品牌標準驗證的排列越多。 太多的選項也可能會混淆作者，因為可能不清楚需要哪個選項或組合才能產生想要的效果。

### 樣式名稱與CSS類 {#style-names-vs-css-classes}

樣式名稱或向AEM作者呈現的選項，以及實作CSS類別名稱在AEM中是分離的。

這可讓AEM作者清楚地以辭匯來標示樣式選項，並加以了解，但讓CSS開發人員以未來適用的語義方式為CSS類別命名。 例如：

元件必須有選項可搭配品牌的 **主要** 和 **次要** 不過，顏色是AEM作者 **綠色** 和 **黃色**，而非主要和次要的設計語言。

AEM樣式系統可使用方便作者的標籤來公開這些色彩顯示樣式 **綠色** 和 **黃色**，同時允許CSS開發人員使用語義命名 `.cmp-component--primary-color` 和 `.cmp-component--secondary-color` 來定義CSS中的實際樣式實作。

樣式名稱 **綠色** 已對應至 `.cmp-component--primary-color`，和 **黃色** to `.cmp-component--secondary-color`.

如果公司的品牌顏色未來會有所改變，則只需改變的單一實作即可 `.cmp-component--primary-color` 和 `.cmp-component--secondary-color`和樣式名稱。

## Teaser元件作為範例使用案例 {#the-teaser-component-as-an-example-use-case}

以下是設計Teaser元件樣式的範例使用案例，以具有數種不同的版面和顯示樣式。

這將探索樣式名稱（向作者公開）的組織方式，以及後備CSS類的組織方式。

### Teaser元件樣式設定 {#component-styles-configuration}

下圖顯示 [!UICONTROL 樣式] 使用案例中討論之變異的Teaser元件設定。

此 [!UICONTROL 樣式組] 「名稱」、「版面」和「顯示」依偶發事件符合「顯示樣式」和「版面樣式」的一般概念，用於對本文中的樣式類型進行概念分類。

此 [!UICONTROL 樣式組] 名稱和 [!UICONTROL 樣式群組] 應根據元件使用案例和專案專用元件樣式慣例量身打造。

例如， **顯示** 樣式組名稱可能已命名 **顏色**.

![顯示樣式組](assets/style-config.png)

### 樣式選取功能表 {#style-selection-menu}

下圖顯示 [!UICONTROL 樣式] 功能表作者會與互動，以選取元件的適當樣式。 請注意 [!UICONTROL 樣式格線] 名稱以及樣式名稱都會向作者公開。

![樣式下拉式選單](assets/style-menu.png)

### 預設樣式 {#default-style}

預設樣式通常是元件最常使用的樣式，而預設的預設預告檢視新增至頁面時沒有樣式。

CSS可以直接套用至 `.cmp-teaser` （不含任何修飾元）或 `.cmp-teaser--default`.

如果預設樣式規則的套用頻率比不適用於所有變數，最好使用 `.cmp-teaser` 作為預設樣式的CSS類，因為假設遵循BEM樣式的慣例，所有變數都應以隱含方式繼承它們。 否則，應透過預設修飾詞來套用，例如 `.cmp-teaser--default`，而這又需要新增至 [元件的樣式配置的預設CSS類](#component-styles-configuration) 欄位，否則這些樣式規則將必須在每個變數中被覆蓋。

甚至可以將「已命名」樣式指派為預設樣式，例如Hero樣式 `(.cmp-teaser--hero)` 定義，但更清楚的是針對 `.cmp-teaser` 或 `.cmp-teaser--default` CSS類別實作。

>[!NOTE]
>
>請注意，「預設版面樣式」沒有「顯示樣式」名稱，但作者可以在「AEM樣式系統」選取工具中選取「顯示」選項。
>
>這違反了最佳做法：
>
>**僅顯示有效的樣式組合**
>
>如果作者選取的 **綠色** 不會發生任何事。
>
>在此使用案例中，我們承認這項違反規定，因為所有其他版面樣式都必須使用品牌顏色可著色。
>
>在 **促銷（對齊右）** 下節將說明如何防止不想要的樣式組合。

![預設樣式](assets/default.png)

* **版面樣式**
   * 預設
* **顯示樣式**
   * 無
* **有效的CSS類**: `.cmp-teaser--promo` 或 `.cmp-teaser--default`

### 促銷樣式 {#promo-style}

此 **促銷版面樣式** 用於促銷網站上的高價值內容，並水準排列以佔用網頁上的一段空間，且必須以品牌顏色建立樣式，且預設促銷版面樣式使用黑色文字。

實現此目標， **版面樣式** of **促銷** 和 **顯示樣式** of **綠色** 和 **黃色** 在AEM Style System中為Teaser元件設定。

#### 促銷預設值

![促銷活動預設](assets/promo-default.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 無
* **有效的CSS類**: `.cmp-teaser--promo`

#### 促銷主要

![促銷主要](assets/promo-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### 促銷次要

![促銷次要](assets/promo-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### 促銷右對齊樣式 {#promo-r-align}

此 **促銷活動右對齊** 版面樣式是促銷樣式的變化，樣式會反轉影像和文字（影像在右，文字在左）的位置。

正確的對齊方式的核心是顯示樣式，可以在AEM樣式系統中作為顯示樣式輸入，該顯示樣式與促銷版面樣式一起選擇。 這違反了以下最佳做法：

**僅顯示有效的樣式組合**

...已經在 [預設樣式](#default-style).

因為正確的對齊方式只會影響促銷版面樣式，而不會影響其他2種版面樣式：預設和主圖，我們可以建立新的佈局樣式Promo（右對齊），其中包含CSS類，該類將Promo佈局樣式內容右對齊： `cmp -teaser--alternate`.

將多種樣式組合成單一樣式項目，也有助於減少可用樣式和樣式排列的數量，而這最好能將數量降至最低。

請注意CSS類的名稱， `cmp-teaser--alternate`，不必與「對作者有利」的命名法相符。

#### 促銷活動右對齊預設值

![促銷活動右對齊](assets/promo-alternate-default.png)

* **版面樣式**
   * 樣式名稱： **促銷（對齊右）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 無
* **有效的CSS類**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### 促銷右對齊主

![促銷右對齊主](assets/promo-alternate-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷（對齊右）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 促銷右對齊次級

![促銷右對齊次級](assets/promo-alternate-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷（對齊右）**
   * CSS 類別: `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 英雄風格 {#hero-style}

主圖版面樣式會以背景顯示元件的影像，標題和連結已覆蓋。 主圖版面樣式（如促銷版面樣式）必須可以用品牌顏色著色。

若要使用品牌顏色為「主圖」版面樣式著色，可以使用與促銷版面樣式相同的顯示樣式。

對於元件，樣式名稱將映射到單個CSS類集，這表示顏色為促銷版式樣式背景的CSS類名稱必須為Hero版式樣式的文本和連結的顏色。

這可透過略過CSS規則來達成，但這需要CSS開發人員了解在AEM上如何實施這些排列。

用於著色背景的CSS **提升** 主（綠色）顏色的佈局樣式：

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

用於著色文本的CSS **英雄** 主（綠色）顏色的佈局樣式：

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### 主圖預設值

![英雄風格](assets/hero.png)

* **版面樣式**
   * 樣式名稱： **英雄**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 無
* **有效的CSS類**: `.cmp-teaser--hero`

#### 主圖

![主圖](assets/hero-primary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS 類別: `cmp-teaser--primary-color`
* **有效的CSS類**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero次級

![Hero次級](assets/hero-secondary.png)

* **版面樣式**
   * 樣式名稱： **促銷**
   * CSS 類別: `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS 類別: `cmp-teaser--secondary-color`
* **有效的CSS類**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## 其他資源 {#additional-resources}

* [樣式系統文檔](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立AEM Client程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（區塊元素修飾詞）檔案網站](https://getbem.com/)
* [LESS文檔網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
