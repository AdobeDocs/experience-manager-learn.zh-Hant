---
title: 在AEM Sites中針對頁面差異開發
description: 此影片說明如何為AEM Sites的「頁面差異」功能提供自訂樣式。
feature: 製作
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 1%

---


# 針對頁面差異{#developing-for-page-difference}開發

此影片說明如何為AEM Sites的「頁面差異」功能提供自訂樣式。

## 自訂頁面差異樣式{#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>此影片會將自訂CSS新增至we.Retail用戶端資料庫，因此應對自訂者的AEM Sites專案進行這些變更；在下列范常式式碼中：`my-project`。

AEM頁面差異會透過直接載入`/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`來取得OOTB CSS。

由於CSS的直接載入，而不是使用用戶端程式庫類別，因此我們必須為自訂樣式找到另一個插入點，而此自訂插入點是專案的製作clientlib。

這樣的好處是，允許這些自訂樣式覆寫是租用戶專用的。

### 準備編寫clientlib {#prepare-the-authoring-clientlib}

確保`/apps/my-project/clientlib/authoring.`處的項目存在`authoring` clientlib

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自訂CSS {#provide-the-custom-css}

新增至專案的`authoring` clientlib a `css.txt` ，此檔案指向將提供覆寫樣式的較少檔案。 [](https://lesscss.org/) Less是偏好使用的選項，因為它有許多便利的功能，包括此範例中採用的類別包裝。

```shell
base=./css

htmldiff.less
```

在`/apps/my-project/clientlibs/authoring/css/htmldiff.less`處建立包含樣式覆蓋的`less`檔案，並根據需要提供覆蓋樣式。

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### 透過頁面元件{#include-the-authoring-clientlib-css-via-the-page-component}包含Authoring clientlib CSS

將製作clientlibs類別直接加入專案基礎頁面`/apps/my-project/components/structure/page/customheaderlibs.html`的`</head>`標籤前，以確保載入樣式。

這些樣式應限制為[!UICONTROL Edit]和[!UICONTROL preview] WCM模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

套用上述樣式之比較頁面的結尾結果看起來會像這樣（已新增HTML且元件已變更）。

![頁面差異](assets/page-diff.png)

## 其他資源 {#additional-resources}

* [下載we.Retail範例網站](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用AEM用戶端程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [較少的CSS檔案](https://lesscss.org/)
