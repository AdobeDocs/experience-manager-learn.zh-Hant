---
title: 了解AdobeCloud Manager
description: AdobeCloud Manager提供簡單而強大的解決方案，可輕鬆管理、審視和自助服務AEM環境。
sub-product: cloud-manager,foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: 架構
role: Architect
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# 了解AdobeCloud Manager

AdobeCloud Manager提供簡單而強大的解決方案，可輕鬆管理、審視和自助服務AEM環境。

## Cloud Manager概述

此影片系列探討適用於AEM的Cloud Manager的主要功能，包括：

* [計劃](#programs)
* [環境](#environments)
* [報表](#reports)
* [CI/CD生產管道](#cicd-production-pipeline)
* [CI/CD非生產管道](#cicd-non-production-pipeline)
* [活動](#activity)

如需完整概述，請檢閱[Cloud Manager使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=zh-Hant)。

## 計劃 {#programs}

[Cloud Manager程](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) 式代表一組支援業務計畫邏輯集的AEM環境，通常對應於已購買的服務級別協定(SLA)。例如，一個方案可代表支援全域公用網站的AEM資源，而另一個方案則代表內部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager環](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) 境由AEM製作、AEM Publish和Dispatcher例項組成。不同的環境支援角色，且可使用不同的CI/CD管道參與（如下所述）。 Cloud Manager環境通常有一個生產環境和一個階段環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 報表 {#reports}

[Cloud Manager報](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 表可透過一組圖表提供方案環境和AEM例項的檢視，以報告並追蹤每個AEM例項的各種量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生產管道 {#cicd-production-pipeline}

*[在Cloud Manager影片系列中使用CI/CD管](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 道，深入探討生產管道執行，包括探索失敗的部署和成功的部署。*

>[!NOTE]
>
> 在這些影片中，建置、測試和部署時間都已加快，以縮短影片的時間。 完成管道執行通常需要45分鐘或更久的時間（包括強制性的30分鐘效能測試），具體取決於專案大小、AEM例項數和UAT程式數。

### 設定

[CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)配置定義將啟動管道的觸發器、控制生產部署和效能測試參數的參數。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道執行

[CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)用於透過Stage建置程式碼並部署至生產環境，縮短創造價值的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生產管道 {#cicd-non-production-pipeline}

[CI/CD非生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 分為兩類：代碼品質管道和部署管道。程式碼品質會管道來自Git分支的所有程式碼，以根據Cloud Manager的程式碼品質掃描來建置和評估。 部署管道可支援將程式碼從Git存放庫自動部署至任何非生產環境，亦即任何非預備或生產的布建AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager提供方案活動的整合檢視，列出所有CI/CD管道執行（生產和非生產），讓您能夠洞察過去和目前的活動，並且可以審查任何活動的詳細資料。

Cloud Manager也在每個使用者層級與[Adobe Experience Cloud Notifications](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/notifications.html)整合，提供您所關心事件和動作的全方位檢視。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
