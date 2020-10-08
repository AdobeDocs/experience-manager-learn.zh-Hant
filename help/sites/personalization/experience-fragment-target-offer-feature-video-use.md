---
title: 在Adobe Target中使用AEM體驗片段選件
seo-title: 在Adobe Target中使用AEM體驗片段選件
description: Adobe Experience Manager 6.4重新塑造了AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以直接以HTML選件的形式傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。
seo-description: Adobe Experience Manager 6.4重新塑造了AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以直接以HTML選件的形式傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。
sub-product: 內容服務
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 1%

---


# 在Adobe Target中使用體驗片段選件{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4重新塑造了AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以直接以HTML選件的形式傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>建議使用at.js用戶端程式庫，而最佳實務是使用標籤管理解決方案（例如Launch By Adobe、Adobe DTM或任何協力廠商標籤管理解決方案），將目標程式庫新增至您的網站頁面

>[!NOTE]
>
>Adobe Target中的AEM體驗片段選件也以AEM 6.3使用者的功能套件形式提供。 請參閱下節的功能套件和相依性資訊。


* Adobe Experience Manager的簡單易用且功能強大的內容建立機制，以及Adobe Target的人工智慧(AI)和機器學習，可協助內容作者在集中位置為所有通道建立和管理內容。 行銷人員現在可以將「體驗片段」匯出為HTML選件，因此更有彈性地使用這些選件來建立更個人化的體驗，而且現在可以測試並縮放他們建立的每個體驗。
* HTML選件和「體驗片段」選件之間的主要差異在於，只有在AEM中才能進行後續的編輯工作，然後才能與Adobe Target同步
* 套用至「Experience Fragment」檔案夾的Target Cloud服務設定會繼承至直接在父檔案夾下建立的所有「Experience Fragments」。 子資料夾不繼承父雲服務配置。
* 若要建立個人化優惠，我們現在可以輕鬆運用儲存在AEM中的內容。
* 您可以建立Target活動類型，包括Sensei支援的活動，例如自動配置、自動定位和自動個人化

## AEM 6.3功能套件與相依性 {#aem-feature-packs-and-dependencies}

| AEM 6.3功能套件 | 相依關係 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## 其他資源 {#additional-resources}

* [體驗片段檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [使用體驗片段](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
