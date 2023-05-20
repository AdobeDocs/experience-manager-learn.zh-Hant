---
title: 使用jQuery和Handlebar進行篩選
description: 使用篩選WKND冒險項以顯示的jQuery和Handlebar的JavaScript實現。。
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
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 使用jQuery和Handlebar進行篩選

探AEM索無頭GraphQLAPI使用使用JavaScript應用篩選資料的功能 [j查詢](https://jquery.com/) 和 [車把](https://handlebarsjs.com/)。 此應用建立可按活動類型篩選的WKND冒險清單。

此代碼演示使用Adobe [用AEM於JavaScript的無頭客戶端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 調用永續的GraphQL查詢。 此應用使用 `wknd-shared/adventures-all` 永續查詢以收集所有冒險，並導出可用活動類型清單。 當用戶選擇「活動類型」時，所選類型將傳遞給 `wknd-shared/adventures-by-activity` 永續查詢並僅檢索指定活動類型的冒險的冒險詳細資訊。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
