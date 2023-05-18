---
title: 整合Experience Platform資料收集標籤(Launch)和AEM
description: 「Experience Platform資料收集」中的標籤是Adobe的新一代標籤管理解決方案，也是部署Adobe Analytics、Target、Audience Manager和其他解決方案的最佳方式。 概略了解標籤（先前稱為Launch），以及建議的與Adobe Experience Manager整合。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 1%

---

# 整合Experience Platform資料收集標籤與AEM {#overview}

了解如何整合Experience Platform _資料收集標籤_ （前稱為Launch）與Adobe Experience Manager。

>[!NOTE]
>
>Adobe Experience Platform Launch在Adobe Experience Platform中已重新命名為一套資料收集技術。 因此，產品檔案中已推出數個術語變更。 請參閱下列內容 [檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 以匯整術語變更的參考。


標籤是Adobe Experience Platform新一代的標籤管理技術。 標籤提供部署Adobe Analytics、Target、Audience Manager和其他解決方案的最簡單方式。 取得標籤的概觀，並建議與Adobe Experience Manager整合。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 必備條件

整合Experience Platform資料收集標籤時，需要下列項目。

+ AEM管理員AEMas a Cloud Service環境存取權
+ 參考網站，如 [WKND](https://github.com/adobe/aem-guides-wknd) 部署在它上。
+ 存取Adobe Experience Platform資料收集解決方案
+ 系統管理員存取 [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## 高階步驟

+ 在Adobe Experience Platform資料收集中，建立標籤屬性並加以編輯 _新增規則_. 然後 _新增程式庫_，選取新增的規則，核准並發佈。
+ 使用現有（或新）IMS設定連線AEM和標籤
+ 在AEM中，建立Launch雲端服務設定，然後將其套用至現有網站，最後驗證「標籤」屬性，以及其程式庫已載入已發佈或製作網站。

## 後續步驟

[建立標籤屬性](create-tag-property.md)

## 其他資源 {#additional-resources}

+ [Experience Platform與Experience Cloud應用程式的整合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [標籤概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在含有標籤的網站中實作Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
