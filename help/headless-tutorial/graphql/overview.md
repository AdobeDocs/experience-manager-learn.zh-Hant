---
title: AEM無周邊功能快速入門 — GraphQL
description: 概述AEM GraphQL API和功能。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---


# AEM無周邊功能快速入門 — GraphQL

AEM GraphQL API（適用於內容片段）
支援無頭式CMS案例，外部用戶端應用程式會使用AEM中管理的內容來呈現體驗。

現代化的內容傳送API是基於Javascript前端應用程式效率與效能的關鍵。 使用REST API會帶來下列挑戰：

* 一次擷取一個物件的請求數量很多
* 通常是「過度傳送」內容，這表示應用程式的接收量超出其需求

為克服這些難題，GraphQL提供了基於查詢的API，允許客戶僅查詢AEM所需的內容，並使用單一API呼叫接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

此影片概略介紹如何在AEM中實作GraphQL API。 AEM中的GraphQL API主要設計為在無周邊部署作業中，將AEM內容片段傳送至下游應用程式。

## AEM無周邊GraphQL視訊系列

透過內容片段和AEM GraphQL API及開發工具的深入逐步說明，了解AEM GraphQL功能。

* [AEM無周邊GraphQL視訊系列](./video-series/modeling-basics.md)

## AEM無頭式GraphQL實作教學課程

建置可透過AEM GraphQL API使用內容片段的React應用程式，探索AEM GraphQL功能。

* [AEM無頭式GraphQL實作教學課程](./multi-step/overview.md)

## AEM GraphQL與AEM內容服務的比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 按GraphQL查詢 | 依AEM頁面 |
| 傳送格式 | GraphQL JSON | AEM ComponentExporter JSON |