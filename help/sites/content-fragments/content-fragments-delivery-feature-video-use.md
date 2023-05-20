---
title: 在中傳送內AEM容片段
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: 內容片段，與佈局無關，可直接在具有核心元件的AEM Sites使用，或可以無頭方式傳送到下游頻道。
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 7%

---

# 傳遞內容片段 {#delivering-content-fragments}

Adobe Experience Manager(AEM)內容片段是基於文本的編輯內容，可能包括一些與某些結構化資料元素相關聯但被認為是純內容，而沒有設計或佈局資訊。 內容片段通常建立為與頻道無關的內容，該內容旨在跨頻道使用和重新使用，這反過來又將內容包裝為特定於上下文的體驗。

內容片段，與佈局無關，可直接在具有核心元件的AEM Sites使用，或可以無頭方式傳送到下游頻道。

此視頻系列介紹了使用內容片段的傳遞選項。 有關定義和 [可在此處找到創作內容片段](content-fragments-feature-video-use.md)。

1. 在網頁上使用內容片段
2. 使用內容服務將內容片段AEM顯示為JSON
3. 使用資產HTTP API

## 在網頁中使用內容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

內容片段可以在AEM Sites頁面上使用，或以類似方式使用「經驗片段」，AEM使用WCM核心元件 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)。

可以使用樣式系統設定內AEM容片段元件的樣式，以根據需要顯示內容。

## 將內容片段顯示為JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

Content ServicesAEM有助於建立基AEM於頁面的HTTP端點，將內容格式副本化為規範化JSON格式。

以上視頻使用 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) 來顯示單個內容片段。 的 [內容片段清單元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 是一個新元件，它允許作者定義一個查詢，該查詢將用內容片段清單動態填充頁面。 當需要公開多個內容片段時，首選內容片段清單元件。

*Content Services端點JSON負載示例：*\
**[運動員數](assets/athletes.json)**

## 使用資產HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

首先在AEM6.5中介紹的是使用資產HTTP API增強對內容片段的支援。 這為開發人員提供了一種針對內容片段執行建立、讀取、更新和刪除(CRUD)操作的簡單方法。

*POSTMAN請求示例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 要使用哪種遞送方法

### Web Channel

通過Web渠道傳遞內容片段的方法是直接的，方法是將內容片段元件與AEM Sites一起使用。

### Headless

在無頭使用情形中，有兩個選項可將內容片段作為JSON公開以支援第三方通道：

1. 當主AEM要用例是通過第三方渠道提供內容片段以供使用（只讀）時，請使用Content Services和代理API頁(視頻#2)。 Content Services框架提供了更靈活的選項，可以選擇哪些資料會被洩露。 開發人員還可以擴展內容服務框架以增加和/或豐富資料。

2. 當第三方通道需要修改和/或更新內容片段時，請使用Assets HTTP API(Video #3)。 一個典型的使用案例是在作者環境中接收第AEM三方內容。

## 其他資源 {#additional-resources}

* [編寫內容片段](content-fragments-feature-video-use.md)
* [AEM WCM 核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [WCMAEM核心內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

要在6.4+實例上從視頻系AEM列下載並安裝以下軟體包，以獲得最終狀態：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
