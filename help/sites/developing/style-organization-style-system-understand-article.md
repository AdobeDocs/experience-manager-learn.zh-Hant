---
title: 透過AEM Sites瞭解樣式系統最佳實務
description: 說明透過Adobe Experience Manager Sites實作樣式系統的最佳實務的詳細文章。
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 328
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# 瞭解樣式系統最佳實務{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>請檢閱位於[的內容瞭解如何為樣式系統](style-system-technical-video-understand.md)編碼，以確保瞭解AEM樣式系統使用的類似BEM的慣例。

針對AEM樣式系統實作有兩種主要風格或樣式：

* **配置樣式**
* **顯示樣式**

**配置樣式**&#x200B;會影響元件的許多元素，以建立定義良好且可識別的元件轉譯（設計和配置），通常會與特定可重複使用的品牌概念一致。 例如，Teaser元件可以呈現為傳統卡片式版面、水準促銷樣式，或是在影像上以主圖版面覆蓋文字。

**顯示樣式**&#x200B;是用來影響版面樣式的細微變化，但是它們不會改變版面樣式的基本特性或意圖。 例如，主圖版面配置樣式可能有顯示樣式，可將色彩配置從主要品牌色彩配置變更為次要品牌色彩配置。

## 風格組織最佳實務 {#style-organization-best-practices}

在定義AEM作者可用的樣式名稱時，最好：

* 使用作者能理解的辭彙來命名樣式
* 最小化樣式選項的數目
* 僅公開品牌標準允許的樣式選項和組合
* 僅公開具有效果的樣式組合
   * 如果暴露無效組合，請確定它們至少不會產生不良影響

隨著AEM作者可用的可能樣式組合數量增加，存在更多必須通過QA及品牌標準驗證的排列。 太多選項也會讓作者感到困惑，因為不知道需要哪個選項或組合才能產生想要的效果。

### 樣式名稱與CSS類別的比較 {#style-names-vs-css-classes}

樣式名稱，或呈現給AEM作者的選項，以及實作CSS類別名稱在AEM中是分離的。

這可讓樣式選項以清楚的辭彙標示，並讓AEM作者瞭解，但可讓CSS開發人員以未來的校訂、語意方式命名CSS類別。 例如：

元件必須具備選項，才能使用品牌的&#x200B;**主要**&#x200B;和&#x200B;**次要**&#x200B;色彩來著色，但是AEM作者知道色彩為&#x200B;**綠色**&#x200B;和&#x200B;**黃色**，而不是主要和次要的設計語言。

AEM樣式系統可使用作者友善的標籤&#x200B;**綠色**&#x200B;和&#x200B;**黃色**&#x200B;來公開這些色彩顯示樣式，同時允許CSS開發人員使用`.cmp-component--primary-color`和`.cmp-component--secondary-color`的語意命名來定義CSS中的實際樣式實作。

**綠色**&#x200B;的樣式名稱對應至`.cmp-component--primary-color`，黃色&#x200B;**對應至`.cmp-component--secondary-color`。**

如果公司的品牌色彩未來有所變更，則只需變更`.cmp-component--primary-color`和`.cmp-component--secondary-color`的單一實作，以及樣式名稱。

## Teaser元件作為範例使用案例 {#the-teaser-component-as-an-example-use-case}

以下範例使用案例來設定Teaser元件的樣式，使其具有數個不同的「配置」和「顯示」樣式。

這將會探索樣式名稱（向作者公開）的方式以及支援CSS類別的組織方式。

### Teaser元件樣式設定 {#component-styles-configuration}

下圖針對使用案例中討論的變數，顯示Teaser元件的[!UICONTROL 樣式]設定。

[!UICONTROL 樣式群組]名稱、[配置]和[顯示]，依偶然性符合本文中用來將樣式型別概念分類的顯示樣式和配置樣式的一般概念。

[!UICONTROL 樣式群組]名稱和[!UICONTROL 樣式群組]的數目應該根據元件使用案例和專案特定的元件樣式慣例量身打造。

