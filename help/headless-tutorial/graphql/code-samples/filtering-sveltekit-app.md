---
title: 簡單SvelteKit應用程式
description: 顯示使用內容片段模組化之WKND冒險的簡單SvelteKit應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# 篩選SvelteKit應用程式

探索AEM Headless GraphQL API使用[SvelteKit](https://kit.svelte.dev/)應用程式列出資料的能力。 此SvelteKit應用程式會建立WKND冒險清單，您可以選取這些冒險清單來顯示冒險的詳細資訊。

此程式碼示範使用Adobe的[AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md)從SvelteKit叫用持續的GraphQL查詢。 此應用程式使用`wknd-shared/adventures-all`持續查詢來收集所有冒險，並衍生可用活動型別清單。 冒險詳細資料是透過`wknd-shared/adventures-by-slug`持續查詢來要求。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-slug`
