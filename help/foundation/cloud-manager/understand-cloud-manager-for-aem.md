---
title: 瞭解Adobe Cloud Manager
description: Adobe Cloud Manager提供簡單但強穩的解決方案，讓AEM環境的管理、檢視和自助服務變得輕鬆。
sub-product: 雲端管理員，基礎
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 3%

---


# 瞭解Adobe Cloud Manager

Adobe Cloud Manager提供簡單但強穩的解決方案，讓AEM環境的管理、檢視和自助服務變得輕鬆。

## Cloud Manager概觀

此影片系列探討Cloud Manager for AEM的主要功能，包括：

* [計劃](#programs)
* [環境](#environments)
* [報表](#reports)
* [CI/CD生產管道](#cicd-production-pipeline)
* [CI/CD非生產管道](#cicd-non-production-pipeline)
* [活動](#activity)

如需完整概觀，請參閱 [Cloud Manager使用指南](https://docs.adobe.com/content/help/zh-Hant/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)。

## 計劃 {#programs}

[Cloud Manager計畫](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) (Cloud Manager Programs)代表一組AEM環境，這些環境支援邏輯業務計畫集，通常對應於購買的服務級別協定(SLA)。 例如，一個方案可代表AEM資源以支援全球公共網站，而另一個方案則代表內部的Central DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager環境](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) ，由AEM Author、AEM Publish和Dispatcher例項組成。 不同的環境支援角色，可使用不同的CI/CD管道（如下所述）參與。 Cloud Manager環境通常有一個生產環境和一個階段環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 報表 {#reports}

[「Cloud Manager報表](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 」透過一組圖表提供方案環境和AEM例項的檢視，這些圖表可報告並追蹤每個AEM例項的各種量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[使用Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)video系列中的CI/CD管道可深入探討Production Pipeline的執行，包括探索部署失敗和成功。*

>[!NOTE]
>
> 在這些視訊中，建置、測試和部署時間已加快，以縮短視訊的時間。 完整的管道執行通常需要45分鐘或更長時間（包括強制性的30分鐘效能測試），視專案大小、AEM例項數和UAT程式數而定。

### 設定

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) （生產管線）配置定義將啟動管線的觸發器、控制生產部署和效能測試參數的參數。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管線執行

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) （CI/CD製作管道）用於透過Stage建立程式碼並部署至生產環境，以縮短實現價值的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生產管道 {#cicd-non-production-pipeline}

[CI/CD非生產管道](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) ，分為代碼質量管道和部署管道兩類。 程式碼品質會從Git分支輸入所有程式碼，以根據Cloud Manager的程式碼品質掃描來建立和評估。 部署管道支援將程式碼從Git存放庫自動部署至任何非生產環境，這表示任何非Stage或Production的已布建AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager提供了方案活動的整合視圖，列出了所有CI/CD Pipeline執行（包括生產和非生產），允許查看過去和現在的活動，並可以查看任何活動的詳細資訊。

Cloud Manager也可在每位使用者層級與 [Adobe Experience Cloud Notifications整合](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)，提供對所關注事件和動作的全方位檢視。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
