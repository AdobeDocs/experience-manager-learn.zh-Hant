---
title: 基本React應用程式
description: 基本的React應用程式，可顯示WKND冒險清單及其詳細資訊
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 21
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# 基本React應用程式

這個 [React](https://reactjs.org/) 應用程式示範如何使用AEM GraphQL API透過持續性查詢來查詢內容。 此應用程式會呈現WKND Adventures的可篩選內容，並在選取冒險後顯示該冒險的完整詳細資訊。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持久查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

如需如何建立此Next.js應用程式的深入檢視，請檢閱 [React應用程式檔案範例](../example-apps/react-app.md).
