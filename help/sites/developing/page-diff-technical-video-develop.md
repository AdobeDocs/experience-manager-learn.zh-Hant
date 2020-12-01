---
title: 針對AEM網站中的頁面差異進行開發
description: 此影片說明如何提供AEM Sites「頁面差異」功能的自訂樣式。
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# 開發頁面差異{#developing-for-page-difference}

此影片說明如何提供AEM Sites「頁面差異」功能的自訂樣式。

## 自訂頁面差異樣式{#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>此影片將自訂CSS新增至we.Retail用戶端程式庫，因此應對客戶的AEM Sites專案進行這些變更；在以下范常式式碼中：`my-project`。

AEM的頁面差異透過直接載入`/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`來取得OOTB CSS。

由於直接載入CSS而非使用用戶端資料庫類別，因此我們必須為自訂樣式尋找另一個注入點，而此自訂注入點是專案的編寫clientlib。

其優點是，允許這些自訂樣式覆寫是租用戶專屬。

### 準備編寫clientlib {#prepare-the-authoring-clientlib}

確保項目`/apps/my-project/clientlib/authoring.`中存在`authoring` clientlib

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自訂CSS {#provide-the-custom-css}

新增至專案的`authoring` clientlib a `css.txt`，指向提供覆寫樣式的較少檔案。 [Less](https://lesscss.org/) 是較佳的選擇，因為它有許多方便的功能，包括本例中運用的類別封裝功能。

```shell
base=./css

htmldiff.less
```

在`/apps/my-project/clientlibs/authoring/css/htmldiff.less`建立包含樣式覆寫的`less`檔案，並視需要提供覆寫樣式。

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

### 透過頁面元件{#include-the-authoring-clientlib-css-via-the-page-component}加入編寫clientlib CSS

將編寫clientlibs類別加入專案基本頁面的`/apps/my-project/components/structure/page/customheaderlibs.html`中，直接加入`</head>`標籤，以確保載入樣式。

這些樣式應限制為[!UICONTROL Edit]和[!UICONTROL preview] WCM模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

套用上述樣式的比較頁面的結果會如下所示（新增HTML並變更元件）。

![頁面差異](assets/page-diff.png)

## 其他資源 {#additional-resources}

* [下載we.Retail範例網站](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用AEM用戶端程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [更少的CSS檔案](https://lesscss.org/)
