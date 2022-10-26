---
title: 了解AdobeCloud Manager
description: AdobeCloud Manager提供簡單而強大的解決方案，可輕鬆管理、介紹AEM環境並提供自助服務。
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 2%

---

# 了解AdobeCloud Manager

AdobeCloud Manager提供簡單而強大的解決方案，可輕鬆管理、介紹AEM環境並提供自助服務。

## Cloud Manager概述

此影片系列探討適用於AEM的Cloud Manager的主要功能，包括：

* [計劃](#programs)
* [環境](#environments)
* [報表](#reports)
* [CI/CD生產管道](#cicd-production-pipeline)
* [CI/CD非生產管道](#cicd-non-production-pipeline)
* [活動](#activity)

如需完整概觀，請檢閱 [Cloud Manager使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## 計劃 {#programs}

[Cloud Manager方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) 表示支援業務計畫邏輯集的AEM環境集，通常對應於購買的服務級別協定(SLA)。 例如，一個方案可代表支援全域公用網站的AEM資源，而另一個方案則代表內部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) 由AEM Author、AEM Publish和Dispatcher例項組成。 不同的環境支援角色，且可使用不同的CI/CD管道參與（如下所述）。 Cloud Manager環境通常有一個生產環境和一個階段環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 報表 {#reports}

[Cloud Manager報表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) 透過一組圖表提供方案環境和AEM例項的檢視，報告及追蹤每個AEM例項的各種量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生產管道 {#cicd-production-pipeline}

*[在Cloud Manager中使用CI/CD管道Adobe](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 影片系列可深入探討生產管道的執行，包括探索失敗的部署和成功的部署。*

>[!NOTE]
>
> 在這些影片中，建置、測試和部署時間皆已加快，以縮短影片的時間。 完成管道執行通常需要45分鐘或更久的時間（包括強制性的30分鐘效能測試），具體取決於專案大小、AEM例項數和UAT程式數。

### 設定

此 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 設定會定義啟動管道的觸發器，以及控制生產部署和效能測試參數的參數。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道執行

此 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) 可用來透過Stage建置程式碼並部署至生產環境，縮短創造價值的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生產管道 {#cicd-non-production-pipeline}

[CI/CD非生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 分為兩個類別：代碼品質管道和部署管道。 程式碼品質會管道來自Git分支的所有程式碼，以根據Cloud Manager的程式碼品質掃描來建置和評估。 部署管道可支援將程式碼從Git存放庫自動部署至任何非生產環境，亦即任何非預備或生產的布建AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager提供方案活動的整合檢視，列出所有CI/CD管道執行（生產和非生產），讓您能夠洞察過去和目前的活動，並且可以審查任何活動的詳細資料。

Cloud Manager也可在個別使用者層級與 [Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)，提供對所關注事件和行動的全方位檢視。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
