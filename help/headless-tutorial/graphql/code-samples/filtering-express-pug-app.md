---
title: 篩選快速應用程式
description: 簡易的快速應用程式，可篩選使用內容片段模組化的WKND Adventures。
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 篩選快速應用程式

探索AEM Headless GraphQL API使用[Express](https://expressjs.com/)和[Pug](https://pugjs.org/)應用程式篩選資料的能力。 此Express應用程式會建立可依活動型別篩選的WKND Adventures清單。

此程式碼示範使用Adobe的[適用於NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs)的AEM Headless使用者端，以使用以Node.js為基礎的JavaScript叫用持續的GraphQL查詢。 此應用程式使用`wknd-shared/adventures-all`持續查詢來收集所有冒險，並衍生可用活動型別清單。 當使用者選取活動型別時，選取的型別會傳遞到`wknd-shared/adventures-by-activity`持續查詢，並僅擷取指定活動型別的那些冒險的冒險詳細資料。 冒險詳細資料是透過`wknd-shared/adventures-by-slug`持續查詢從AEM擷取的。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`、`wknd-shared/adventures-by-activity`和`wknd-shared/adventures-by-slug`
