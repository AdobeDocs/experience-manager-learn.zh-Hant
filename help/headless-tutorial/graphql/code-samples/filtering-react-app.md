---
title: 篩選React應用程式
description: 簡易的React應用程式，可篩選使用內容片段模組化的WKND Adventures。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# 篩選React應用程式

探索AEM Headless GraphQL API使用[React](https://reactjs.org/)應用程式篩選資料的能力。 此React應用程式會建立可按活動型別篩選的WKND Adventures清單。

此程式碼示範如何使用Adobe的[AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)從React叫用持續儲存的GraphQL查詢。 此應用程式使用`wknd-shared/adventures-all`持續查詢來收集所有冒險，並衍生可用活動型別清單。 當使用者選取活動型別時，選取的型別會傳遞到`wknd-shared/adventures-by-activity`持續查詢，並僅擷取指定活動型別的那些冒險的冒險詳細資料。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-activity`
