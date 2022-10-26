---
title: AEM無頭式實作教學課程快速入門 — GraphQL
description: 端對端教學課程，說明如何使用AEM GraphQL API來建置和公開內容。
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 3%

---

# AEM無周邊功能快速入門 — GraphQL

端對端教學課程，說明如何在無周邊CMS情境下，使用AEM GraphQL API建置和公開內容，並供外部應用程式使用。

本教學課程探討如何使用AEM GraphQL API和無周邊功能，強化外部應用程式中呈現的體驗。

本教學課程涵蓋下列主題：

* 建立專案設定
* 建立內容片段模型以建立資料模型
* 根據先前建立的模型建立內容片段。
* 探索如何使用整合的GraphiQL開發工具查詢AEM中的內容片段。
* 若要儲存或保留GraphQL查詢至AEM
* 從範例React應用程式中使用持續的GraphQL查詢


## 必備條件 {#prerequisites}

請遵循本教學課程，並遵循下列步驟：

* 基本HTML和JavaScript技能
* 必須在本機安裝下列工具：
   * [Node.js v14+](https://nodejs.org/en/)
   * [npm 6+](https://www.npmjs.com/)
   * [Git](https://git-scm.com/)
   * IDE(例如 [Microsoft® Visual Studio代碼](https://code.visualstudio.com/))

### AEM環境

若要完成本教學課程，建議AEM管理員存取AEMas a Cloud Service環境。 如果您沒有AEMas a Cloud Service環境的存取權，可以使用 [本機AEMas a Cloud ServiceQuickstart SDK](/help/cloud-service/local-development-environment/aem-runtime.md). 不過，請務必注意，某些產品UI畫面（例如「內容片段」導覽）不同。

## 開始吧！

以開始本教學課程 [定義內容片段模型](content-fragment-models.md).

## GitHub專案

原始碼和內容套件可在 [AEM指南 — WKND GraphQL GitHub專案](https://github.com/adobe/aem-guides-wknd-graphql).

若您發現本教學課程或程式碼的問題，請保留 [GitHub問題](https://github.com/adobe/aem-guides-wknd-graphql/issues).
