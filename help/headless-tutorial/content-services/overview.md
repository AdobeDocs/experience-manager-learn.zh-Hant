---
title: AEM Headless快速入門 — 內容服務
description: 端對端的教學課程，說明如何使用 AEM Headless 來建立和公開內容。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 4%

---


# AEM Headless快速入門 — 內容服務

AEM內容服務運用傳統AEM頁面來撰寫無頭REST API端點，並定義或參照要在這些端點上公開的內容。

AEM Content Services允許使用與在AEM Sites中製作網頁時所用的相同內容抽象化功能，來定義這些HTTP API的內容和結構。 使用AEM頁面和AEM元件，行銷人員就能快速撰寫和更新彈性的JSON API，為任何應用程式提供強大功能。

## 內容服務教學課程

端對端教學課程，說明如何在無頭CMS情境中，使用AEM建置和公開原生行動應用程式使用的內容。

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

本教學課程探討如何使用AEM Content Services來強化顯示事件資訊（音樂、效能、藝術等）的行動應用程式體驗 由WKND團隊策劃。

本教學課程將涵蓋下列主題：

* 使用內容片段建立代表事件的內容
* 使用將事件資料公開為JSON的AEM Sites範本和頁面，定義AEM Content Services端點
* 探索如何使用AEM WCM核心元件來讓行銷人員編寫JSON結尾點
* 從行動應用程式使用AEM Content Services JSON
   * 使用Android是因為其有跨平台模擬器，本教學課程的所有使用者（Windows、macOS和Linux）都可用來執行原生應用程式。

## GitHub專案

原始碼和內容套件可在[AEM指南 — WKND Mobile GitHub專案](https://github.com/adobe/aem-guides-wknd-mobile)中取得。

若您發現教學課程或程式碼的問題，請保留[ GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## AEM GraphQL與AEM內容服務的比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 按GraphQL查詢 | 依AEM頁面 |
| 傳送格式 | GraphQL JSON | AEM ComponentExporter JSON |
