---
title: 在移至AEMas a Cloud Service時設定Dispatcher
description: 瞭解AEM Dispatcher for AEMas a Cloud Service的重大變更、Dispatcher轉換工具以及如何使用Dispatcher Tools SDK。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1634
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

瞭解AEM適用的AEM Dispatcheras a Cloud Service，重點放在適用於AEM 6的Dispatcher、Dispatcher轉換工具及如何使用Dispatcher Tools SDK。

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher轉換工具

![Dispatcher 轉換工具](./assets/dispatcher-converter-diagram.png)

在重構程式碼庫時，請使用 [AEM Dispatcher轉換工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 若要將現有的內部部署或AdobeManaged Services Dispatcher設定重構為與AEMas a Cloud Service相容的Dispatcher設定。

## 重要活動

+ 使用 [Adobe I/ODispatcher轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 移轉現有的Dispatcher設定。
+ 從參照Dispatcher模組 [AEM專案原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作為最佳實務。
+ [設定本機Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 以驗證Dispatcher，然後在Cloud Service環境中進行測試。

## 實作練習

透過嘗試您透過此實作練習學到的內容，運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [AEM 現代化工具](./aem-modernization-tools.md)
+ [上線](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

此外，請確定您已完成前一個實作練習：

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
            <div style="font-size:1.25rem;font-weight:400;">使用Dispatcher工具實作</div>
            <p style="margin:1rem 0">
                探索使用AEM SDK的Dispatcher工具來驗證Dispatcher設定，以及使用Docker在本機執行AEM Dispatcher。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用Dispatcher工具</span>
            </a>
        </td>
    </tr>
</table>
