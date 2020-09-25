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

AEM無頭教學課程的第3章涵蓋從第2章中建立的內容片段模型建立和製作事件內容 [片段](./chapter-2.md)。

## 編寫事件內容片段

建立內 [!DNL Event] 容片段模型並將WKND的AEM設定套用至「資產」檔案夾( `/content/dam/wknd-mobile` 透過屬性)後，就可建立 `cq:conf`[!DNL Event] 內容片段。

「內容片段」是一種資產，在AEM Assets中應該像其他資產一樣加以組織和管理。

* 如果需要翻譯（或可能需要翻譯），請在「資產」檔案夾結構中使用地區設定檔案夾
* 以邏輯方式組織內容片段，以方便尋找和管理

在此步驟中，請在資產檔 [!DNL Event] 案夾 `Punkrock Fest` 中建立 `/content/dam/wknd-mobile/en/events` 新檔案。

1. 導覽至「 **[!UICONTROL AEM]>資[!UICONTROL 產]>[!UICONTROL >建立資][!DNL WKND Mobile][!DNL English]****[!DNL Events]**&#x200B;產資料夾」。
1. 在 **[!UICONTROL >]>[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English][!DNL Events]****[!DNL Event]****[!DNL Punkrock Fest]**>建立標題為新內容片段類型的資產。
1. 製作新建立的 [!DNL Event] 內容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;輸入幾行說明……>**
   * [!DNL Event Date] : **&lt;選擇未來日期>**
   * [!DNL Event Type] : **音樂**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬蟲屋**
   * [!DNL Venue City] : **紐約**

   點選 **[!UICONTROL 頂端動作列中的]** 「儲存」以儲存變更。

1. 使用 [AEM的Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝下列套件。 此套件包含數個事件內容片段。

   [取得檔案：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 檢視內容片段的JCR結構

*本節僅提供資訊，用於將內容片段模型所建立之內容片段的基礎JCR結構社交化。*

1. 在AEM Author上開 **[啟CRXDE](http://localhost:4502/crx/de/index.jsp)** Lite。
1. 在CRXDE Lite的左側階層功能表中，導覽至 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) ，此節點代表JCR中的 [!DNL Punkrock Fest][!DNL Event] Content Fragment。
1. 展開數 [據節](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 點。
在「屬 **性」窗格中** ，查看其是否具有指向「內 `cq:model` 容片段模型 [!DNL Event] 」定義的屬性。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在節點 `data` 下選擇主節 [點](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ，並查看屬性。 此節點包含在編寫內容片段模型期間收 [!DNL Event] 集的內容。 JCR屬性名稱對應於「內容片段模型」屬性名稱，值對應於「[!DNL Punkrock Fest]」內容片 [!DNL Event] 段的創作值。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 下一步

建議您透過 [AEM的](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Package Manager，將 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip [!UICONTROL content套件安裝在AEM Author上]](http://localhost:4502/crx/packmgr/index.jsp)。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第4章——定義AEM Content Services範本](./chapter-4.md)
