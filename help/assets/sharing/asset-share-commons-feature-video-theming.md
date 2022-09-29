---
title: 資產共用公域中的主題
description: 功能與技術了解的資料資產共用公域
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 1%

---

# 資產共用公域中的主題 {#asset-share-commons-theme}

資產共用公域中主題的簡介。 影片會逐步說明如何使用自訂色彩配置建立新主題。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

此影片會根據「資產共用公域深色」主題建立新主題。 色彩配置將符合自訂標誌，讓網站呈現一致的外觀與風格。

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

下載 [自訂用戶端程式庫主題](assets/asc-theme-demo.zip)

## 其他資源{#additional-resources}

* [資產共用公域發行下載](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+版本下載](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
