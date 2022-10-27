---
title: 使用jQuery和Handlebars進行篩選
description: 使用jQuery和Handlebars的JavaScript實作，可篩選WKND歷程以顯示。.
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
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---


# 使用jQuery和Handlebars進行篩選

探索AEM無周邊GraphQL API使用以下功能的JavaScript應用程式來篩選資料： [jQuery](https://jquery.com/) 和 [手把](https://handlebarsjs.com/). 此應用程式會建立可依活動類型篩選的WKND歷險清單。

此程式碼會示範如何使用Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 調用持續GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有歷險，並衍生可用活動類型清單。 使用者選取活動類型時，選取的類型會傳遞至 `wknd-shared/adventures-by-activity` 持續查詢並僅擷取指定活動類型之歷險的探險詳細資料。

此程式碼：

+ 連線至AEM發佈服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
