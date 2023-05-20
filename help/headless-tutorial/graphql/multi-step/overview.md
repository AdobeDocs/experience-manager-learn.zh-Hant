---
title: 無頭實AEM踐教程入門 — GraphQL
description: 一個端到端教程，演示如何使用GraphQLAPI構建和公開AEM內容。
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
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 5%

---

# AEM Headless - GraphQL 快速入門

一個端到端教程，演示如何在無頭CMS方案中使用AEMGraphQLAPI構建和公開內容，並讓外部應用使用這些內容。

本教程探AEM討如何使用GraphQLAPI和無頭功能為外部應用中出現的體驗提供動力。

本教程介紹以下主題：

* 建立項目配置
* 建立內容片段模型以對資料建模
* 基於先前製作的模型建立內容片段。
* 瞭解如何使用整合AEM的GraphiQL開發工具查詢中的內容片段。
* 儲存或保留GraphQL查詢AEM
* 從示例React應用中使用永續GraphQL查詢

## 必備條件 {#prerequisites}

以下內容需要學習本教程：

* 基本HTML和JavaScript技能
* 必須本地安裝以下工具：
   * [節點.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE(例如， [Microsoft® Visual Studio代碼](https://code.visualstudio.com/))

### 環AEM境

要完成本教程，建AEM議管理員訪AEM問as a Cloud Service環境。 如果您沒有訪問AEMas a Cloud Service環境的權限， [本地AEMas a Cloud Service快速啟動SDK](/help/cloud-service/local-development-environment/aem-runtime.md)。 但是，必須注意的是，某些產品UI螢幕（如內容片段導航）不同。

## 開始吧！

開始教程 [定義內容片段模型](content-fragment-models.md)。

## GitHub項目

原始碼和內容包在 [指AEM南 — WKNDGraphQLGitHub項目](https://github.com/adobe/aem-guides-wknd-graphql)。

如果您發現教程或代碼有問題，請保留 [GitHub問題](https://github.com/adobe/aem-guides-wknd-graphql/issues)。
