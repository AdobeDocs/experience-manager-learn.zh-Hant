---
title: 反應元件
description: 顯示內容片段和參考影像資產的範例React元件。
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '55'
ht-degree: 0%

---


# 反應元件

此React元件會取用單一WKND探險內容片段，並將其內容顯示為促銷橫幅。

此程式碼：

+ 連接到 [wknd.site](https://wknd.site)的AEM Publish服務，且不需要驗證
+ 使用持續查詢： `wknd-shared/adventures-by-slug`
