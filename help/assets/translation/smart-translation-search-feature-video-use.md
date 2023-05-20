---
title: 將智慧翻譯搜索與AEM Assets
description: 智慧翻譯搜索支援跨語言搜索和發現，可AEM以自動跨內容（包括資產和頁面）進行搜索和發現，支援50多種語言，並減少了手動內容翻譯的需要。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# 將智慧翻譯搜索與AEM Assets{#using-smart-translation-search-with-aem-assets}

智慧翻譯搜索支援跨語言搜索和發現，可AEM以自動跨內容（包括資產和頁面）進行搜索和發現，支援50多種語言，並減少了手動內容翻譯的需要。

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

智AEM能翻譯搜索允許用戶使用非英語詞AEM對內容執行搜索，以匹配其上具有相AEM同英文詞的資產。

智慧翻譯搜索是智慧標籤的完AEM美補充，智慧標籤以英文應用於資產。

此視頻假定 [智慧AEM翻譯搜索](smart-translation-search-technical-video-setup.md) 已設定。

## 智慧翻譯搜索的工作原理 {#how-smart-translation-search-works}

![智慧翻譯搜索流圖](assets/smart-translation-search-flow.png)

1. 用AEM戶執行全文搜索，提供本地化搜索詞(例如 西班牙語中「man」、「hombre」的詞)。
2. Apache Oak機器翻譯OSGi捆綁包提供的智慧翻譯搜索已投入使用，並評估所提供的搜索詞是否可以使用註冊的語言包進行翻譯。
3. 將收集步驟#2中的所有已翻譯術語，並在內部增強查詢，以將其作為搜索術語包含。 如果通常根據查找相關匹配的搜索索引進行評AEM估，則此增強的搜索項集。
4. 將收集與原始術語(「hombre」)或已翻譯術語(「man」)匹配的搜索結果，並將用戶作為搜索結果返回。

## 其他資源{#additional-resources}

* [與AEM Assets建立智慧翻譯搜索](smart-translation-search-technical-video-setup.md)
* [Apache Joshua語言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
