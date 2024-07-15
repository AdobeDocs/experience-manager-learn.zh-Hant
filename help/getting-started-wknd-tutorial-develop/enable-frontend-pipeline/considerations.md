---
title: 開發考量事項
description: 啟用前端管道後，請考量對前端和後端開發流程的影響。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 開發考量事項

在啟用前端管道僅在AEM as a Cloud Service環境中部署前端資源後，對本機AEM開發有一些影響，因此您必須調整Git分支模型。

## 目標

* 如何擁有順暢的前端與後端開發流程
* 檢閱完整棧疊和前端管道之間的相依性


## 本機開發考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 調整後的開發方法

* 對於使用AEM SDK的本機開發，後端開發團隊仍需要透過`ui.frontend`模組產生clientlib，但在Cloud Manager部署至AEM as a Cloud Service環境期間，您必須略過它。 這會在如何隔離[更新專案](update-project.md)章節中概述的專案設定變更上帶來挑戰。

__解決方案__&#x200B;可能是為了調整您的Git分支模型，並確保AEM專案設定變更絕不會回到AEM後端開發人員使用的&#x200B;__本機開發__&#x200B;分支。


* 作為持續增強您的AEM專案的一部分，如果您引進新元件或更新同時具有`ui.app`和`ui.frontend`模組變更的現有元件，您必須執行完整棧疊和前端管道。
