---
title: Asset Share Commons主題化簡介
description: Assets共用區功能與技術瞭解資料
version: 6.4, 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
doc-type: Tutorial
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
duration: 734
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 1%

---

# Asset Share Commons主題化簡介 {#asset-share-commons-theme}

簡要介紹「資產共用公域」的主題。 影片會逐步說明使用自訂色彩配置建立新佈景主題的過程。

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

此影片會根據資產共用共用深色主題建立一個新主題。 色彩配置會比對自訂標誌，以提供一致的外觀和風格。

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

下載[自訂使用者端資料庫主題](assets/asc-theme-demo.zip)

## 其他資源{#additional-resources}

* [Asset Share Commons發行下載](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+版本下載](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
