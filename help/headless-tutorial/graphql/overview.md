---
title: 開始使用無AEM頭 — GraphQL
description: 瞭解Experience ManagerGraphQL API及其功能。
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# 無頭入門AEM- GraphQL

用AEM於內容片段的GraphQL API支援無頭CMS方案，其中外部客戶端應用程式使用中管理的內容呈現AEM體驗。

基於Javascript的前端應用程式的效率和效能關鍵在於現代內容傳遞API。 使用REST API將帶來以下挑戰：

* 一次獲取一個對象的請求數量很大
* 通常是「過量交付」內容，這意味著應用程式收到的內容超出其需要

為克服這些難題，GraphQL提供了基於查詢的API，允許客戶AEM只查詢它需要的內容，並使用單個API調用接收。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

此視頻是中實現的GraphQL API的概AEM述。 中的GraphQL APIAEM主要設計為將內AEM容片段作為無頭部部署的一部分提供到下游應用程式。

## 無AEM頭GraphQL視頻系列

通過深入AEM瀏覽內容片段和GraphQL API和開發工具，瞭解AEMGraphQL功能。

* [無AEM頭GraphQL視頻系列](./video-series/modeling-basics.md)

## 無AEM頭GraphQL操作教程

通過AEM構建通過GraphQL API消耗內容片段的反應應應AEM用來瀏覽GraphQL功能。

* [無AEM頭GraphQL操作教程](./multi-step/overview.md)

## AEMGraphQL與內AEM容服務

|  | AEMGraphQL API | 內AEM容服務 |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | 組AEM件 |
| 內容 | 內容片段 | 組AEM件 |
| 內容發現 | 按GraphQL查詢 | 按頁AEM面 |
| 傳遞格式 | GraphQL JSON | 組AEM件導出器JSON |
