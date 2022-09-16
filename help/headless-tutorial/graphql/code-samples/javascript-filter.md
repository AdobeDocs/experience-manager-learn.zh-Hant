---
title: 篩選查詢
description: 一種JavaScript實作，可供選取特定內容片段，然後顯示其詳細資料。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '57'
ht-degree: 0%

---


# 篩選查詢

此JavaScript和Handlebars範例說明如何篩選GraphQL結果，以及顯示選取的結果。

此程式碼：

+ 連接到 [wknd.site](https://wknd.site)的AEM Publish服務，且不需要驗證
+ 使用持續的查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
