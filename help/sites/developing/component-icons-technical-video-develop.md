---
title: 在Adobe Experience Manager Sites自定義元件表徵圖
description: 元件表徵圖允許作者使用表徵圖或有意義的縮寫快速識別元件。 作者現在可以找到構建其Web體驗所需的元件，其速度比以往任何時候都快。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# 自定義元件表徵圖 {#developing-component-icons-in-aem-sites}

元件表徵圖允許作者使用表徵圖或有意義的縮寫快速識別元件。 作者現在可以找到構建其Web體驗所需的元件，其速度比以往任何時候都快。

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

元件瀏覽器現在以一致的灰色主題顯示，顯示：

* **[!UICONTROL 元件群組]**
* **[!UICONTROL 元件標題]**
* **[!UICONTROL 元件說明]**
* **[!UICONTROL 元件表徵圖]**
   * 元件標題的前兩個字母 *（預設）*
   * 自定義PNG影像 *（由開發人員配置）*
   * 自定義SVG影像 *（由開發人員配置）*
   * CoralUI表徵圖 *（由開發人員配置）*

## 元件表徵圖配置選項 {#component-icon-configuration-options}

### 縮略語 {#abbreviations}

預設情況下，元件標題的前2個字元(**[cq：元件]@jcr：標題**)用作縮寫。 例如，如果 **[cq：元件]@jcr:title=文章清單** 縮寫將顯示為&quot;**阿爾**。

可通過 **[cq：元件]@abbreviation** 屬性。 雖然此值可以接受2個以上的字元，但建議將縮寫限制為2個字元，以避免任何視覺干擾。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI表徵圖 {#coralui-icons}

由提供的CoralUI圖AEM標可用於元件表徵圖。 要配置CoralUI表徵圖，請設定 **[cq：元件]@cq：表徵圖** 屬性到所需CoralUI表徵圖的HTML表徵圖屬性值(在 [CoralUI文檔](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG影像 {#png-images}

PNG影像可用於元件表徵圖。 要將PNG影像配置為元件表徵圖，請將所需影像添加為 **nt：檔案** 命名 **cq：表徵圖.png** 下 **[cq：元件]**。

PNG應具有透明背景或背景顏色設定為 **#707070**。

PNG影像將縮放到 **20px x 20px**。 但是要適應視網膜顯示 **40px** 按 **40px** 也許更可取。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG影像 {#svg-images}

SVG影像（基於向量）可用於元件表徵圖。 要將SVG影像配置為元件表徵圖，請將所需SVG添加為 **nt：檔案** 命名 **cq：表徵圖.svg** 下 **[cq：元件]**。

SVG影像的背景顏色應設定為 **#707070** 和 **20px,20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他資源 {#additional-resources}

* [可用的CoralUI表徵圖](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
