---
title: 搭配AEM資產使用智慧型翻譯搜尋
seo-title: 搭配AEM資產使用智慧型翻譯搜尋
description: 智慧型轉譯搜尋功能可自動跨AEM內容（包括資產和頁面）進行跨語言搜尋和搜尋，支援超過50種語言，並減少手動內容轉譯的需求。
seo-description: 智慧型轉譯搜尋功能可自動跨AEM內容（包括資產和頁面）進行跨語言搜尋和搜尋，支援超過50種語言，並減少手動內容轉譯的需求。
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# 搭配AEM資產使用智慧型翻譯搜尋{#using-smart-translation-search-with-aem-assets}

智慧型轉譯搜尋功能可自動跨AEM內容（包括資產和頁面）進行跨語言搜尋和搜尋，支援超過50種語言，並減少手動內容轉譯的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM智慧型翻譯搜尋可讓使用者使用非英文詞語來搜尋AEM中的內容，以比對AEM中包含相同英文詞語的資產。

智慧型翻譯搜尋是套用英文資產的AEM智慧型標籤的完美輔助。

此影片假設 [已設定AEM智慧型轉譯搜尋](smart-translation-search-technical-video-setup.md) 。

## 智慧型翻譯搜尋的運作方式 {#how-smart-translation-search-works}

![智慧翻譯搜索流圖](assets/smart-translation-search-flow.png)

1. AEM使用者會執行全文搜尋，提供本地化的搜尋詞(例如： 西班牙語中&quot;man&quot;,&quot;hombre&quot;的詞)。
2. Apache Oak Machine Translation OSGi Bundle提供的智慧型翻譯搜尋功能已參與，並評估所提供的搜尋詞是否可使用註冊的語言套件進行翻譯。
3. 系統會收集步驟#2中所有翻譯的詞語，並在內部擴充查詢，將其加入搜尋詞中。 這個擴充的搜尋詞集（如果正常評估是針對AEM的搜尋索引來尋找相關符合項目）。
4. 會收集與原始詞語(&#39;hombre&#39;)或翻譯的詞語(&#39;man&#39;)相符的搜尋結果，並將使用者傳回為搜尋結果。

## 其他資源{#additional-resources}

* [使用AEM資產設定智慧型翻譯搜尋](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)