---
title: AEM Assets微服務，轉向AEMas a Cloud Service
description: 瞭解AEM Assetsas a Cloud Service的asset compute微服務如何讓您自動高效地為資產生成任何格式副本，從而取代傳統工作流的這一AEM角色。
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 5%

---

# AEM Assets微服務 — 轉AEM向as a Cloud Service

瞭解AEM Assetsas a Cloud Service的asset compute微服務如何讓您自動高效地為資產生成任何格式副本，從而取代傳統工作流的這一AEM角色。

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## 工作流遷移工具

![資產工作流程移轉工具](./assets/asset-workflow-migration.png)

作為重構代碼庫的一部分，使用 [資產工作流遷移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) 遷移現有工作流以在as a Cloud Service中使用Asset compute微AEM服務。

## 關鍵活動

+ 使用 [Adobe I/O工作流遷移程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) 工具，用於遷移資產處理工作流以使用Asset compute微服務。
+ 設定 [地方開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant) 並部署更新的工作流。 複雜的工作流可能需要手動調整。
+ 繼續使用SDK在本地開發環境中迭代，AEM直到更新的工作流與功能奇偶校驗匹配。
+ 將更新的代碼庫部署到AEMas a Cloud Service開發環境並繼續驗證。

## 動手練習

通過嘗試通過實際操作所學到的知識來應用您的知識。

在嘗試動手練習之前，請確保您已觀看並瞭解上面的視頻，以及以下資料：

+ [對as a Cloud Service的不AEM同思考](./introduction.md)
+ [上線](./onboarding.md)

另外，確保您已完成以前的動手練習：

+ [搜索和索引動手練習](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="實際練習GitHub儲存庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上傳資產的操作</div>
            <p style="margin:1rem 0">
                瞭解如何使用'aem-upload' npm CLI模組定義和分配AEM Assets處理配置文AEM件到資料夾，以及將資產上載到。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試資產管理</span>
            </a>
        </td>
    </tr>
</table>
