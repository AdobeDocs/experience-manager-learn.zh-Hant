---
title: AEM Headless 快速入門 - GraphQL
description: 了解 Experience Manager GraphQL API 及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '266'
ht-degree: 100%

---

# AEM Headless 快速入門 - GraphQL {#getting-started-with-aem-headless}

AEM 的內容片段 GraphQL API
支援無周邊 CMS 情境，其中外部用戶端應用程式會使用 AEM 中所管理的內容來轉譯體驗。

現代的內容傳遞 API 是 Javascript 型前端應用程式發揮效率和效能的關鍵。使用 REST API 會帶來的挑戰：

* 一次發生擷取一個物件的大量要求
* 經常「過度傳遞」內容，表示應用程式收到的內容超出其需求

為克服這些挑戰，GraphQL 提供查詢型 API，讓用戶端能夠僅針對其所需內容向 AEM 查詢，並使用單一 API 呼叫接收。

>[!VIDEO](https://video.tv.adobe.com/v/3452890?quality=12&learn=on&captions=chi_hant)

此影片為 AEM 中實施之 GraphQL API 的概觀。在 AEM 中，GraphQL API 的主要功能是在無周邊部署作業中，將 AEM 內容片段傳遞給下游應用程式。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless - GraphQL 快速入門"
>abstract="了解如何使用 GraphQL 傳遞內容片段。"
>additional-url="https://video.tv.adobe.com/v/3452890?captions=chi_hant" text="AEM 中的 GraphQL 概觀"

## AEM Headless GraphQL 影片系列

透過內容片段以及 AEM GraphQL API 和開發工具的深入逐步講解，了解 AEM 的 GraphQL 功能。

* [AEM Headless GraphQL 影片系列](./video-series/modeling-basics.md)

## AEM Headless GraphQL 實作教學課程

建置一個透過 AEM GraphQL API 使用內容片段的 React 應用程式，探索 AEM 的 GraphQL 功能。

* [AEM Headless GraphQL 實作教學課程](./multi-step/overview.md)

## 比較 AEM GraphQL 與 AEM 內容服務

|                                | AEM GraphQL API | AEM 內容服務 |
|--------------------------------|:-----------------|:---------------------|
| 結構描述定義 | 結構化內容片段模型 | AEM 元件 |
| 內容 | 內容片段 | AEM 元件 |
| 內容探索 | 透過 GraphQL 查詢 | 透過 AEM 頁面 |
| 傳遞格式 | GraphQL JSON | AEM ComponentExporter JSON |
