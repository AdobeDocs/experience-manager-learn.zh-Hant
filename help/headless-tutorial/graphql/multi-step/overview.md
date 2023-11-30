---
title: AEM Headless實作教學課程快速入門 — GraphQL
description: 端對端教學課程，說明如何使用AEM GraphQL API建置和公開內容。
doc-type: tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 5%

---

# AEM Headless - GraphQL 快速入門

{{aem-headless-trials-promo}}

端對端教學課程說明如何在Headless CMS案例中使用AEM GraphQL API建置和公開內容並由外部應用程式使用。

本教學課程探討如何使用AEM GraphQL API和Headless功能來增強外部應用程式中呈現的體驗。

本教學課程涵蓋下列主題：

* 建立專案設定
* 建立內容片段模型以模型化資料
* 根據先前建立的模型建立內容片段。
* 探索如何使用整合的GraphiQL開發工具來查詢AEM中的內容片段。
* 儲存或保留GraphQL查詢至AEM
* 使用範例React應用程式中的持續GraphQL查詢

## 先決條件 {#prerequisites}

依照本教學課程操作，須具備下列條件：

* 基本HTML和JavaScript技能
* 下列工具必須安裝在本機：
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE (例如， [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM環境

若要完成本教學課程，建議AEM管理員存取AEMas a Cloud Service環境。 如果您沒有AEMas a Cloud Service環境的存取權，可以使用 [本機AEMas a Cloud Service快速入門SDK](/help/cloud-service/local-development-environment/aem-runtime.md). 不過，請務必注意，某些產品UI畫面（例如內容片段導覽）不同。

## 讓我們開始吧！

開始教學課程使用 [定義內容片段模型](content-fragment-models.md).

## github專案

原始程式碼和內容套件可在 [AEM Guides - WKND GraphQL GitHub專案](https://github.com/adobe/aem-guides-wknd-graphql).

如果您發現教學課程或程式碼有問題，請保留 [GitHub問題](https://github.com/adobe/aem-guides-wknd-graphql/issues).
