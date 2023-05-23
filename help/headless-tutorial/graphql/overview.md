---
title: 開始使用AEMHeadless -GraphQL
description: 瞭解Experience ManagerGraphQLAPI及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 11%

---

# AEM Headless - GraphQL 快速入門 {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

用AEM於內容片段的GraphQLAPI支援無頭CMS方案，其中外部客戶端應用程式使用中管理的內容呈現AEM體驗。

基於Javascript的前端應用程式的效率和效能關鍵在於現代內容傳遞API。 使用REST API將帶來以下挑戰：

* 一次獲取一個對象的請求數量很大
* 通常是「過量交付」內容，這意味著應用程式收到的內容超出其需要

為克服這些挑戰，GraphQL提供了基於查詢的API，允許客戶AEM僅查詢它需要的內容，並使用單個API調用接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

此視頻是中實現的GraphQLAPI的概AEM述。 中的GraphQLAPIAEM主要設計為將內AEM容片段作為無頭部署的一部分提供到下游應用程式。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless - GraphQL 快速入門"
>abstract="了解如何使用 GraphQL 傳遞內容片段。"
>additional-url="https://video.tv.adobe.com/v/328618" text="AEM 中的 GraphQL 概觀"

## 無AEM頭GraphQL系列

通過深入AEM瀏覽內容片段和GraphQLAPI以及開發工具，瞭解GraphQLAEM的功能。

* [無AEM頭GraphQL系列](./video-series/modeling-basics.md)

## 無AEM頭GraphQL實踐教程

通過構AEM建通過GraphQLAPI消耗內容片段的React App來探索GraphQLAEM功能。

* [無AEM頭GraphQL實踐教程](./multi-step/overview.md)

## AEMGraphQL與內AEM容服務

|  | GraphQLAEMAPI | 內AEM容服務 |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | 組AEM件 |
| 內容 | 內容片段 | 組AEM件 |
| 內容發現 | 按GraphQL查詢 | 按頁AEM面 |
| 傳遞格式 | GraphQLJSON | 組AEM件導出器JSON |
