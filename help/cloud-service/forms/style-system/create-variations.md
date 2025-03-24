---
title: 在AEM Forms中使用樣式系統
description: 定義變數的CSS類別
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 68030c25-1674-4a64-9f5f-c1a74a38e3ca
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# 建立按鈕元件的變數

複製主題後，請使用Visual Studio程式碼開啟專案。 您應該會看到類似的檢視
在visual studio code中
![專案總管](assets/easel-theme.png)

開啟src->components->button->_button.scss檔案。 我們將在此檔案中定義自訂變數。

## 公司變數

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## 解釋

* **cmp-adaptiveform-button—corporate**：這是「cmp-adaptiveform-button—corporate」元件的主要包裝函式或容器類別。
此區塊內的任何樣式或mixin將套用至此類別內的元素。
* **@include容器**：這會使用_mixins.scss中定義的mixin （稱為container）。 mixin容器通常會套用版面相關樣式（例如設定邊界、填補或其他結構樣式），以確保容器的運作方式一致。
* **.cmp-adaptiveform-button**：在corporate-style-button區塊中，您正在以.cmp-adaptiveform-button類別鎖定子專案。
* **&amp;__Widget**： &amp;符號代表父選取器，在此例中為.cmp-adaptiveform-button。
這表示目標的最終類別將是.cmp-adaptiveform-button__widget，這是BEM樣式的類別（區塊元素修飾元），代表.cmp-adaptiveform-button區塊內的子元件(__widget元素)。
* **@include主要按鈕**：這包含主要按鈕mixin （定義於_mixin.scss中），並新增與按鈕相關的樣式（例如邊框間距、顏色、游標效果等）。 會覆寫mixin主要按鈕中定義的屬性background、text-transform、border-radius、color。

_mixins.scss檔案定義於src->site下，如下面的熒幕擷圖所示

![mixin.scss](assets/mixins.png)

## 行銷變數

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## 後續步驟

[測試變數](./build.md)
