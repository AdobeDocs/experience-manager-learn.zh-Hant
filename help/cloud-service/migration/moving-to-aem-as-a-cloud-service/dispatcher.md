---
title: 在移至AEM時設定Dispatcheras a Cloud Service
description: 了解AEM Dispatcher for AEMas a Cloud Service、Dispatcher轉換工具的重大變更，以及如何使用Dispatcher工具SDK。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 6%

---


# Dispatcher

了解AEM Dispatcher for AEMas a Cloud Service，著重於Dispatcher for AEM 6、Dispatcher轉換工具以及如何使用Dispatcher工具SDK的重大變更。

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher 轉換工具

![Dispatcher 轉換工具](./assets/dispatcher-converter-diagram.png)

作為重構程式碼基底的一部分，請使用 [AEM Dispatcher轉換工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 將現有的內部部署或Adobe Managed Services Dispatcher設定重新調整為AEMas a Cloud Service相容的Dispatcher設定。

## 關鍵活動

+ 使用 [Adobe I/ODispatcher轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 移轉現有Dispatcher設定。
+ 從 [AEM專案原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作為最佳實務。
+ [設定本機Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 驗證dispatcher，然後再於Cloud Service環境中進行測試。

## 動手練習

通過這個實踐練習來嘗試你學到的東西，來運用你的知識。

在嘗試動手練習之前，請確定您已觀看並了解上述影片，以及下列材料：

+ [AEM 現代化工具](./aem-modernization-tools.md)
+ [上線](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

此外，請確定您已完成先前的實作練習：

+ [Cloud Manager實作練習](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher工具的實作</div>
            <p style="margin:1rem 0">
                探索如何使用AEM SDK的Dispatcher工具來驗證Dispatcher設定，以及使用Docker在本機執行AEM Dispatcher。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用Dispatcher工具</span>
            </a>
        </td>
    </tr>
</table>
