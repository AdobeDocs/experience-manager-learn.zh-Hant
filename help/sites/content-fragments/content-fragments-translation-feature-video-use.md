---
title: AEM內容片段的翻譯支援
description: 瞭解如何使用Adobe Experience Manager本地化和翻譯內容片段。 與內容片段相關的混合媒體資產也符合擷取和翻譯的資格。
feature: 內容片段，多網站管理員
topics: Localization
role: 業務從業人員
level: 中級
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: 4620acc18a08d71994753903b79247a8ed3fd8f5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---


# AEM內容片段的轉譯支援{#translation-support-content-fragments}

瞭解如何使用Adobe Experience Manager本地化和翻譯內容片段。 與內容片段相關的混合媒體資產也符合擷取和翻譯的資格。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## 內容片段轉譯使用案例{#content-fragment-translation-use-cases}

「內容片段」是AEM擷取的可辨識內容類型，會傳送至外部轉譯服務。 現成可支援數個使用案例：

1. 您可以直接在「資產」主控台中選取內容片段，以進行語言複製和翻譯
2. 在「網站」頁面上參考的內容片段會複製到適當的語言資料夾，並在選取「網站」頁面做為語言副本時，擷取以供翻譯
3. 內嵌在內容片段中的內嵌媒體資產可供擷取和翻譯。
4. 與內容片段相關聯的資產集合可以擷取和翻譯

## 翻譯規則編輯器 {#translation-rules-editor}

使用&#x200B;**翻譯規則編輯器**&#x200B;可更新Experience Manager翻譯行為。 要更新翻譯，請導航至&#x200B;**工具** > **一般** > **翻譯配置** ，位於[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)。

立即可用的配置引用資源類型`core/wcm/components/contentfragment/v1/contentfragment`的`fragmentPath`上的內容片段。 預設配置會識別從`v1/contentfragment`繼承的所有元件。

![翻譯規則編輯器](assets/translation-configuration.png)
