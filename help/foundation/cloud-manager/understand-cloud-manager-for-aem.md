---
title: 瞭解Adobe雲管理員
description: Adobe雲管理器提供簡單但強穩的解決方案，允許輕鬆管理、查看和自助服務環AEM境。
sub-product: 雲端管理員，基礎
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: 架構
role: 架構師
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 3%

---


# 瞭解Adobe雲管理員

Adobe雲管理器提供簡單但強穩的解決方案，允許輕鬆管理、查看和自助服務環AEM境。

## Cloud Manager概觀

本影片系列探討Cloud Manager的主要功能，包括：

* [計劃](#programs)
* [環境](#environments)
* [報表](#reports)
* [CI/CD生產管道](#cicd-production-pipeline)
* [CI/CD非生產管道](#cicd-non-production-pipeline)
* [活動](#activity)

如需完整概觀，請參閱[Cloud Manager使用指南](https://docs.adobe.com/content/help/zh-Hant/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)。

## 計劃 {#programs}

[Cloud Manager ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) Programs表示支援AEM邏輯業務計畫集的一組環境，通常對應於購買的服務級別協定(SLA)。例如，一個方案可代表支AEM援全球公共網站的資源，而另一個方案則代表內部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) Environments由AEM Author、AEM Publish和Dispatcher例項組成。不同的環境支援角色，可使用不同的CI/CD管道（如下所述）參與。 Cloud Manager環境通常有一個生產環境和一個階段環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 報表 {#reports}

[Cloud Manager ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) Reports透過一組圖表提供方案AEM環境與例項的檢視，這些圖表會報告並追蹤每個例項的各種度AEM量。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生產管線{#cicd-production-pipeline}

*[在AdobeCloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager視訊系列中使用CI/CD管道可深入探討Production Pipeline的執行，包括探索部署失敗和成功。*

>[!NOTE]
>
> 在這些視訊中，建置、測試和部署時間已加快，以縮短視訊的時間。 完整的管道執行通常需要45分鐘或更長時間（包括強制性的30分鐘效能測試），視專案大小、例項數AEM和UAT程式而定。

### 設定

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)配置定義將啟動管線的觸發器、控制生產部署和效能測試參數的參數。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管線執行

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)用於通過Stage構建代碼並將代碼部署到生產環境，從而縮短了實現價值的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生產管線{#cicd-non-production-pipeline}

[CI/CD非生產管](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 線分為代碼質量管線和部署管線兩類。程式碼品質會從Git分支輸入所有程式碼，以根據Cloud Manager的程式碼品質掃描來建立和評估。 部署管線支援將代碼從Git儲存庫自動部署到任何非生產環境，這意味著任何非生AEM產環境的已布建環境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活動 {#activity}

Cloud Manager提供了方案活動的整合視圖，列出了所有CI/CD Pipeline執行（包括生產和非生產），允許查看過去和現在的活動，並可以查看任何活動的詳細資訊。

Cloud Manager還在每位使用者層級與[Adobe Experience Cloud通知](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)整合，提供對所關注事件和動作的全方位檢視。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
