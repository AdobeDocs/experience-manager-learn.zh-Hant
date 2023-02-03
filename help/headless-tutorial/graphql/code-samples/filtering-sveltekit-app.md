---
title: 簡單SvelteKit應用程式
description: 簡單的SvelteKit應用程式，可顯示使用內容片段模型化的WKND歷險。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# 篩選SvelteKit應用程式

探索AEM Headless GraphQL API使用 [SvelteKit](https://kit.svelte.dev/) 應用程式。 此SvelteKit應用程式會建立WKND歷險清單，您可以選取該清單以顯示歷險的詳細資訊。

此程式碼會示範如何使用Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 從SvelteKit叫用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有歷險，並衍生可用活動類型清單。 探險詳細資訊需透過 `wknd-shared/adventures-by-slug` 持續查詢。

此程式碼：

+ 連線至AEM發佈服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
