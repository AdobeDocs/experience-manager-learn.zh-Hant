---
title: 篩選預設應用程式
description: 簡單的預作應用程式，可篩選使用內容片段模型化的WKND歷險。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# 篩選預設應用程式

探索AEM無周邊GraphQL API使用 [Preact](https://preactjs.com/) 應用程式。 此Preact應用程式會建立可依活動類型篩選的WKND歷險清單。

此程式碼會示範如何使用Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 從React調用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有歷險，並衍生可用活動類型清單。 使用者選取活動類型時，選取的類型會傳遞至 `wknd-shared/adventures-by-activity` 持續查詢並僅擷取指定活動類型之歷險的探險詳細資料。

此程式碼：

+ 連線至AEM發佈服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`