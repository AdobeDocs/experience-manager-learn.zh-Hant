---
title: AEM內容片段的翻譯支援
description: 了解如何使用Adobe Experience Manager來本地化和翻譯內容片段。 與內容片段相關聯的混合媒體資產也有資格擷取及翻譯。
feature: 內容片段，多網站管理員
topic: 本土化
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# AEM內容片段{#translation-support-content-fragments}的翻譯支援

了解如何使用Adobe Experience Manager來本地化和翻譯內容片段。 與內容片段相關聯的混合媒體資產也有資格擷取及翻譯。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## 內容片段翻譯使用案例{#content-fragment-translation-use-cases}

內容片段是已辨識的內容類型，AEM會擷取以傳送至外部翻譯服務。 可立即支援數個使用案例：

1. 您可以直接在Assets主控台中選取內容片段，以執行語言復本和翻譯
2. 系統會將「網站」頁面上參考的內容片段複製到適當的語言資料夾，並在為語言副本選取「網站」頁面時擷取以供翻譯
3. 內嵌於內容片段中的內嵌媒體資產可供擷取和翻譯。
4. 與內容片段相關聯的資產集合適合擷取及翻譯

## 翻譯規則編輯器 {#translation-rules-editor}

Experience Manager翻譯行為可使用&#x200B;**翻譯規則編輯器**&#x200B;更新。 若要更新翻譯，請導覽至&#x200B;**工具** > **一般** > **翻譯設定** ，網址為[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)。

現成可用的設定會參考`fragmentPath`的內容片段，其資源類型為`core/wcm/components/contentfragment/v1/contentfragment`。 預設配置將識別從`v1/contentfragment`繼承的所有元件。

![翻譯規則編輯器](assets/translation-configuration.png)
