---
title: 內容片段的AEM翻譯支援
description: 瞭解如何使用Adobe Experience Manager本地化和翻譯內容片段。 與內容片段關聯的混合媒體資產也有資格被提取和翻譯。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# 內容片段的AEM翻譯支援 {#translation-support-content-fragments}

瞭解如何使用Adobe Experience Manager本地化和翻譯內容片段。 與內容片段關聯的混合媒體資產也有資格被提取和翻譯。

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## 內容片段翻譯使用案例 {#content-fragment-translation-use-cases}

內容片段是識別的內容類型，AEM它提取後要發送到外部翻譯服務。 現成支援幾個使用案例：

1. 內容片段可以是 [直接在Assets控制台中選擇，用於語言複製和翻譯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html)。
2. 在「站點」頁面上引用的內容片段將複製到相應的語言資料夾，並在為語言副本選擇「站點」頁面時提取內容片段以供翻譯。
3. 嵌入在內容片段中的內嵌媒體資產有資格被提取和翻譯。
4. 與內容片段關聯的資產集合有資格被提取和翻譯。

## 翻譯規則編輯器 {#translation-rules-editor}

Experience Manager翻譯行為可通過使用 **翻譯規則編輯器**。 要更新轉換，請導航至 **工具** > **常規** > **翻譯配置** 在 [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)。

現成配置參考位於 `fragmentPath` 資源類型為 `core/wcm/components/contentfragment/v1/contentfragment`。 繼承自 `v1/contentfragment` 由預設配置識別。

![翻譯規則編輯器](assets/translation-configuration.png)
