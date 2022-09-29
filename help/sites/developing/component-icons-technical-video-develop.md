---
title: 在Adobe Experience Manager Sites中自訂元件圖示
description: 元件圖示可讓作者快速識別包含圖示或有意義縮寫的元件。 作者現在可以比以往更快找到建立其網頁體驗所需的元件。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# 自訂元件圖示 {#developing-component-icons-in-aem-sites}

元件圖示可讓作者快速識別包含圖示或有意義縮寫的元件。 作者現在可以比以往更快找到建立其網頁體驗所需的元件。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

元件瀏覽器現在會以一致的灰色主題顯示，並顯示：

* **[!UICONTROL 元件群組]**
* **[!UICONTROL 元件標題]**
* **[!UICONTROL 元件說明]**
* **[!UICONTROL 元件圖示]**
   * 元件標題的前兩個字母 *（預設）*
   * 自訂PNG影像 *（由開發人員設定）*
   * 自訂SVG影像 *（由開發人員設定）*
   * CoralUI圖示 *（由開發人員設定）*

## 元件圖示組態選項 {#component-icon-configuration-options}

### 縮寫 {#abbreviations}

依預設，元件標題的前2個字元(**[cq：元件]@jcr:title**)作為縮寫。 例如，若 **[cq：元件]@jcr:title=文章清單** 縮寫將顯示為「**Ar**」。

縮寫可透過 **[cq：元件]@abbreviation** 屬性。 雖然此值可以接受2個以上的字元，但建議將縮寫限制為2個字元，以避免任何視覺干擾。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI圖示 {#coralui-icons}

AEM提供的CoralUI圖示可用於元件圖示。 若要設定CoralUI圖示，請設定 **[cq：元件]@cq：表徵圖** 屬性中，填入所需CoralUI圖示的HTML圖示屬性值(列舉於 [CoralUI檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG影像 {#png-images}

PNG影像可用於元件圖示。 若要將PNG影像設定為元件圖示，請將所需影像新增為 **nt:file** 已命名 **cq:icon.png** 在 **[cq：元件]**.

PNG應具有透明背景，或將背景顏色設定為 **#707070**.

PNG影像會縮放至 **20px x 20px**. 但是要容納視網膜顯示器 **40px** by **40px** 可能更好。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG影像 {#svg-images}

SVG影像（基於向量）可用於元件圖示。 若要將SVG影像設定為元件圖示，請將所需的SVG新增為 **nt:file** 已命名 **cq:icon.svg** 在 **[cq：元件]**.

SVG影像的背景顏色應設定為 **#707070** 和 **20px寬20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他資源 {#additional-resources}

* [可用的CoralUI圖示](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
