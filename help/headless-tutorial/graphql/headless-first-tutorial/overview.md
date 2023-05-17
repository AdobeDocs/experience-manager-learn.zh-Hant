---
title: AEM Headless第一個教學課程
description: 了解如何成為AEM Headless的第一個應用程式。
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


# AEM Headless第一個教學課程

![AEM Headless第一個教學課程](./assets/overview/overview.png)

歡迎使用完全由AEM無頭API和GraphQL支援的React來建立網頁體驗的教學課程。 在本教學課程中，我們將引導您結合React、Adobe Experience Manager(AEM)無頭API和GraphQL的強大功能，逐步引導您建立動態互動式網頁應用程式。

React是建置使用者介面的熱門JavaScript程式庫，以其簡單性、可重複使用性和元件型架構著稱。 AEM提供強大的內容管理功能，並公開無頭式API，讓開發人員可透過各種管道和應用程式存取AEM中儲存的內容和資料。

透過運用AEM無頭API，您可以從AEM例項擷取內容、資產和資料，並使用它們為React應用程式提供動力。 GraphQL是API的彈性查詢語言，可提供有效且精確的方式，從AEM例項要求特定資料，使React與AEM之間可順暢整合。

在本教學課程中，我們將逐步引導您完成使用React和AEM Headless API搭配GraphQL來建立網頁體驗的程式。 您將學習如何設定開發環境、建立React與AEM之間的連線、使用GraphQL查詢擷取內容，以及在網頁應用程式中動態呈現內容。

我們將討論相關主題，例如設定您的React專案、使用AEM建立驗證、使用GraphQL從AEM查詢內容、處理React元件中的資料，以及運用快取和分頁來最佳化效能。

在本教學課程結束前，您將能深入了解如何運用React、AEM Headless API和GraphQL，打造強大且吸引人的網頁體驗。 因此，讓我們開始構建下一個Web應用程式！

## 必備條件

### 技能

+ 反應能力
+ GraphQL熟練程度
+ AEMas a Cloud Service基本知識

### AEM as a Cloud Service 

本教學課程需要管理員存取AEMas a Cloud Service環境。

### 軟體

+ [Node.js v16+](https://nodejs.org/en/)
   + 執行以檢查節點版本 `node -v` 從命令行
+ [npm 6+](https://www.npmjs.com/)
   + 執行以檢查npm版本 `npm -v` 從命令行
+ [Git](https://git-scm.com/)
   + 執行以檢查您的Git版本 `git -v` 從命令行

使用 [節點版本管理器(nvm)](https://github.com/nvm-sh/nvm) 要解決同一台電腦上有多個版本的node.js。

確保您有權在電腦上全局安裝軟體。

## 下一步

現在環境已設定完畢，接下來的步驟如下： [在AEM as a Cloud Service中設定及製作內容](./1-content-modeling.md)
