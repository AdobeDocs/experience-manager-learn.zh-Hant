---
title: 第3章 — 製作事件內容片段 — 內容服務
description: AEM Headless教學課程的第3章涵蓋從第2章建立的內容片段模式建立及編寫事件內容片段。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 第3章 — 編寫事件內容片段

AEM Headless教學課程的第3章涵蓋從在[第2](./chapter-2.md)章中建立的內容片段模式建立及編寫事件內容片段。

## 編寫事件內容片段

建立[!DNL Event]內容片段模型，並將WKND的AEM設定套用至`/content/dam/wknd-mobile`資產資料夾（透過`cq:conf`屬性），即可建立[!DNL Event]內容片段。

內容片段是一種資產，應像其他資產一樣在AEM Assets中組織和管理。

* 如果需要（或可能需要）翻譯，請在Assets資料夾結構中使用地區資料夾
* 以邏輯方式組織內容片段，以便輕鬆找到和管理

在此步驟中，請在`/content/dam/wknd-mobile/en/events`資產資料夾中為`Punkrock Fest`建立新的[!DNL Event]。

1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 檔案] > [!DNL WKND Mobile] >[!DNL English]**，並建立資產資料夾&#x200B;**[!DNL Events]**。
1. 在&#x200B;**[!UICONTROL Assets] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;中，建立標題為&#x200B;**[!DNL Punkrock Fest]**&#x200B;且型別為&#x200B;**[!DNL Event]**&#x200B;的新內容片段。
1. 編寫新建立的[!DNL Event]內容片段。

   * [!DNL Event Title] ： **[!DNL Punkrock Fest]**
   * [!DNL Event Description] ： **&lt;輸入幾行說明……>**
   * [!DNL Event Date] ： **&lt;選取未來的日期>**
   * [!DNL Event Type] ： **音樂**
   * [!DNL Ticket Price] ： **10**
   * [!DNL Event Image] ： **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] ： **爬行動物屋**
   * [!DNL Venue City] ： **紐約**

   點選頂端動作列中的&#x200B;**[!UICONTROL 儲存]**&#x200B;以儲存變更。

1. 使用[AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝以下封裝。 此套件包含許多事件內容片段。

   [取得檔案： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 檢閱內容片段的JCR結構

*本節僅供參考，其目的是將內容片段模型中所製作內容片段的基礎JCR結構社會化。*

1. 在AEM作者上開啟&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**。
1. 在CRXDE Lite的左側階層功能表中，導覽至[/content/dam/wknd-mobile/en/events/punkrock-fest/jcr：content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)，此節點代表JCR中的[!DNL Punkrock Fest] [!DNL Event]內容片段。
1. 展開[資料](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點。
檢閱&#x200B;**[內容]窗格**&#x200B;中的屬性`cq:model`是否指向[!DNL Event]內容片段模型定義。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在`data`節點底下選取[主要](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點並檢閱屬性。 此節點包含在編寫[!DNL Event]內容片段模型期間收集的內容。 JCR屬性名稱對應至內容片段模型屬性名稱，而值對應至&quot;[!DNL Punkrock Fest]&quot; [!DNL Event]內容片段的編寫值。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建議您透過[AEM的[!UICONTROL 封裝管理員]](http://localhost:4502/crx/packmgr/index.jsp)在AEM Author上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容封裝。 此套件包含本教學課程及先前章節中概述的設定和內容。

* [第4章 — 定義AEM Content Services範本](./chapter-4.md)
