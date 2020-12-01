---
title: 第3章——製作事件內容片段
seo-title: AEM Content Services快速入門——第3章——編寫事件內容片段
description: AEM無頭教學課程的第3章涵蓋從第2章中建立的內容片段模型建立和製作事件內容片段。
seo-description: AEM無頭教學課程的第3章涵蓋從第2章中建立的內容片段模型建立和製作事件內容片段。
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---


# 第3章——製作事件內容片段

AEM無頭教學課程的第3章涵蓋從[第2章](./chapter-2.md)中建立的內容片段模型建立和編寫事件內容片段。

## 編寫事件內容片段

建立[!DNL Event]內容片段模型，並將WKND的AEM設定套用至`/content/dam/wknd-mobile`資產資料夾（透過`cq:conf`屬性）後，即可建立[!DNL Event]內容片段。

「內容片段」是一種資產，在AEM Assets中應該像其他資產一樣加以組織和管理。

* 如果需要翻譯（或可能需要翻譯），請在「資產」檔案夾結構中使用地區設定檔案夾
* 以邏輯方式組織內容片段，以方便尋找和管理

在此步驟中，請在`/content/dam/wknd-mobile/en/events`資產資料夾中，為`Punkrock Fest`建立新的[!DNL Event]。

1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL  Files] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;並建立資產資料夾&#x200B;**[!DNL Events]**。
1. 在&#x200B;**[!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;中，建立標題為&#x200B;**[!DNL Punkrock Fest]**&#x200B;的類型&#x200B;**[!DNL Event]**&#x200B;的新內容片段。
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

1. 使用[AEM的Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，將下列套件安裝在AEM Author。 此套件包含數個事件內容片段。

   [取得檔案：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 檢視內容片段的JCR結構

*本節僅提供資訊，用於將內容片段模型所建立之內容片段的基礎JCR結構社交化。*

1. 在「AEM作者」上開啟&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**。
1. 在CRXDE Lite的左側階層功能表中，導覽至[/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)，此節點代表JCR中的[!DNL Punkrock Fest] [!DNL Event]內容片段。
1. 展開[data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點。
在**「屬性」窗格**&#x200B;中查看其具有指向「[!DNL Event]內容片段模型」定義的`cq:model`屬性。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在`data`節點下，選擇[master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)節點並查看屬性。 此節點包含在製作[!DNL Event]內容片段模型期間收集的內容。 JCR屬性名稱對應於「內容片段模型」屬性名稱，值對應於「[!DNL Punkrock Fest]」[!DNL Event]內容片段的創作值。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 下一步

建議您透過[AEM的[!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝[com.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第4章——定義AEM Content Services範本](./chapter-4.md)
