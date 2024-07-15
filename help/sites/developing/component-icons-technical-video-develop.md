---
title: 在Adobe Experience Manager Sites中自訂元件圖示
description: 元件圖示可讓作者透過圖示或有意義的縮寫快速識別元件。 作者現在可以找到所需的元件，以前所未有的速度建置其網頁體驗。
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 自訂元件圖示 {#developing-component-icons-in-aem-sites}

元件圖示可讓作者透過圖示或有意義的縮寫快速識別元件。 作者現在可以找到所需的元件，以前所未有的速度建置其網頁體驗。

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

元件瀏覽器現在會以一致的灰色主題顯示，並顯示：

* **[!UICONTROL 元件群組]**
* **[!UICONTROL 元件標題]**
* **[!UICONTROL 元件說明]**
* **[!UICONTROL 元件圖示]**
   * 元件標題&#x200B;*（預設）*&#x200B;的前兩個字母
   * 自訂PNG影像&#x200B;*（由開發人員設定）*
   * 自訂SVG影像&#x200B;*（由開發人員設定）*
   * CoralUI圖示&#x200B;*（由開發人員設定）*

## 元件圖示組態選項 {#component-icon-configuration-options}

### 縮寫 {#abbreviations}

依預設，元件標題(**[cq：Component]@jcr：title**)的前2個字元會作為縮寫。 例如，如果&#x200B;**[cq：Component]@jcr：title=Article List**，縮寫會顯示為&quot;**Ar**&quot;。

縮寫可以透過&#x200B;**[cq：Component]@abbreviation**&#x200B;屬性自訂。 雖然此值可接受超過2個字元，但建議將縮寫限製為2個字元，以避免任何視覺干擾。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI圖示 {#coralui-icons}

AEM提供的CoralUI圖示可用於元件圖示。 若要設定CoralUI圖示，請將&#x200B;**[cq：Component]@cq：icon**&#x200B;屬性設定為所需的CoralUI圖示的HTML圖示屬性值（列於[CoralUI檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG影像 {#png-images}

PNG影像可用於元件圖示。 若要將PNG影像設定為元件圖示，請在&#x200B;**[cq：Component]**&#x200B;下將所需的影像新增為名為&#x200B;**cq：icon.png**&#x200B;的&#x200B;**nt：file**。

PNG應該要有透明背景，或是背景顏色設定為&#x200B;**#707070**。

PNG影像已縮放為&#x200B;**20px x x 20px**。 不過，如果要讓&#x200B;**40px**&#x200B;與&#x200B;**40px**&#x200B;搭配使用，可能比較合適。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG影像 {#svg-images}

SVG影像（向量型）可用於元件圖示。 若要將SVG影像設定為元件圖示，請在&#x200B;**[cq：Component]**&#x200B;下將所需的SVG新增為名為&#x200B;**cq：icon.svg**&#x200B;的&#x200B;**nt：file**。

SVG影像的背景顏色應設為&#x200B;**#707070**，大小為&#x200B;**20px x 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他資源 {#additional-resources}

* [可用的CoralUI圖示](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
