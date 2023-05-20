---
title: 第3章 — 創作事件內容片段 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: 《無頭教程》AEM第3章涉及從第2章建立的內容片段模型中建立和創作事件內容片段。
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

# 第3章 — 創作事件內容片段

「無頭」教程AEM的第3章涉及從建立的內容片段模型建立和創作事件內容片段 [第二章](./chapter-2.md)。

## 創作事件內容片段

與 [!DNL Event] 已建立的內容片段模AEM型和應用於的WKND的配置 `/content/dam/wknd-mobile` 資產資料夾(通過 `cq:conf` 屬性), a [!DNL Event] 可以建立內容片段。

內容片段是一種資產類型，應像其他資產一樣在AEM Assets組織和管理。

* 如果需要翻譯（或可能需要翻譯），請在「資產」資料夾結構中使用區域設定資料夾
* 以邏輯方式組織內容片段，以便於查找和管理

在此步驟中，建立新 [!DNL Event] 為 `Punkrock Fest` 的 `/content/dam/wknd-mobile/en/events` assets資料夾。

1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] >[!DNL English]** 和建立資產資料夾 **[!DNL Events]**。
1. 在 **[!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 建立類型的新內容片段 **[!DNL Event]** 標題為 **[!DNL Punkrock Fest]**。
1. 編寫新建立的 [!DNL Event] 內容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音樂**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬蟲屋**
   * [!DNL Venue City] : **紐約**

   點擊 **[!UICONTROL 保存]** 的子菜單。

1. 使用 [包管AEM理器](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝下面的軟體包。 此包包含多個事件內容片段。

   [獲取檔案：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 評述內容片段的JCR結構

*此部分僅供參考，旨在將從內容片段模型生成的內容片段的基礎JCR結構進行社交。*

1. 開啟 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** AEM作者。
1. 在CRXDE Lite中，在左側層次結構菜單上，導航到 [/content/dam/wknd-mobile/en/events/punkrock fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) 即表示 [!DNL Punkrock Fest] [!DNL Event] JCR中的內容片段。
1. 展開 [資料](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 的下界。
在 **屬性窗格** 它擁有 `cq:model` 指向 [!DNL Event] 內容片段模型定義。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在 `data` 節點選擇 [主](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 並查看屬性。 此節點包含在創作 [!DNL Event] 內容片段模型。 JCR屬性名稱與「內容片段模型」屬性名稱相對應，這些值與「 」的創作值相對應[!DNL Punkrock Fest]&quot; [!DNL Event] 內容片段。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建議您安裝 [com.adobe.aem.guides.wknd-mobile content-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 通過 [AEM [!UICONTROL 包管理器]](http://localhost:4502/crx/packmgr/index.jsp)。 此軟體包包含本教程及前面各章中概述的配置和內容。

* [第4章 — 定義AEMContent Services模板](./chapter-4.md)
