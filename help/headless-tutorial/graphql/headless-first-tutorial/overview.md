---
title: AEM Headless第一個教學課程
description: 瞭解如何成為AEM Headless的第一個應用程式。
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
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 2%

---

# AEM Headless第一個教學課程

歡迎參加有關使用React建置網頁體驗的教學課程，這套教學課程由AEM Headless API和GraphQL提供完整支援。 在本教學課程中，我們將結合React、Adobe Experience Manager (AEM) Headless API和GraphQL的強大功能，引導您完成建立動態和互動式Web應用程式的程式。

React是用於建置使用者介面的熱門JavaScript程式庫，以其簡易性、可重複使用性和元件型架構而聞名。 AEM提供強大的內容管理功能並公開Headless API，好讓開發人員透過各種管道和應用程式存取儲存在AEM中的內容和資料。

善用AEM Headless API，您可以從AEM執行個體擷取內容、資產和資料，並使用它們來強化React應用程式。 GraphQL是一種彈性的API查詢語言，可讓您以有效率且精確的方式向AEM執行個體要求特定資料，實現React與AEM之間的順暢整合。

![AEM Headless第一個教學課程](./assets/overview/overview.png)

在本教學課程中，我們將逐步引導您完成使用React和AEM Headless API搭配GraphQL來建置Web體驗的程式。 您將瞭解如何設定開發環境、在React和AEM之間建立連線、使用GraphQL查詢擷取內容，以及在您的網頁應用程式中動態呈現。

我們將介紹設定React專案、使用AEM建立驗證、使用GraphQL從AEM查詢內容、處理React元件中的資料，以及利用快取和分頁來最佳化效能等主題。

在本教學課程結束時，您將會充分瞭解如何運用React、AEM Headless API和GraphQL來建立強大且吸引人的Web體驗。 那麼，讓我們開始深入瞭解並開始建置您的下一個網頁應用程式！

## 先決條件

### 技能

+ 熟練React
+ 熟練掌握GraphQL
+ AEM as a Cloud Service的基本知識

### AEM as a Cloud Service 

本教學課程需要管理員存取AEM as a Cloud Service環境。

### 軟體

+ [Node.js v16+](https://nodejs.org/en/)
   + 從命令列執行`node -v`以檢查您的節點版本
+ [npm 6+](https://www.npmjs.com/)
   + 從命令列執行`npm -v`以檢查您的npm版本
+ [Git](https://git-scm.com/)
   + 從命令列執行`git -v`以檢查您的Git版本

使用[節點版本管理員(nvm)](https://github.com/nvm-sh/nvm)來位址在同一部電腦上有多個node.js版本。

確定您擁有在電腦上全域安裝軟體的許可權。

## 下一步

現在您的環境已設定完畢，接著進行下一個步驟：[在AEM as a Cloud Service中設定及撰寫內容](./1-content-modeling.md)
