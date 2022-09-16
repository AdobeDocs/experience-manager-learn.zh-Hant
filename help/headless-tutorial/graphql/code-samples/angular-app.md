---
title: Angular應用程式
description: 顯示使用內容片段模型化WKND歷險的簡單Angular應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '69'
ht-degree: 0%

---


# Angular應用程式

使用簡單的Angular應用程式，探索AEM無周邊GraphQL API。 此Angular應用程式連線至 [wknd.site](https://wknd.site)，顯示WKND歷險清單，並提供每個歷險的詳細資訊檢視。

此程式碼：

+ 連接到 [wknd.site](https://wknd.site)的AEM Publish服務，且不需要驗證
+ 使用持續的查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

