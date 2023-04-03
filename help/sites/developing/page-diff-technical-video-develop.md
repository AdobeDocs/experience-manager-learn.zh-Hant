---
title: 在AEM Sites中針對頁面差異開發
description: 此影片說明如何為AEM Sites的「頁面差異」功能提供自訂樣式。
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

# 針對頁面差異開發 {#developing-for-page-difference}

此影片說明如何為AEM Sites的「頁面差異」功能提供自訂樣式。

## 自訂頁面差異樣式 {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>此影片會將自訂CSS新增至we.Retail用戶端資料庫，因此應對自訂者的AEM Sites專案進行這些變更；在下列范常式式碼中： `my-project`.

AEM頁面差異會透過直接載入 `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

由於CSS的直接載入，而不是使用用戶端程式庫類別，因此我們必須為自訂樣式找到另一個插入點，而此自訂插入點是專案的製作clientlib。

這樣的好處是，允許這些自訂樣式覆寫是租用戶專用的。

### 準備編寫clientlib {#prepare-the-authoring-clientlib}

確保 `authoring` 您專案的clientlib(位於 `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自訂CSS {#provide-the-custom-css}

新增至專案的 `authoring` clientlib a `css.txt` 這指向提供覆蓋樣式的較少檔案。 [較少](https://lesscss.org/) 是偏好使用，因為它有許多方便的功能，包括此範例中採用的類別包裝。

```shell
base=./css

htmldiff.less
```

建立 `less` 包含樣式覆蓋的檔案 `/apps/my-project/clientlibs/authoring/css/htmldiff.less`，並視需要提供覆蓋樣式。

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

### 透過頁面元件包含Authoring clientlib CSS {#include-the-authoring-clientlib-css-via-the-page-component}

在專案基礎頁面的 `/apps/my-project/components/structure/page/customheaderlibs.html` 就在 `</head>` 標籤來確保載入樣式。

這些樣式應限於 [!UICONTROL 編輯] 和 [!UICONTROL 預覽] WCM模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

套用上述樣式之比較頁面的結尾結果會如下所示(新增HTML且元件已變更)。

![頁面差異](assets/page-diff.png)

## 其他資源 {#additional-resources}

* [下載we.Retail範例網站](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用AEM用戶端程式庫](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [較少的CSS檔案](https://lesscss.org/)
