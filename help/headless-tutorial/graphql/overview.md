---
title: 開始使用AEM Headless - GraphQL
description: 了解Experience ManagerGraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 11%

---

# AEM Headless - GraphQL 快速入門 {#getting-started-with-aem-headless}

AEM GraphQL API for內容片段支援無頭CMS案例，供外部用戶端應用程式使用AEM中管理的內容呈現體驗。

現代化的內容傳送API是基於Javascript前端應用程式效率與效能的關鍵。 使用REST API會帶來下列挑戰：

* 一次擷取一個物件的請求數很多
* 通常是「過度傳送」內容，這表示應用程式收到的內容超出其需求

為克服這些難題，GraphQL提供以查詢為基礎的API，讓用戶端只能查詢AEM所需的內容，並透過單一API呼叫接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

此影片概述在AEM中實作的GraphQL API。 AEM中的GraphQL API主要設計為在無頭部署作業中，將AEM內容片段傳送至下游應用程式。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless - GraphQL 快速入門"
>abstract="了解如何使用 GraphQL 傳遞內容片段。"
>additional-url="https://video.tv.adobe.com/v/328618" text="AEM 中的 GraphQL 概觀"

## AEM無頭GraphQL影片系列

透過內容片段和AEM GraphQL API及開發工具的深入逐步說明，了解AEM GraphQL功能。

* [AEM無頭GraphQL影片系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL實作教學課程

透過AEM GraphQL API建置會取用內容片段的React應用程式，探索AEM GraphQL功能。

* [AEM Headless GraphQL實作教學課程](./multi-step/overview.md)

## AEM GraphQL與AEM Content Services的比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 按GraphQL查詢 | 依AEM頁面 |
| 傳送格式 | GraphQL JSON | AEM ComponentExporter JSON |
