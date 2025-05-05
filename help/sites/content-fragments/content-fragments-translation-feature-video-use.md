---
title: AEM內容片段的翻譯支援
description: 瞭解如何使用Adobe Experience Manager將內容片段當地語系化及翻譯。 與內容片段相關的混合媒體資產也有資格被擷取及翻譯。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# AEM內容片段的翻譯支援 {#translation-support-content-fragments}

瞭解如何使用Adobe Experience Manager將內容片段當地語系化及翻譯。 與內容片段相關的混合媒體資產也有資格被擷取及翻譯。

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## 內容片段翻譯使用案例 {#content-fragment-translation-use-cases}

內容片段是AEM擷取並傳送至外部翻譯服務的認可內容型別。 現成支援數個使用案例：

1. 可以直接在Assets主控台中選取內容片段[以進行語言複製和翻譯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=zh-Hant)。
2. 為語言複製選取網站頁面時，在網站頁面上參考的內容片段會複製到適當的語言資料夾並擷取以供翻譯。
3. 內嵌在內容片段中的內嵌媒體資產符合擷取和翻譯的條件。
4. 與內容片段相關的資產集合符合擷取和翻譯的條件。

## 翻譯規則編輯器 {#translation-rules-editor}

可以使用&#x200B;**翻譯規則編輯器**&#x200B;來更新Experience Manager翻譯行為。 若要更新翻譯，請瀏覽至[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)的&#x200B;**工具** > **一般** > **翻譯組態**。

現成可用的設定會參考資源型別為`core/wcm/components/contentfragment/v1/contentfragment`且位於`fragmentPath`的內容片段。 預設組態會識別從`v1/contentfragment`繼承的所有元件。

![翻譯規則編輯器](assets/translation-configuration.png)
