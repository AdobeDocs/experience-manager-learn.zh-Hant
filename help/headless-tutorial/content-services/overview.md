---
title: AEM Headless — 內容服務快速入門
description: 端對端的教學課程，說明如何使用 AEM Headless 來建立和公開內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 333
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 4%

---

# AEM Headless — 內容服務快速入門

AEM Content Services運用傳統AEM頁面來撰寫Headless REST API端點，而AEM元件會定義或參考要在這些端點上公開的內容。

AEM Content Services允許在AEM Sites中製作網頁時使用相同的內容抽象化，以定義這些HTTP API的內容和結構。 使用AEM Pages和AEM Components可讓行銷人員快速撰寫和更新可支援任何應用程式的彈性JSON API。

## Content Service教學課程

端對端教學課程說明如何在Headless CMS案例中使用AEM建立和公開內容，以及原生行動應用程式所使用。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

本教學課程探索AEM Content Services如何用來強化顯示事件資訊（音樂、效能、藝術等）的行動應用程式體驗 由WKND團隊所策劃。

本教學課程將涵蓋下列主題：

* 使用內容片段建立代表事件的內容
* 使用AEM Sites的範本和公開事件資料為JSON的頁面來定義AEM Content Services端點
* 探索如何使用AEM WCM核心元件來讓行銷人員編寫JSON端點
* 使用行動應用程式中的AEM Content Services JSON
   * 使用Android是因為其擁有跨平台模擬器，本教學課程的所有使用者(Windows、macOS和Linux)都可使用它來執行原生應用程式。

## github專案

原始程式碼和內容套件可在 [AEM Guides - WKND Mobile GitHub專案](https://github.com/adobe/aem-guides-wknd-mobile).

若教學課程或程式碼有任何問題，請留下 [GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL與AEM Content Services

|                                | AEM GRAPHQL API | AEM內容服務 |
|--------------------------------|:-----------------|:---------------------|
| 結構描述定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 依GraphQL查詢 | 依AEM頁面 |
| 傳遞格式 | GRAPHQL JSON | AEM ComponentExporter JSON |
