---
title: 無頭入門AEM — 內容服務
description: 端對端的教學課程，說明如何使用 AEM Headless 來建立和公開內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 6%

---

# AEM Headless 快速入門 - Content Services

ContentAEM Services利用傳統AEM頁面來構成無頭REST API端點，AEM而元件定義或引用這些端點上要公開的內容。

Content AEM Services允許使用與在AEM Sites創作網頁時使用的相同內容抽象來定義這些HTTP API的內容和架構。 使用「頁AEM面」和「AEM元件」使營銷人員能夠快速合成和更新可支援任何應用程式的靈活JSON API。

## 內容服務教程

一個端到端教程，演示如何在無頭CMS方案中使用和AEM使用本機移動應用構建和公開內容。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

本教程探AEM討如何使用內容服務為顯示事件資訊（音樂、效能、藝術等）的移動應用提供強大的體驗 由WKND團隊策劃。

本教程將介紹以下主題：

* 使用內容片段建立表示事件的內容
* 使用AEM站AEM點的模板和將事件資料公開為JSON的頁面定義Content Services端點
* 探AEM討如何使用WCM核心元件使營銷人員能夠編寫JSON端點
* 從移AEM動應用使用Content Services JSON
   * Android的使用是因為它有一個跨平台模擬器，本教程的所有用戶(Windows、macOS和Linux)都可以使用該模擬器運行本機App。

## GitHub項目

原始碼和內容包在 [指AEM南 — WKND Mobile GitHub項目](https://github.com/adobe/aem-guides-wknd-mobile)。

如果您發現教程或代碼有問題，請留下 [GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## AEMGraphQL與內AEM容服務

|  | GraphQLAEMAPI | 內AEM容服務 |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | 組AEM件 |
| 內容 | 內容片段 | 組AEM件 |
| 內容發現 | 按GraphQL查詢 | 按頁AEM面 |
| 傳遞格式 | GraphQLJSON | 組AEM件導出器JSON |
