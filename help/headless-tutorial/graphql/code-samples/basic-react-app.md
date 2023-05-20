---
title: 基本反應應用
description: 一個基本的React應用，顯示WKND冒險及其詳細資訊清單
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---

# 基本反應應用

此 [反應](https://reactjs.org/) app演示了如何使用GraphQLAPIAEM使用永續查詢來查詢內容。 此應用程式將可過濾WKND冒險項，在選擇冒險項時，將顯示冒險項的完整詳細資訊。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

要更深入地查看此Next.js應用是如何構建的，請查看 [示例React應用文檔](../example-apps/react-app.md)。
