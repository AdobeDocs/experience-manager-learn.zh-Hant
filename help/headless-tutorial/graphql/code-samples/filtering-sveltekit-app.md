---
title: 簡單的SvelteKit應用
description: 一個簡單的SvelteKit應用程式，它顯示使用內容片段建模的WKND冒險。
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
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# 篩選SvelteKit應用

探AEM索無頭GraphQLAPI使用 [斯韋爾特基特](https://kit.svelte.dev/) 的子菜單。 此SvelteKit應用建立WKND冒險清單，可以選擇該清單來顯示冒險的詳細資訊。

此代碼演示使用Adobe [用AEM於JavaScript的無頭客戶端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 調用SvelteKit中的永續GraphQL查詢。 此應用使用 `wknd-shared/adventures-all` 永續查詢以收集所有冒險，並導出可用活動類型清單。 探險詳細資訊可通過 `wknd-shared/adventures-by-slug` 永續查詢。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
