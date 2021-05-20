---
title: 第3章 — 編寫事件內容片段 — 內容服務
seo-title: AEM內容服務快速入門 — 第3章 — 編寫事件內容片段
description: AEM Headless教學課程的第3章涵蓋從第2章建立的內容片段模型中建立和編寫事件內容片段。
seo-description: AEM Headless教學課程的第3章涵蓋從第2章建立的內容片段模型中建立和編寫事件內容片段。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 2%

---


# 第3章 — 編寫事件內容片段

AEM Headless教學課程的第3章涵蓋從[Chapter 2](./chapter-2.md)中建立的內容片段模型中建立和編寫事件內容片段。

## 編寫事件內容片段

建立[!DNL Event]內容片段模型，並將WKND的AEM設定套用至`/content/dam/wknd-mobile`資產資料夾（透過`cq:conf`屬性）後，即可建立[!DNL Event]內容片段。

內容片段是一種資產，應像其他資產一樣，在AEM Assets中組織和管理。

* 如果需要翻譯（或可能需要），請在Assets資料夾結構中使用地區設定資料夾
* 以邏輯方式組織內容片段，以便輕鬆找到和管理

在此步驟中，請在`/content/dam/wknd-mobile/en/events`資產資料夾中為`Punkrock Fest`建立新的[!DNL Event]。

1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;並建立資產資料夾&#x200B;**[!DNL Events]**。
1. 在&#x200B;**[!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;內建立標題為&#x200B;**[!DNL Punkrock Fest]**&#x200B;的&#x200B;**[!DNL Event]**&#x200B;類型的新內容片段。
1. 製作新建立的[!DNL Event]內容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音樂**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬蟲屋**
   * [!DNL Venue City] : **紐約**

   點選頂端動作列中的&#x200B;**[!UICONTROL 儲存]**&#x200B;以儲存變更。

1. 使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝以下套件。 此套件包含許多事件內容片段。

   [獲取檔案：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 檢閱內容片段的JCR結構

*本節僅提供資訊，目的是將從內容片段模型建立的內容片段基礎JCR結構社交化。*

1. 在AEM作者上開啟&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**。
1. 在CRXDE Lite中，在左側階層功能表中，導覽至[/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)，這是JCR中代表[!DNL Punkrock Fest] [!DNL Event]內容片段的節點。
1. 展開[data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點。
在**屬性窗格**&#x200B;中查看其是否具有指向[!DNL Event]內容片段模型定義的屬性`cq:model`。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在`data`節點下，選擇[master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點並查看屬性。 此節點包含編寫[!DNL Event]內容片段模型期間收集的內容。 JCR屬性名稱對應於內容片段模型屬性名稱，且值對應於「[!DNL Punkrock Fest]」 [!DNL Event]內容片段的創作值。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 下一步

建議您透過[AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)，在AEM作者上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第4章 — 定義AEM內容服務範本](./chapter-4.md)
