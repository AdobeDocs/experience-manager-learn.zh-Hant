---
title: AEM Headless教學課程
description: 有關如何將Adobe Experience Manager用作無頭CMS的教學課程集。
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 5%

---


# AEM Headless教學課程

Adobe Experience Manager提供多種選項來定義無頭端點並將其內容傳送為JSON。 使用實際操作教學課程來探索如何使用各種選項並選擇適合您的選項。

## AEM GraphQL API教學課程

>[!CAUTION]
>
> 內容片段傳送的AEM GraphQL API可應要求提供。
> 請聯絡Adobe支援以啟用AEM雲端服務方案的API。

AEM的GraphQL內容片段API
支援無頭CMS藍本，其中外部用戶端應用程式使用AEM中管理的內容來呈現體驗。

現代化的內容傳送API是Javascript前端應用程式效率與效能的關鍵。 使用REST API會帶來挑戰：

* 一次提取一個對象的大量請求
* 通常是「超量傳送」內容，這表示應用程式所接收的內容超出其需求

為克服這些挑戰，GraphQL提供以查詢為基礎的API，讓客戶僅查詢AEM所需的內容，並使用單一API呼叫接收。

* 瞭解如何使用AEM的GraphQL API，請參閱[ AEM GraphQL APIs快速入門教學課程](./graphql/overview.md)

## AEM Content Services教學課程

AEM的Content Services運用傳統的AEM Pages來合成無頭REST API端點，而AEM Components會定義或參考要在這些端點上公開的內容。

AEM Content Services允許使用與在AEM Sites中製作網頁時相同的內容抽象化，來定義這些HTTP API的內容和結構。 使用AEM頁面和AEM元件可讓行銷人員快速合成和更新可支援任何應用程式的彈性JSON API。

* 瞭解如何使用AEM的Content Services，請參閱[AEM Content Services快速入門教學課程](./content-services/overview.md)

## AEM GraphQL與AEM Content Services比較

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