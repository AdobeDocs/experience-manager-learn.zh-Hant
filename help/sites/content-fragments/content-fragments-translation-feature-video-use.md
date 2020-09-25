---
title: 搭配AEM內容片段使用翻譯
description: AEM 6.3提供翻譯內容片段的功能。 與內容片段關聯的混合媒體資產和資產集合也符合提取和翻譯的資格。
sub-product: 網站、資產、內容服務
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# 搭配AEM內容片段使用翻譯{#using-translation-with-aem-content-fragments}

AEM 6.3提供翻譯內容片段的功能。 與內容片段關聯的混合媒體資產和資產集合也符合提取和翻譯的資格。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## 內容片段轉譯使用案例 {#content-fragment-translation-use-cases}

「內容片段」是AEM將擷取並傳送至外部轉譯服務的可識別內容類型。 現成可支援數個使用案例：

1. 您可以直接在「資產」主控台中選取內容片段，以進行語言複製和翻譯
2. 在「網站」頁面上參考的內容片段將複製至適當的語言資料夾，並在選取「網站」頁面做為語言副本時，擷取以供翻譯
3. 內嵌在內容片段中的內嵌媒體資產可供擷取和翻譯。
4. 與內容片段相關聯的資產集合可以擷取和翻譯

## 翻譯配置選項 {#translation-config-options}

立即可用的轉換設定支援數個轉換內容片段的選項。 依預設，內嵌媒體資產和相關的資產集合不會進行翻譯。 要更新翻譯配置，請導航到 [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html)。

轉換「內容片段」資產有四個選項：

1. **不翻譯（預設）**
2. **僅限內嵌媒體資產**
3. **僅限相關的資產集合**
4. **內嵌媒體資產和相關的集合**

![翻譯設定](assets/classic-ui-dialog.png)
