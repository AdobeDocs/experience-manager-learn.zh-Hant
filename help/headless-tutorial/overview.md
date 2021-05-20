---
title: AEM Headless教學課程
description: 有關如何使用Adobe Experience Manager as a Headless CMS的教學課程系列。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# AEM Headless教學課程

Adobe Experience Manager有多個選項可定義無頭端點並將其內容傳送為JSON。 使用實作教學課程，探索如何使用各種選項，並選擇適合您的選項。

## AEM GraphQL API教學課程

AEM GraphQL API（適用於內容片段）
支援無頭式CMS案例，外部用戶端應用程式會使用AEM中管理的內容來呈現體驗。

現代化的內容傳送API是基於Javascript前端應用程式效率與效能的關鍵。 使用REST API會帶來下列挑戰：

* 一次擷取一個物件的請求數量很多
* 通常是「過度傳送」內容，這表示應用程式的接收量超出其需求

為克服這些難題，GraphQL提供了基於查詢的API，允許客戶僅查詢AEM所需的內容，並使用單一API呼叫接收。

* 在[AEM GraphQL API快速入門教學課程](./graphql/overview.md)中，了解如何使用AEM GraphQL API

## Token式驗證教學課程

AEM會公開各種HTTP端點，可以無周邊方式互動，從GraphQL、AEM Content Services到Assets HTTP API。 這些無頭消費者通常需要向AEM驗證，才能存取受保護的內容或動作。 為方便執行此作業，AEM支援來自外部應用程式、服務或系統之HTTP要求的權杖式驗證。

* 了解如何使用[從外部應用程式教學課程](./authentication/overview.md)驗證AEM作為Cloud Service中的存取權杖，透過HTTP驗證AEM

## AEM Content Services教學課程

AEM內容服務運用傳統AEM頁面來撰寫無頭REST API端點，並定義或參照要在這些端點上公開的內容。

AEM Content Services允許使用與在AEM Sites中製作網頁時所用的相同內容抽象化功能，來定義這些HTTP API的內容和結構。 使用AEM頁面和AEM元件，行銷人員就能快速撰寫和更新彈性的JSON API，為任何應用程式提供強大功能。

* [AEM內容服務快速入門教學課程](./content-services/overview.md)中的如何使用AEM內容服務

## AEM GraphQL與AEM內容服務的比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | AEM元件 |
| 內容 | 內容片段 | AEM元件 |
| 內容探索 | 按GraphQL查詢 | 依AEM頁面 |
| 傳送格式 | GraphQL JSON | AEM ComponentExporter JSON |

## 其他實用的教學課程

其他與無頭概念相關的AEM教學課程包括：

* [AEM SPA Editor and Angular 快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor and React 快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)