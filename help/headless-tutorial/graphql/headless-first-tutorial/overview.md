---
title: AEM無頭第一教程
description: 瞭解如何成為無頭AEM的第一個應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 3%

---


# AEM無頭第一教程

![無AEM頭第一教程](./assets/overview/overview.png)

歡迎學習有關使用React構建Web體驗的教程，該教程完全由AEMHeadless API和GraphQL支援。 在本教程中，我們將通過結合React、Adobe Experience Manager(AEM)無頭API和GraphQL的功能，指導您完成建立動態互動式Web應用程式的過程。

React是一個用於構建用戶介面的常用JavaScript庫，以其簡單性、可重用性和基於元件的體系結構而聞名。 提AEM供了強大的內容管理功能，並公開了無頭API，使開發人員能夠通過各種渠道和應用程AEM序訪問儲存在中的內容和資料。

通過利AEM用無頭API，您可以從實例中檢索內容、資產和數AEM據，並使用它們為React應用程式提供電源。 GraphQL是API的靈活查詢語言，它提供了一種高效、精確的方法來從實例請求特定資料AEM，從而實現了React和React之間的無縫AEM整合。

在本教程中，我們將逐步向您介紹如何使用React和Headless API與GraphQL一起構建Web體AEM驗的過程。 您將學習如何設定開發環境、在React和之間建立連接、使用AEMGraphQL查詢檢索內容，以及在Web應用程式中動態呈現內容。

我們將介紹以下主題：配置您的React項目、使用建立身份驗證、使用AEMGraphQL查詢內AEM容、處理React元件中的資料，以及利用快取和分頁優化效能。

在本教程的結束時，您將對如何利用React 、 AEM Headless API和GraphQL構建強大且引人入勝的Web體驗有深入的瞭解。 所以，讓我們深入瞭解並開始構建您的下一個Web應用程式！

## 必備條件

### 技能

+ 反應能力
+ 熟練的GraphQL
+ as a Cloud Service基礎知AEM識

### AEM as a Cloud Service 

本教程要求管理員訪問AEMas a Cloud Service環境。

### 軟體

+ [Node.js v16+](https://nodejs.org/en/)
   + 通過運行來檢查節點版本 `node -v` 從命令行
+ [npm 6+](https://www.npmjs.com/)
   + 通過運行檢查npm版本 `npm -v` 從命令行
+ [Git](https://git-scm.com/)
   + 通過運行來檢查Git版本 `git -v` 從命令行

使用 [節點版本管理器(nvm)](https://github.com/nvm-sh/nvm) 在同一台電腦上具有多個版本的node.js的地址。

確保您有權在電腦上全局安裝軟體。

## 下一步

現在您的環境已設定完畢，讓我們繼續下一步： [設定和創作AEMas a Cloud Service](./1-content-modeling.md)
