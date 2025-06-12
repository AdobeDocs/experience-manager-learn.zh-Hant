---
title: AEM Headless 首次教學課程
description: 了解如何成為 AEM Headless 的第一個應用程式。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# AEM Headless 首次教學課程

歡迎來到使用 React 建置網頁體驗的教學課程，此教學課程完全由 AEM Headless API 和 GraphQL 支援。在本教學課程中，我們會引導您結合 React、Adobe Experience Manager (AEM) Headless API 和 GraphQL 的強大功能，建立動態互動式網頁應用程式。

React 是一個用來建置使用者介面的常見 JavaScript 程式庫，以其簡單性、可重複使用性和元件型的架構聞名。AEM 提供強大的內容管理功能，並公開無周邊 API，讓開發人員能夠透過各種管道和應用程式存取儲存在 AEM 中的內容和資料。

運用 AEM Headless API，您可以從 AEM 實例中擷取內容、資產和資料，並用來支援您的 React 應用程式。GraphQL 是一項彈性的 API 查詢語言，讓您可以有效並精準地向 AEM 實例要求特定資料，使 React 和 AEM 可以緊密整合。

![AEM Headless 首次教學課程](./assets/overview/overview.png)

在本教學課程中，我們會向您逐步解說使用 React 和 AEM Headless API 搭配 GraphQL 建置網頁體驗的流程。您會學習到如何建立開發環境、在 React 和 AEM 之間建立連線、使用 GraphQL 查詢擷取內容，以及在網頁應用程式中動態轉譯內容。

我們會介紹設定 React 專案、使用 AEM 建立驗證、使用 GraphQL 向 AEM 查詢內容、處理 React 元件中的資料，以及利用快取和分頁將效能最佳化等主題。

在本教學課程結束時，對於如何利用 React、AEM Headless API 和 GraphQL 建立強大且引人入勝之網頁體驗，您會有深入的了解。那麼，讓我們開始建置您的下一個網頁應用程式！

## 先決條件

### 技能

+ 精通 React
+ 精通 GraphQL
+ AEM as a Cloud Service 基礎知識

### AEM as a Cloud Service

本教學課程需要 AEM as a Cloud Service 環境的管理員存取權。

### 軟體

+ [Node.js v16+](https://nodejs.org/en/)
   + 從命令列執行 `node -v`，確認您的節點版本
+ [npm 6+](https://www.npmjs.com/)
   + 從命令列執行 `npm -v`，確認您的 npm 版本
+ [Git](https://git-scm.com/)
   + 從命令列執行 `git -v`，確認您的 git 版本

使用[節點版本管理器 (nvm)](https://github.com/nvm-sh/nvm) 解決同一台機器上的 node.js 存在多個版本的問題。

確保您有在電腦上全域安裝軟體的權限。

## 下一步

現在，您的環境已設定完畢，讓我們繼續下一步：[在 AEM as a Cloud Service 中設定和製作內容](./1-content-modeling.md)
