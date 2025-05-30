---
title: 開始使用AEM Headless - GraphQL
description: 瞭解Experience Manager GraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# AEM Headless - GraphQL 快速入門 {#getting-started-with-aem-headless}

AEM的內容片段GraphQL API
支援headless CMS情境，讓外部使用者端應用程式使用AEM中管理的內容呈現體驗。

現代化的內容傳送API是Javascript型前端應用程式效率和效能的關鍵。 使用REST API會帶來挑戰：

* 一次擷取一個物件的大量請求
* 通常「過度傳遞」內容，表示應用程式收到的內容多於其需求

為克服這些挑戰，GraphQL提供查詢式API，讓客戶可查詢AEM所需的內容，並使用單一API呼叫接收內容。

>[!VIDEO](https://video.tv.adobe.com/v/3452890?quality=12&learn=on&captions=chi_hant)

這部影片會概略介紹如何在AEM中實作GraphQL API。 AEM中的GraphQL API主要是將AEM內容片段的傳送給下游應用程式，作為Headless部署的一部分。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless - GraphQL 快速入門"
>abstract="了解如何使用 GraphQL 傳遞內容片段。"
>additional-url="https://video.tv.adobe.com/v/3452890?captions=chi_hant" text="AEM 中的 GraphQL 概觀"

## AEM Headless GraphQL影片系列

透過內容片段和AEM的GraphQL API及開發工具的深入逐步說明，瞭解AEM的GraphQL功能。

* [AEM Headless GraphQL影片系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL實作教學課程

透過AEM的GraphQL API建置使用內容片段的React應用程式，探索AEM的GraphQL功能。

* [AEM Headless GraphQL實作教學課程](./multi-step/overview.md)

## AEM GraphQL與AEM Content Services

|                                | AEM GRAPHQL API | AEM內容服務 |
|--------------------------------|:-----------------|:---------------------|
| 結構描述定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 依GraphQL查詢 | 依AEM頁面 |
| 傳遞格式 | GRAPHQL JSON | AEM ComponentExporter JSON |
