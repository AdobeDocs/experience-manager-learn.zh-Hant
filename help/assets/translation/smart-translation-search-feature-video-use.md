---
title: 使用智慧翻譯搜索與AEM Assets
description: 智慧型翻譯搜尋可自動跨內容（包括資產和頁面）AEM進行跨語言搜尋和搜尋，支援超過50種語言，並減少手動內容轉譯的需求。
version: 6.3, 6.4, 6.5
feature: 搜尋
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# 使用智慧翻譯搜索與AEM Assets{#using-smart-translation-search-with-aem-assets}

智慧型翻譯搜尋可自動跨內容（包括資產和頁面）AEM進行跨語言搜尋和搜尋，支援超過50種語言，並減少手動內容轉譯的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

智AEM慧型翻譯搜尋可讓使用者使用非英文詞AEM語來搜尋內容，以比對AEM上具有相同英文詞語的資產。

智慧型翻譯搜尋是套用英AEM文資產的智慧型標籤的完美輔助。

此視訊假設已AEM設定[智慧型翻譯搜尋](smart-translation-search-technical-video-setup.md)。

## 智慧型翻譯搜尋的運作方式{#how-smart-translation-search-works}

![智慧翻譯搜索流圖](assets/smart-translation-search-flow.png)

1. 使AEM用者執行全文搜尋，提供本地化搜尋詞(例如 西班牙語中&quot;man&quot;,&quot;hombre&quot;的詞)。
2. Apache Oak Machine Translation OSGi Bundle提供的智慧型翻譯搜尋功能已參與，並評估所提供的搜尋詞是否可使用註冊的語言套件進行翻譯。
3. 系統會收集步驟#2中所有翻譯的詞語，並在內部擴充查詢，將其加入搜尋詞中。 此增強的搜尋詞集(如果正常評估時是根據搜尋索引找到AEM相關的相符項目)。
4. 會收集與原始詞語(&#39;hombre&#39;)或翻譯的詞語(&#39;man&#39;)相符的搜尋結果，並將使用者傳回為搜尋結果。

## 其他資源{#additional-resources}

* [設定智慧翻譯搜索與AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)