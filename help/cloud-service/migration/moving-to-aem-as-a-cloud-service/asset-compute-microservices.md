---
title: AEM Assets微服務和移至AEM as a Cloud Service
description: 瞭解AEM Assets as a Cloud Service的資產計算微服務如何讓您有效率地產生您資產的任何轉譯，取代此傳統AEM Workflow的角色。
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# AEM Assets微服務 — 移至AEM as a Cloud Service

瞭解AEM Assets as a Cloud Service的資產計算微服務如何讓您有效率地產生您資產的任何轉譯，取代此傳統AEM Workflow的角色。

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## 工作流程移轉工具

![資產工作流程移轉工具](./assets/asset-workflow-migration.png)

在重構程式碼基底的過程中，請使用[資產工作流程移轉工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=zh-Hant)移轉現有的工作流程，以便在AEM as a Cloud Service中使用Asset Compute微服務。

## 重要活動

+ 使用[Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator)工具移轉資產處理工作流程，以使用Asset Compute微服務。
+ 設定[本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)並部署更新的工作流程。 複雜的工作流程可能需要手動調整。
+ 繼續使用AEM SDK在本機開發環境中反複進行，直到更新的工作流程符合功能對等為止。
+ 將更新的程式碼基底部署到AEM as a Cloud Service開發環境，並繼續驗證。

## 實作練習

透過嘗試您透過此實作練習學到的內容，運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [以不同方式思考AEM as a Cloud Service](./introduction.md)
+ [上線](./onboarding.md)

此外，請確定您已完成前一個實作練習：

+ [搜尋和建立索引實作練習](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上傳資產的實際操作</div>
            <p style="margin:1rem 0">
                探索如何使用「aem-upload」npm CLI模組來定義及指派AEM Assets處理設定檔至資料夾，以及上傳資產至AEM。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試資產管理</span>
            </a>
        </td>
    </tr>
</table>
