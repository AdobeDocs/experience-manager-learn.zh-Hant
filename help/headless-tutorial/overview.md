---
title: 無AEM頭教學課程
description: 一系列教學課程，說明如何將Adobe Experience Manager當做無頭CMS。
feature: '"內容片段、API"'
topic: 「無頭、內容管理」
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 4%

---


# 無AEM頭教學課程

Adobe Experience Manager有多種選項可定義無頭端點，並將其內容傳送為JSON。 使用實際操作教學課程來探索如何使用各種選項並選擇適合您的選項。

## GraphQL AEM API教學課程

>[!CAUTION]
>
> GraphQL AEM API for Content Fragments Delivery可應要求提供。
> 請洽詢Adobe支援，以啟用API做為您的AEMCloud Service方案。

內AEM容片段的GraphQL API
支援無頭CMS藍本，其中外部用戶端應用程式使用中管理的內容來呈現體AEM驗。

現代化的內容傳送API是Javascript前端應用程式效率與效能的關鍵。 使用REST API會帶來挑戰：

* 一次提取一個對象的大量請求
* 通常是「超量傳送」內容，這表示應用程式所接收的內容超出其需求

為克服這些挑戰，GraphQL提供了基於查詢的API，允許客戶僅查詢AEM所需的內容，並使用單個API調用接收。

* 在[GraphQL APIsAEM快速入門教程](./graphql/overview.md)中瞭解AEM如何使用GraphQL API

## Token型驗證教學課程

公AEM開各種HTTP端點，這些端點可以無頭方式互動，從GraphQL、內AEM容服務到資產HTTP API。 通常，這些無頭消費者可能需要驗證AEM才能存取受保護的內容或動作。 為方便使用，支AEM援來自外部應用程式、服務或系統的HTTP要求的Token型驗證。

* 從[](./authentication/overview.md)的「驗證AEM為外部應用程式教學課程的Cloud Service」中，瞭解如何透過HTTPAEM驗證為

## 內容AEM服務教學課程

Content ServicesAEM運用傳統AEM的頁面來構成無頭REST API端點，而元件則定義AEM或參考要在這些端點上公開的內容。

Content AEM Services允許使用與在AEM Sites製作網頁時相同的內容抽象化來定義這些HTTP API的內容和結構。 使用「頁AEM面」和「元AEM件」可讓行銷人員快速編寫和更新可支援任何應用程式的彈性JSON API。

* 在[開始使AEM用內容服務教學課程&lt;a1/AEM>中瞭解如何使用內容服務](./content-services/overview.md)

## GraphAEMQL與內AEM容服務

|  | GraphQL AEM API | 內AEM容服務 |
|--------------------------------|:-----------------|:---------------------|
| 架構定義 | 結構化內容片段模型 | 元AEM件 |
| 內容 | 內容片段 | 元AEM件 |
| 內容探索 | 按GraphQL查詢 | 依頁AEM面 |
| 傳送格式 | GraphQL JSON | AEM ComponentExporter JSON |

## 其他實用的教學課程

其他AEM無頭概念教學課程包括：

* [AEM SPA Editor and Angular 快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor and React 快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)