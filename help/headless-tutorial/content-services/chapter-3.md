---
title: 第3章 — 編寫事件內容片段 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: AEM Headless教學課程的第3章涵蓋從第2章建立的內容片段模型中建立和編寫事件內容片段。
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 2%

---

# 第3章 — 編寫事件內容片段

AEM Headless教學課程的第3章涵蓋從中建立的內容片段模型建立和編寫事件內容片段， [第二章](./chapter-2.md).

## 編寫事件內容片段

使用 [!DNL Event] 已建立的內容片段模型和套用至的AEM WKND組態 `/content/dam/wknd-mobile` 資產資料夾(透過 `cq:conf` 屬性), a [!DNL Event] 可建立內容片段。

內容片段是一種資產，應像其他資產一樣，在AEM Assets中組織和管理。

* 如果需要翻譯（或可能需要），請在Assets資料夾結構中使用地區設定資料夾
* 以邏輯方式組織內容片段，以便輕鬆找到和管理

在此步驟中，請建立新 [!DNL Event] for `Punkrock Fest` 在 `/content/dam/wknd-mobile/en/events` 資產資料夾。

1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] >[!DNL English]** 和建立資產資料夾 **[!DNL Events]**.
1. 內 **[!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 建立類型的新內容片段 **[!DNL Event]** 標題為 **[!DNL Punkrock Fest]**.
1. 製作新建立的 [!DNL Event] 內容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音樂**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬蟲屋**
   * [!DNL Venue City] : **紐約**

   點選 **[!UICONTROL 儲存]** ，以儲存變更。

1. 使用 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，請在AEM Author上安裝以下套件。 此套件包含許多事件內容片段。

   [獲取檔案：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 檢閱內容片段的JCR結構

*本節僅提供資訊，目的是將從內容片段模型建立的內容片段基礎JCR結構社交化。*

1. 開啟 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** 在AEM作者上。
1. 在CRXDE Lite中，在左側階層功能表中，導覽至 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) 代表 [!DNL Punkrock Fest] [!DNL Event] JCR中的內容片段。
1. 展開 [資料](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 節點。
在 **屬性窗格** 它有屬性 `cq:model` 指向 [!DNL Event] 內容片段模型定義。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在 `data` 節點選擇 [主版](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 節點並檢閱屬性。 此節點包含編寫 [!DNL Event] 內容片段模型。 JCR屬性名稱對應於「內容片段模型」屬性名稱，值對應於「 」的創作值[!DNL Punkrock Fest]&quot; [!DNL Event] 內容片段。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建議您安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的內容套件(透過 [AEM [!UICONTROL 封裝管理員]](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第4章 — 定義AEM內容服務範本](./chapter-4.md)
