---
title: 篩選值應用程式
description: 一種簡單的值應用程式，可篩選使用內容片段模組化的WKND Adventures。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
duration: 42
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 篩選值應用程式

探索AEM Headless GraphQL API使用 [值](https://vuejs.org/) 應用程式。 此React應用程式會建立可按活動型別篩選的WKND Adventures清單。

此程式碼示範使用Adobe的 [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 從Vue叫用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有冒險，並衍生可用活動型別清單。 當使用者選擇活動型別時，選擇的型別傳遞給 `wknd-shared/adventures-by-activity` 持久查詢並擷取冒險詳細資料，只針對指定活動型別的那些冒險。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持久查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
