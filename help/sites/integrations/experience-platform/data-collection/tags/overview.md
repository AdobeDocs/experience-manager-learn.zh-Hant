---
title: 整合Experience Platform資料收集標籤（啟動）AEM和
description: Experience Platform資料收集中的標籤是Adobe的下一代標籤管理解決方案，也是部署Adobe Analytics、目標、Audience Manager和更多解決方案的最佳方式。 獲取標籤（以前稱為Launch）和建議與Adobe Experience Manager整合的概述。
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

# 整合Experience Platform資料收集標AEM簽 {#overview}

瞭解如何整合Experience Platform _資料收集標籤_ （前稱Launch）與Adobe Experience Manager。

>[!NOTE]
>
>Adobe Experience Platform Launch已被改名為Adobe Experience Platform的一套資料收集技術。 因此，在產品文檔中已進行了一些術語更改。 請參閱以下內容 [文檔](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 的下一頁。


標籤是Adobe Experience Platform的下一代標籤管理技術。 標籤提供了部署Adobe Analytics、目標、Audience Manager和更多解決方案的最簡單方法。 獲取標籤的概述以及建議與Adobe Experience Manager的整合。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 必備條件

整合Experience Platform資料收集標籤時，需要執行以下操作。

+ AEM管理員訪AEM問as a Cloud Service
+ 參考站點，如 [WKND](https://github.com/adobe/aem-guides-wknd) 部署在它上。
+ 訪問Adobe Experience Platform資料收集解決方案
+ 系統管理員對 [Adobe Developer控制台](https://developer.adobe.com/developer-console/)


## 高級步驟

+ 在Adobe Experience Platform資料收集中，建立Tag屬性並將其編輯為 _添加規則_。 然後 _添加庫_，選擇新添加的規則，批准並發佈它。
+ 使用AEM現有（或新）IMS配置連接和標籤
+ 在中AEM，建立啟動雲服務配置，然後將其應用於現有站點，最後驗證「標籤」屬性及其庫是否已載入到已發佈或作者站點上。

## 後續步驟

[建立標籤屬性](create-tag-property.md)

## 其他資源 {#additional-resources}

+ [Experience Platform與Experience Cloud應用程式的整合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [標籤概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在帶標籤的網站中實現Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
