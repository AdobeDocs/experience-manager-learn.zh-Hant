---
title: 在Adobe TargetAEM使用體驗片段選件
seo-title: 在Adobe TargetAEM使用體驗片段選件
description: Adobe Experience Manager6.4重新設計了與Target之間的個人AEM化工作流程。 現在，您可AEM以將內建的體驗以HTML選件的形式直接傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。
seo-description: Adobe Experience Manager6.4重新設計了與Target之間的個人AEM化工作流程。 現在，您可AEM以將內建的體驗以HTML選件的形式直接傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。
sub-product: 內容服務
feature: 體驗片段
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: 個性化
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# 在Adobe Target使用體驗片段選件{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager6.4重新設計了與Target之間的個人AEM化工作流程。 現在，您可AEM以將內建的體驗以HTML選件的形式直接傳送至Adobe Target。 它可讓行銷人員跨不同通道順暢地測試和個人化內容。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>建議使用at.js用戶端程式庫，而最佳實務是使用Launch by Adobe、AdobeDTM或任何第三方標籤管理解決方案，將目標程式庫新增至您的網站頁面

>[!NOTE]
>
>Adobe TargetAEM地區的體驗片段選件也以適用於AEM6.3使用者的功能套件形式提供。 請參閱下節的功能套件和相依性資訊。


* Adobe Experience Manager簡單易用、功能強大的內容建立機制以及Adobe Target的人工智慧(AI)和機器學習功能，可協助內容作者在集中位置為所有頻道建立和管理內容。 行銷人員現在可以將「體驗片段」匯出為HTML選件，因此更有彈性地使用這些選件來建立更個人化的體驗，而且現在可以測試並縮放他們建立的每個體驗。
* HTML選件和體驗片段選件之間的主要差異在於，編輯後者只能在中完成，AEM然後與Adobe Target同步
* 套用至「Experience Fragment」檔案夾的Target Cloud服務設定會繼承至直接在父檔案夾下建立的所有「Experience Fragments」。 子資料夾不繼承父雲服務配置。
* 為了建立個人化的優惠，我們現在可以輕鬆運用儲存在其中的內AEM容。
* 您可以建立Target活動類型，包括Sensei支援的活動，例如自動配置、自動定位和Automated Personalization

## AEM 6.3功能套件和相依性{#aem-feature-packs-and-dependencies}

| AEM6.3功能套件 | 相依關係 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## 其他資源 {#additional-resources}

* [體驗片段檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [使用體驗片段](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
