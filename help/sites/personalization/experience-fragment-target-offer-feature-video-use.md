---
title: 在Adobe Target中使用AEM體驗片段選件
seo-title: 在Adobe Target中使用AEM體驗片段選件
description: Adobe Experience Manager 6.4會重新調整AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以以HTML選件的形式直接傳遞至Adobe Target。 它可讓行銷人員在不同管道間順暢地測試及個人化內容。
seo-description: Adobe Experience Manager 6.4會重新調整AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以以HTML選件的形式直接傳遞至Adobe Target。 它可讓行銷人員在不同管道間順暢地測試及個人化內容。
sub-product: content-services
feature: 體驗片段
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: 個性化
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 11%

---


# 在Adobe Target中使用體驗片段選件{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4會重新調整AEM和Target之間的個人化工作流程。 現在，在AEM中建立的體驗可以以HTML選件的形式直接傳遞至Adobe Target。 它可讓行銷人員在不同管道間順暢地測試及個人化內容。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>建議使用at.js用戶端程式庫，最佳實務是使用標籤管理解決方案，例如Launch by Adobe、AdobeDTM或任何第三方標籤管理解決方案，將目標程式庫新增至您的網站頁面

>[!NOTE]
>
>Adobe Target中的AEM體驗片段選件也以AEM 6.3使用者適用的Feature Pack形式提供。 請參閱下節中的Feature Pack和相依性。


* Adobe Experience Manager簡單易用且功能強大的內容建立機制，以及Adobe Target的人工智慧(AI)和機器學習，可協助內容作者在集中位置為所有頻道建立和管理內容。 透過將體驗片段匯出為HTML選件的功能，行銷人員現在可以更靈活地使用這些選件來建立更個人化的體驗，並且現在可以測試和縮放他們建立的每個體驗。
* HTML選件和體驗片段選件之間的主要差異，在於您只能在AEM中編輯，然後與Adobe Target同步
* 套用至體驗片段資料夾的Target雲端服務設定會繼承至直接在上層資料夾下建立的所有體驗片段。 子資料夾不繼承父雲服務配置。
* 若要建立個人化優惠方案，我們現在可以輕鬆運用AEM中儲存的內容。
* 您可以建立Target活動類型，包括由Sensei支援的活動，例如自動分配、自動鎖定目標和Automated Personalization

## AEM 6.3 Feature Pack和相依性 {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | 相依關係 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## 其他資源 {#additional-resources}

* [體驗片段檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [使用體驗片段](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
