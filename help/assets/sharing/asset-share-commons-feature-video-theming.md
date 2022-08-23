---
title: 資產共用空間中的主題
description: 功能和技術理解資料共用共用共用
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 1%

---

# 資產共用空間中的主題 {#asset-share-commons-theme}

資產共用下域中的主題簡介 視頻通過自定義顏色方案來瀏覽建立新主題的過程。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

在此視頻中，將基於資產共用共用深暗主題建立新主題。 該配色方案將與自定義徽標相匹配，使網站具有一致的外觀。

## 主題變數

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

下載 [自定義客戶端庫主題](assets/asc-theme-demo.zip)

## 其他資源{#additional-resources}

* [資產共用共用下載](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACSAEM Commons 3.11.0+版本下載](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
