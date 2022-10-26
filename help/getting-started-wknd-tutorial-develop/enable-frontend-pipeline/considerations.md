---
title: 開發考量事項
description: 啟用前端管道後，請考慮對前端和後端開發流程的影響。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# 開發考量事項

在啟用前端管道僅部署AEMas a Cloud Service環境中的前端資源後，對本機AEM開發會有一些影響，您必須調整Git分支模型。

## 目標

* 如何擁有無摩擦的前端和後端開發流程
* 查看完整堆棧和前端管道之間的依賴關係


## 當地開發考量

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## 調整後發展方法

* 針對使用AEM SDK的本機開發，後端開發團隊仍需透過 `ui.frontend` 模組，但在將Cloud Manager部署至AEMas a Cloud Service環境期間，您必須略過。 這在如何隔離中概述的專案設定變更方面，遇到難題 [更新專案](update-project.md) 章節。

A __解決方案__ 可以調整您的git分支模型，並確認AEM專案設定變更永遠不會回流至 __地方發展__ 分支AEM後端開發人員使用的。


* 如果您導入新元件或更新同時變更的現有元件，這是AEM專案持續增強功能的一部分 `ui.app` 和 `ui.frontend` 模組，您必須同時執行完整堆棧和前端管道。



