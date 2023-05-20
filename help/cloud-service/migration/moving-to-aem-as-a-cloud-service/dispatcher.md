---
title: 在移動到AEMas a Cloud Service
description: 瞭解對Dispatcher for AEMas a Cloud Service、Dispatcher轉換工AEM具以及如何使用Dispatcher Tools SDK的顯著更改。
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

了AEM解Dispatcher for AEMfergie，重點介紹Dispatcher for AEM 6 、 Dispatcher轉換工具以及如何使用Dispatcher Tools SDK的顯著更改。

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher 轉換工具

![Dispatcher 轉換工具](./assets/dispatcher-converter-diagram.png)

作為重構代碼庫的一部分，使用 [調度AEM器轉換器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 將現有的本地或Adobe Managed Services Dispatcher配置重構為AEMas a Cloud Service相容的Dispatcher配置。

## 關鍵活動

+ 使用 [Adobe I/O調度器轉換器工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 遷移現有Dispatcher配置。
+ 從 [項AEM目原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作為最佳實踐。
+ [設定本地Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 在Cloud Service環境中進行測試之前驗證調度程式。

## 動手練習

通過嘗試通過實際操作所學到的知識來應用您的知識。

在嘗試動手練習之前，請確保您已觀看並瞭解上面的視頻，以及以下資料：

+ [AEM 現代化工具](./aem-modernization-tools.md)
+ [上線](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

另外，確保您已完成以前的動手練習：

+ [Cloud Manager的動手練習](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="實際練習GitHub儲存庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher Tools的操作</div>
            <p style="margin:1rem 0">
                使用SDK的AEMDispatcher Tools進行瀏覽，以驗證Dispatcher配置，並使用Docker在本AEM地運行Dispatcher。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用Dispatcher工具</span>
            </a>
        </td>
    </tr>
</table>
