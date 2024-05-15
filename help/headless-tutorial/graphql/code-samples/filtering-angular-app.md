---
title: 篩選Angular應用程式
description: 此簡單Angular應用程式會篩選使用內容片段模組化的WKND Adventures。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 篩選Angular應用程式

探索AEM Headless GraphQL API使用 [angular](https://angular.io/) 應用程式。 此Angular應用程式會建立可依活動型別篩選的WKND Adventures清單。

此程式碼示範使用Adobe的 [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 從Angular叫用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有冒險，並衍生可用活動型別清單。 當使用者選擇活動型別時，選擇的型別傳遞給 `wknd-shared/adventures-by-activity` 持久查詢並擷取冒險詳細資料，只針對指定活動型別的那些冒險。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持久查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
