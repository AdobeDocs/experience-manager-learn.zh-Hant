---
title: Filtering Express app
description: A simple Express app that filters WKND adventures modeled using Content Fragments.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Filtering Express app

Explore AEM Headless GraphQL APIs ability to filter data using a [Express](https://expressjs.com/) and [Pug](https://pugjs.org/) app. This Express app creates a list of WKND adventures filterable by Activity Type.

This code demonstrates using Adobe&#39;s [AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) to invoke persisted GraphQL queries using Node.js-based JavaScript. 此應用程式使用`wknd-shared/adventures-all`持續查詢來收集所有冒險，並衍生可用活動型別清單。 當使用者選取活動型別時，選取的型別會傳遞到`wknd-shared/adventures-by-activity`持續查詢，並僅擷取指定活動型別的那些冒險的冒險詳細資料。 Adventure details are retrieved from AEM via the `wknd-shared/adventures-by-slug` persisted query.

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ Uses the WKND&#39;s persisted queries: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, and `wknd-shared/adventures-by-slug`
