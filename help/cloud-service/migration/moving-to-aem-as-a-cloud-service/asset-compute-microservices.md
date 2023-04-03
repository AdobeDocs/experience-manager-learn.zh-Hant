---
title: AEM Assets微服務與移轉至AEMas a Cloud Service
description: 了解AEM Assets as a Cloud Service的asset compute微服務可如何讓您自動且有效率地產生資產的任何轉譯，取代傳統AEM工作流程的這個角色。
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

# AEM Assets Microservices — 轉移至AEMas a Cloud Service

了解AEM Assets as a Cloud Service的asset compute微服務可如何讓您自動且有效率地產生資產的任何轉譯，取代傳統AEM工作流程的這個角色。

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## 工作流程移轉工具

![資產工作流程移轉工具](./assets/asset-workflow-migration.png)

作為重構程式碼基底的一部分，請使用 [資產工作流程移轉工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) 移轉現有工作流程，以使用AEM as a Cloud Service中的Asset compute微服務。

## 關鍵活動

+ 使用 [Adobe I/O工作流遷移程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) 工具來移轉資產處理工作流程，以使用Asset compute微服務。
+ 設定 [本地開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant) 並部署更新的工作流程。 複雜的工作流程可能需要手動調整。
+ 繼續使用AEM SDK在本機開發環境中反覆查詢，直到更新的工作流程符合功能比對。
+ 將更新的程式碼基底部署至AEMas a Cloud Service開發環境，並繼續驗證。

## 動手練習

通過這個實踐練習來嘗試你學到的東西，來運用你的知識。

在嘗試動手練習之前，請確定您已觀看並了解上述影片，以及下列材料：

+ [對AEMas a Cloud Service有不同的思考](./introduction.md)
+ [上線](./onboarding.md)

此外，請確定您已完成先前的實作練習：

+ [搜索和編製實踐練習索引](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上傳資產的實作</div>
            <p style="margin:1rem 0">
                探索如何使用「aem-upload」npm CLI模組定義AEM Assets處理設定檔，並將其指派給資料夾，以及將資產上傳至AEM。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用資產管理</span>
            </a>
        </td>
    </tr>
</table>
