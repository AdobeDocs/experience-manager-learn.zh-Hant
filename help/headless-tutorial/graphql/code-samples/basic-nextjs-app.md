---
title: 基本Next.js應用程式
description: 基本的Next.js應用程式，可顯示WKND歷險及其詳細資訊清單
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---


# 基本Next.js應用程式

此 [Next.js](https://nextjs.org/) 應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。 此應用程式可呈現WKND歷險的可篩選資料，並在選取歷險時，顯示歷險的完整詳細資訊。

此程式碼：

+ 連線至AEM發佈服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

如需深入檢視此Next.js應用程式的建置方式，請檢閱 [範例Next.js應用程式檔案](../example-apps/next-js.md).
