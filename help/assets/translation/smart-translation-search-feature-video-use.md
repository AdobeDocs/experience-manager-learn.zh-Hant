---
title: 搭配AEM Assets使用智慧翻譯搜尋
description: 智慧翻譯搜尋可讓跨語言搜尋和探索在AEM內容（包括資產和頁面）上自動進行，支援超過50種語言，並減少手動內容翻譯的需求。
version: 6.3, 6.4, 6.5
feature: 搜尋
topic: 內容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# 搭配AEM Assets使用智慧翻譯搜尋{#using-smart-translation-search-with-aem-assets}

智慧翻譯搜尋可讓跨語言搜尋和探索在AEM內容（包括資產和頁面）上自動進行，支援超過50種語言，並減少手動內容翻譯的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM智慧型翻譯搜尋可讓使用者使用非英文辭彙來搜尋AEM中的內容，以比對AEM中具有相等英文辭彙的資產。

智慧翻譯搜尋是套用至英文資產的AEM智慧標籤的完美補助。

此視訊假設已設定[AEM智慧翻譯搜尋](smart-translation-search-technical-video-setup.md)。

## 智慧翻譯搜尋如何運作{#how-smart-translation-search-works}

![智慧翻譯搜尋流程圖](assets/smart-translation-search-flow.png)

1. AEM使用者執行全文搜尋，提供本地化的搜尋詞(例如 「男」、「男」的西班牙語詞)。
2. Apache Oak Machine Translation OSGi套件組合提供的智慧翻譯搜尋功能會參與並評估所提供的搜尋辭彙是否可使用註冊的語言套件進行翻譯。
3. 系統會收集步驟#2中所有翻譯的詞語，並在內部擴充查詢，將其納入為搜尋詞。 這個增強的搜尋詞集(如果評估時通常是根據AEM搜尋索引找到相關符合)。
4. 將收集與原始詞語(「hombre」)或翻譯詞語(「man」)匹配的搜索結果，並將用戶作為搜索結果返回。

## 其他資源{#additional-resources}

* [使用AEM Assets設定智慧翻譯搜尋](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)