例如，**Display**&#x200B;樣式群組名稱可能已命名為&#x200B;**Colors**。

![顯示樣式群組](assets/style-config.png)

### 樣式選取功能表 {#style-selection-menu}

下圖顯示[!UICONTROL 樣式]功能表作者與互動，為元件選取適當的樣式。 請注意，[!UICONTROL 樣式Grpi]名稱以及樣式名稱都向作者公開。

![樣式下拉式功能表](assets/style-menu.png)

### 預設樣式 {#default-style}

預設樣式通常是元件最常用的樣式，以及新增到頁面時預設的Teaser未樣式檢視。

根據預設樣式的共通性，CSS可以直接套用至`.cmp-teaser` （沒有任何修飾元）或`.cmp-teaser--default`。

如果預設樣式規則套用頻率高於不套用至所有變數，最好使用`.cmp-teaser`作為預設樣式的CSS類別，因為所有變數都應該隱含繼承它們，假設遵循BEM式的慣例。 若非如此，應透過預設修飾元（例如`.cmp-teaser--default`）套用這些變數，而預設修飾元又需要新增至[元件的樣式組態的預設CSS類別](#component-styles-configuration)欄位，否則必須在每個變數中覆寫這些樣式規則。

您甚至可以將「已命名」樣式指派為預設樣式，例如下方定義的Hero樣式`(.cmp-teaser--hero)`，但更清楚的是在對`.cmp-teaser`或`.cmp-teaser--default` CSS類別實作中實作預設樣式。

>[!NOTE]
>
>請注意，「預設」配置樣式沒有「顯示」樣式名稱，但作者可以在「AEM樣式系統」選取工具中選取「顯示」選項。
>
>這違反了最佳實務：
>
>**僅公開具有效果的樣式組合**
>
>如果作者選取&#x200B;**綠色**&#x200B;的顯示樣式，則不會發生任何狀況。
>
>在此使用案例中，我們將承認此違規，因為所有其他版面樣式都必須可使用品牌顏色上色。
>
>在下方的&#x200B;**促銷（靠右對齊）**&#x200B;區段中，我們將瞭解如何防止不想要的樣式組合。

![預設樣式](assets/default.png)

* **配置樣式**
   * 預設
* **顯示樣式**
   * 無
* **有效的CSS類別**： `.cmp-teaser--promo`或`.cmp-teaser--default`

### 促銷樣式 {#promo-style}

**促銷版面配置樣式**&#x200B;是用來促銷網站上的高價值內容，並且是水準配置以佔用網頁的空間，而且必須能夠依據品牌顏色來配置樣式，預設的促銷版面配置樣式使用黑色文字。

為此，已在AEM樣式系統中為Teaser元件設定&#x200B;**促銷**&#x200B;的&#x200B;**配置樣式**&#x200B;以及&#x200B;**綠色**&#x200B;和&#x200B;**黃色**&#x200B;的&#x200B;**顯示樣式**。

#### 促銷預設

![促銷預設值](assets/promo-default.png)

* **配置樣式**
   * 樣式名稱： **促銷**
   * CSS類別： `cmp-teaser--promo`
* **顯示樣式**
   * 無
* **有效的CSS類別**： `.cmp-teaser--promo`

#### 主要促銷

![主要促銷](assets/promo-primary.png)

* **配置樣式**
   * 樣式名稱： **促銷**
   * CSS類別： `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS類別： `cmp-teaser--primary-color`
* **有效的CSS類別**： `cmp-teaser--promo.cmp-teaser--primary-color`

#### 次要促銷

![促銷次要](assets/promo-secondary.png)

* **配置樣式**
   * 樣式名稱： **促銷**
   * CSS類別： `cmp-teaser--promo`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS類別： `cmp-teaser--secondary-color`
* **有效的CSS類別**： `cmp-teaser--promo.cmp-teaser--secondary-color`

### 促銷右對齊樣式 {#promo-r-align}

**促銷右對齊**&#x200B;版面配置樣式是「促銷」樣式的變異，這種樣式會顛倒影像與文字（影像在右方，文字在左方）的位置。

右對齊方式的核心是顯示樣式，您可以將它作為顯示樣式輸入到AEM樣式系統中，此顯示樣式與「促銷」配置樣式一起選取。 這違反了以下最佳實務：

**僅公開具有效果的樣式組合**

..已在[預設樣式](#default-style)中違反該規則。

由於正確對齊方式只會影響「促銷」配置樣式，而不會影響其他2個配置樣式：預設和主圖，因此我們可以建立新的配置樣式「促銷」（靠右對齊），其中包含將「促銷」配置樣式內容靠右對齊的CSS類別： `cmp -teaser--alternate`。

將多個樣式組合成單一樣式專案，也可協助減少可用樣式和樣式排列的數目，使其最少。

請注意，CSS類別的名稱`cmp-teaser--alternate`不必符合「right aligned」的作者易記命名法。

#### 促銷活動靠右對齊預設值

![促銷活動靠右對齊](assets/promo-alternate-default.png)

* **配置樣式**
   * 樣式名稱： **促銷（靠右對齊）**
   * CSS類別： `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 無
* **有效的CSS類別**： `.cmp-teaser--promo.cmp-teaser--alternate`

#### 主要促銷活動靠右對齊

![促銷右對齊主要](assets/promo-alternate-primary.png)

* **配置樣式**
   * 樣式名稱： **促銷（靠右對齊）**
   * CSS類別： `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS類別： `cmp-teaser--primary-color`
* **有效的CSS類別**： `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 促銷右對齊次要

![促銷右對齊次要](assets/promo-alternate-secondary.png)

* **配置樣式**
   * 樣式名稱： **促銷（靠右對齊）**
   * CSS類別： `cmp-teaser--promo cmp-teaser--alternate`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS類別： `cmp-teaser--secondary-color`
* **有效的CSS類別**： `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### 英雄樣式 {#hero-style}

主圖版面配置樣式會將元件的影像顯示為背景，並覆蓋標題和連結。 主圖版面配置樣式（如促銷版面配置樣式）必須可搭配品牌顏色上色。

若要以品牌色彩為主圖版面樣式上色，可運用與促銷版面樣式相同的顯示樣式。

每個元件中，樣式名稱會對應至單一CSS類別集，這表示代表Promo配置樣式背景的CSS類別名稱必須代表Hero配置樣式的文字和連結。

只要界定CSS規則的範圍，通常就能達成此目的，不過，這確實需要CSS開發人員瞭解這些排列如何在AEM上實施。

以主要（綠色）色彩將&#x200B;**Promote**&#x200B;配置樣式的背景著色的CSS：

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS以主要（綠色）色彩將&#x200B;**Hero**&#x200B;配置樣式的文字著色：

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero預設

![英雄樣式](assets/hero.png)

* **配置樣式**
   * 樣式名稱： **Hero**
   * CSS類別： `cmp-teaser--hero`
* **顯示樣式**
   * 無
* **有效的CSS類別**： `.cmp-teaser--hero`

#### 主要主圖

![主要英雄](assets/hero-primary.png)

* **配置樣式**
   * 樣式名稱： **促銷**
   * CSS類別： `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **綠色**
   * CSS類別： `cmp-teaser--primary-color`
* **有效的CSS類別**： `cmp-teaser--hero.cmp-teaser--primary-color`

#### 主圖次要

![次要英雄](assets/hero-secondary.png)

* **配置樣式**
   * 樣式名稱： **促銷**
   * CSS類別： `cmp-teaser--hero`
* **顯示樣式**
   * 樣式名稱： **黃色**
   * CSS類別： `cmp-teaser--secondary-color`
* **有效的CSS類別**： `cmp-teaser--hero.cmp-teaser--secondary-color`

## 其他資源 {#additional-resources}

* [樣式系統檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [正在建立AEM使用者端資料庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM （區塊元素修飾元）檔案網站](https://getbem.com/)
* [較少說明檔案網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
