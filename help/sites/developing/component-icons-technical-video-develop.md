---
title: 在Adobe Experience Manager Sites自訂元件圖示
description: 「元件圖示」可讓作者使用圖示或有意義的縮寫快速識別元件。 作者現在可以找到建立其網頁體驗所需的元件，比以往更快速。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Core Components
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 2%

---


# 自定義元件表徵圖{#developing-component-icons-in-aem-sites}

「元件圖示」可讓作者使用圖示或有意義的縮寫快速識別元件。 作者現在可以找到建立其網頁體驗所需的元件，比以往更快速。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

元件瀏覽器現在會以一致的灰色主題顯示，顯示：

* **[!UICONTROL 元件群組]**
* **[!UICONTROL 元件標題]**
* **[!UICONTROL 元件說明]**
* **[!UICONTROL 元件圖示]**
   * 元件標題的前兩個字母&#x200B;*（預設）*
   * 自訂PNG影像&#x200B;*（由開發人員設定）*
   * 自訂SVG影像&#x200B;*（由開發人員設定）*
   * CoralUI圖示&#x200B;*（由開發人員設定）*

## 元件表徵圖配置選項{#component-icon-configuration-options}

### 縮寫{#abbreviations}

依預設，元件標題的前2個字元(**[cq:Component]@jcr:title**)會當做縮寫。 例如，如果&#x200B;**[cq:Component]@jcr:title=Article List**，縮寫將顯示為&quot;**Ar**&quot;。

縮寫可以通過&#x200B;**[cq:Component]@abreviation**&#x200B;屬性自訂。 雖然此值可接受2個以上的字元，但建議將縮寫限制為2個字元，以避免任何視覺干擾。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI圖示{#coralui-icons}

CoralUI圖示(由提供AEM)可用於元件圖示。 若要設定CoralUI圖示，請將&#x200B;**[cq:Component]@cq:icon**&#x200B;屬性設定為所需CoralUI圖示的HTML圖示屬性值（列於[CoralUI檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)中）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG影像{#png-images}

PNG影像可用於元件圖示。 若要將PNG影像設定為元件圖示，請在&#x200B;**[cq:Component]**&#x200B;下方，將所要的影像新增為&#x200B;**nt:file**，名為&#x200B;**cq:icon.png**。

PNG應具有透明背景，或背景顏色設定為&#x200B;**#707070**。

PNG影像將會縮放為&#x200B;**20px x 20px**。 但是，最好將視網膜顯示器(**40px** by **40px**)容納。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG影像{#svg-images}

SVG影像（以向量為基礎）可用於元件圖示。 要將SVG影像配置為元件表徵圖，請在&#x200B;**[cq:Component]**&#x200B;下將所需的SVG添加為&#x200B;**nt:file**，名為&#x200B;**cq:icon.svg**。

SVG影像的背景顏色應設為&#x200B;**#707070**，且大小應為&#x200B;**20px x 20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他資源 {#additional-resources}

* [可用的CoralUI圖示](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
