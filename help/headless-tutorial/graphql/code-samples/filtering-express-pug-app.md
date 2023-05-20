---
title: 篩選Express應用
description: 一個簡單的Express應用，它過濾使用內容片段建模的WKND冒險。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 篩選Express應用

探AEM索無頭GraphQLAPI使用 [快](https://expressjs.com/) 和 [普格](https://pugjs.org/) 的子菜單。 此Express應用建立可按活動類型篩選的WKND冒險清單。

此代碼演示使用Adobe [用AEM於NodeJS的無頭客戶端](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用基於Node.js的JavaScript調用永續的GraphQL查詢。 此應用使用 `wknd-shared/adventures-all` 永續查詢以收集所有冒險，並導出可用活動類型清單。 當用戶選擇「活動類型」時，所選類型將傳遞給 `wknd-shared/adventures-by-activity` 永續查詢並僅檢索指定活動類型的冒險的冒險詳細資訊。 從中檢索冒險詳AEM細資訊 `wknd-shared/adventures-by-slug` 永續查詢。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all`。 `wknd-shared/adventures-by-activity`, `wknd-shared/adventures-by-slug`
