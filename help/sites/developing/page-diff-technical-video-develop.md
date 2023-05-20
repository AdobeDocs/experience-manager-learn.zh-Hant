---
title: AEM Sites版頁差的開發
description: 此視頻顯示如何為AEM站點的「頁面差異」功能提供自定義樣式。
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# 為頁差開發 {#developing-for-page-difference}

此視頻顯示如何為AEM站點的「頁面差異」功能提供自定義樣式。

## 自定義頁面差異樣式 {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>此視頻將自定義CSS添加到we.Retail客戶端庫中，在該庫中應對定制者的AEM Sites項目進行這些更改；在下面的示例代碼中： `my-project`。

頁AEM差通過直接載入的 `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`。

由於CSS的直接載入而不是使用客戶端庫類別，因此我們必須為自定義樣式找到另一個注入點，而此自定義注入點是項目的創作客戶端庫。

這的好處是允許這些自定義樣式替代成為特定租戶。

### 準備創作客戶端庫 {#prepare-the-authoring-clientlib}

確保 `authoring` 客戶端庫 `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自定義CSS {#provide-the-custom-css}

添加到項目 `authoring` 客戶端庫 `css.txt` 指向將提供覆蓋樣式的較少檔案。 [少](https://lesscss.org/) 由於它具有許多方便的功能，包括本例中利用的類包裝。

```shell
base=./css

htmldiff.less
```

建立 `less` 包含樣式替代的檔案 `/apps/my-project/clientlibs/authoring/css/htmldiff.less`，並根據需要提供覆蓋樣式。

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

### 通過頁面元件包括創作客戶端庫CSS {#include-the-authoring-clientlib-css-via-the-page-component}

在項目的基頁中包括創作客戶端類別 `/apps/my-project/components/structure/page/customheaderlibs.html` 在 `</head>` 標籤以確保載入樣式。

這些樣式應限於 [!UICONTROL 編輯] 和 [!UICONTROL 預覽] WCM模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

應用了上述樣式的比較頁的結束結果將如下所示(添加了HTML並更改了元件)。

![頁差](assets/page-diff.png)

## 其他資源 {#additional-resources}

* [下載we.Retail示例網站](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用客AEM戶端庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [更少CSS文檔](https://lesscss.org/)
