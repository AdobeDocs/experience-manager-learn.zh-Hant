---
title: 瞭解Adobe雲管理器
description: Adobe雲管理器提供簡單而強健的解決方案，允許輕鬆管理、引言和自AEM助服務。
sub-product: Experience Manager Cloud Manager, Experience Manager
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# 瞭解Adobe雲管理器

Adobe雲管理器提供簡單而強健的解決方案，允許輕鬆管理、引言和自AEM助服務。

## 雲管理器概述

此視頻系列探討了Cloud Manager的關鍵功能，包括AEM:

* [方案](#programs)
* [環境](#environments)
* [報告](#reports)
* [CI/CD 生產管道](#cicd-production-pipeline)
* [CI/CD非生產管道](#cicd-non-production-pipeline)
* [活動](#activity)

有關完整概述，請查看 [《 Cloud Manager使用手冊》](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)。

## 方案 {#programs}

[雲管理器程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) 表示支援AEM邏輯業務計畫集的一組環境，通常對應於購買的服務級別協定(SLA)。 例如，一個方案可以代表支援AEM全球公共網站的資源，而另一個方案則代表一個內部中央資料庫。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 環境 {#environments}

[雲管理器環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) 由AEM Author、AEM Publish和Dispatcher實例組成。 不同的環境支援角色，並可以使用不同的CI/CD管道（如下所述）。 Cloud Manager環境通常具有一個生產環境和一個階段環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## 報告 {#reports}

[雲管理器報告](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) 通過一組圖表來查看程式的環境AEM和實例，這些圖表報告並跟蹤每個實例的各種AEM度量。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 生產管道 {#cicd-production-pipeline}

*[在Adobe雲管理器中使用CI/CD管道](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 視頻系列深入瞭解生產管道的執行，包括對失敗和成功部署的探索。*

>[!NOTE]
>
> 在這些視頻中，已加快了生成、test和部署時間，以縮短視頻的時間。 完整的管道執行通常需要45分鐘或更長時間（包括強制性的30分鐘效能測試），具體取決於項目大小、實例數和UATAEM進程數。

### 設定

的 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 配置定義啟動管道的觸發器以及控制生產部署和效能test參數的參數。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### 管道執行

的 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) 用於通過Stage構建和部署代碼到生產環境，從而縮短價值實現時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD非生產管道 {#cicd-non-production-pipeline}

[CI/CD 非生產用管道分成兩種類別：程式碼品質管道以及部署管道。](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html)程式碼品質管道會從 Git 分支輸入所有程式碼，以建置並根據 Cloud Manager 的程式碼品質掃描加以評估。部署管道會支援將程式碼從 Git 存放庫自動部署到任何非生產環境，這指的是任何已佈建的 AEM 環境 (非預備或生產環境)。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager提供對計畫活動的整合視圖，列出所有CI/CD管道執行，包括生產和非生產，允許查看過去和當前活動，並可以查看任何活動的詳細資訊。

Cloud Manager還以每用戶級別與 [Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)提供對所關心事件和行動的全面瞭解。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
