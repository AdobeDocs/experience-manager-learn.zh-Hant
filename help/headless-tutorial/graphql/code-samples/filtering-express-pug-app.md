---
title: 篩選Express應用程式
description: 簡單的Express應用程式，可篩選使用內容片段模型化的WKND歷險。
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
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# 篩選Express應用程式

探索AEM無頭式GraphQL API使用 [Express](https://expressjs.com/) 和 [普格](https://pugjs.org/) 應用程式。 此Express應用程式會建立可依活動類型篩選的WKND歷險清單。

此程式碼會示範如何使用Adobe [AEM NodeJS的無頭式用戶端](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用Node.js型JavaScript叫用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有歷險，並衍生可用活動類型清單。 使用者選取活動類型時，選取的類型會傳遞至 `wknd-shared/adventures-by-activity` 持續查詢並僅擷取指定活動類型之歷險的探險詳細資料。 探險詳細資料會透過 `wknd-shared/adventures-by-slug` 持續查詢。

此程式碼：

+ 連線至AEM發佈服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`，和 `wknd-shared/adventures-by-slug`
