---
title: 瞭解AdobeCloud Manager
description: AdobeCloud Manager提供簡單但強大的解決方案，可讓您輕鬆管理、深入分析並自助服務AEM環境。
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 15%

---

# 瞭解AdobeCloud Manager

AdobeCloud Manager提供簡單但強大的解決方案，可讓您輕鬆管理、深入分析並自助服務AEM環境。

## Cloud Manager 總覽

本影片系列會探討適用於AEM的Cloud Manager的主要功能，包括：

* [方案](#programs)
* [環境](#environments)
* [報告](#reports)
* [CI/CD 生產管道](#cicd-production-pipeline)
* [CI/CD非生產用管道](#cicd-non-production-pipeline)
* [活動](#activity)

如需完整的總覽，請檢閱[Cloud Manager使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=zh-Hant)。

## 方案 {#programs}

[Cloud Manager計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=zh-Hant)代表支援邏輯業務計畫集合的AEM環境集合，通常會對應到已購買的服務等級協定(SLA)。 例如，一個計畫可能代表AEM資源以支援全球公共網站，而另一個計畫代表內部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=zh-Hant)由AEM Author、AEM Publish和Dispatcher執行個體組成。 不同的環境可支援角色，並使用不同的CI/CD管道與其互動（如下所述）。 Cloud Manager環境通常有一個生產環境和一個中繼環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## 報告 {#reports}

[Cloud Manager報表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=zh-Hant)透過一組圖表提供計畫的環境和AEM執行個體的檢視，這些圖表報告並追蹤每個AEM執行個體的各種量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 生產管道 {#cicd-production-pipeline}

*[在AdobeCloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)影片系列中使用CI/CD管道可深入瞭解生產管道的執行，包括探索失敗和成功的部署。*

>[!NOTE]
>
> 透過這些影片，已加快建置、測試和部署時間，以縮短影片時間。 根據專案大小、AEM執行個體數量和UAT流程，完整的管道執行通常需要45分鐘或更長時間（包括強制性的30分鐘效能測試）。

### 設定

[CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=zh-Hant)設定會定義起始管道的觸發程式，以及控制生產部署與效能測試引數的引數。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### 管道執行

[CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=zh-Hant)可用來建置程式碼，並透過Stage部署至生產環境，減少值所需的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD非生產用管道 {#cicd-non-production-pipeline}

[CI/CD非生產用管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=zh-Hant)分成兩個類別：程式碼品質管道和部署管道。 程式碼品質管道會從 Git 分支輸入所有程式碼，以建置並根據 Cloud Manager 的程式碼品質掃描加以評估。部署管道會支援將程式碼從 Git 存放庫自動部署到任何非生產環境，這指的是任何已佈建的 AEM 環境 (非預備或生產環境)。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager會提供方案活動的整合式檢視，以及所有CI/CD管道執行的清單（包括生產和非生產），讓您檢視過去和現在的活動，以及可以檢視任何活動的詳細資訊。

Cloud Manager也整合了每個使用者層級的[Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=zh-Hant)，提供全方位檢視感興趣的事件和動作。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
