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
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

了解AEM Dispatcher for AEMas a Cloud Service，著重於Dispatcher for AEM 6、Dispatcher轉換工具以及如何使用Dispatcher工具SDK的重大變更。

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher 轉換工具

![Dispatcher 轉換工具](./assets/dispatcher-converter-diagram.png)

作為重構程式碼基底的一部分，請使用 [AEM Dispatcher轉換工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 將現有的內部部署或Adobe Managed Services Dispatcher設定重新調整為AEMas a Cloud Service相容的Dispatcher設定。

### 關鍵活動

* 使用 [Adobe I/ODispatcher轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 移轉現有Dispatcher設定。
* 從 [AEM專案原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作為最佳實務。
* [設定本機Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 驗證dispatcher，然後再於Cloud Service環境中進行測試。


