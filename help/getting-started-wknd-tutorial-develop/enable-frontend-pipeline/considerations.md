---
title: 發展考慮
description: 在啟用前端管線後，請考慮對前端和後端開發流程的影響。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 發展考慮

在使前端管道僅在as a Cloud Service環境中部署前端資源AEM後，會對本地開發產生一些影AEM響，您必須調整Git分支模型。

## 目標

* 如何實現無摩擦的前端和後端開發流
* 查看完整堆棧和前端管線之間的依賴關係


## 地方發展考慮

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 調整後的發展辦法

* 對於使用SDK的本AEM地開發，後端開發團隊仍需要通過 `ui.frontend` 模組，但在將Cloud Manager部署到AEMas a Cloud Service環境時，必須跳過它。 這將說明如何隔離中概述的項目配置更改 [更新項目](update-project.md) 一章。

A __解決方案__ 可以調整git分支模型，並確保項AEM目配置更改永遠不會返回 __地方發展__ 支AEM持後端開發人員。


* 作為對項目進行的增強的一AEM部分，如果您引入了新元件或更新了兩個元件都發生更改的現有元件 `ui.app` 和 `ui.frontend` 模組，必須同時運行全棧和前端管線。
