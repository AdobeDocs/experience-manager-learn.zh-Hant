---
title: 篩選Vue應用
description: 一個簡單的Vue應用，它過濾使用內容片段建模的WKND冒險。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# 篩選Vue應用

探AEM索無頭GraphQLAPI使用 [武埃](https://vuejs.org/) 的子菜單。 此React應用建立可按活動類型篩選的WKND冒險清單。

此代碼演示使用Adobe [用AEM於JavaScript的無頭客戶端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 調用Vue的永續GraphQL查詢。 此應用使用 `wknd-shared/adventures-all` 永續查詢以收集所有冒險，並導出可用活動類型清單。 當用戶選擇「活動類型」時，所選類型將傳遞給 `wknd-shared/adventures-by-activity` 永續查詢並僅檢索指定活動類型的冒險的冒險詳細資訊。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
