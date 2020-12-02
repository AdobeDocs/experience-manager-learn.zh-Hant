---
title: 第6章——將AEM Publish上的內容公開為JSON —— 內容服務
description: AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」上，以允許從「行動應用程式」使用。
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# 第6章——公開AEM Publish中的內容以進行傳送

AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」上，以允許「行動應用程式」使用。

## 發佈AEM Content Services的內容

透過AEM Content Services驅動「事件」的設定和內容必須發佈至AEM Publish，讓「行動應用程式」可以存取。

由於AEM Content Services是從「設定」（內容片段模型、可編輯範本）、「資產」（內容片段、影像）和「頁面」建立的，所以這些片段都會自動體驗AEM的內容管理功能，包括：

* 審核與處理的工作流程
* 以及從AEM Publish的AEM Content Services端點推送和拉出內容的啟動／停用

1. 確保&#x200B;**[!DNL WKND Mobile]Application Packages**（列於[Chapter 1](./chapter-1.md#wknd-mobile-application-packages)）已使用[!UICONTROL Package Manager]安裝在&#x200B;**AEM Publish**&#x200B;上。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈&#x200B;**[!DNL WKND Mobile Events API]可編輯範本**
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**
   1. 選擇&#x200B;**[!DNL Event API]**&#x200B;模板
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL Publish]**
   1. 發佈&#x200B;**template**&#x200B;和&#x200B;**所有參考**（內容原則、內容原則對應和範本）

1. 發佈&#x200B;**[!DNL WKND Mobile Events]內容片段**。

請注意，這是必要的，因為事件API使用內容片段清單元件，而不是特別引用內容片段。
1.導覽至「**[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**」
1.選取所有**[!DNL Event]**內容片段
1.點選頂端動作列中的「管理出版物」(**[!UICONTROL Manage Publication)
1.保留預設的** Publish **動作原樣，點選頂端動作列中的**[!UICONTROL  Next ]**1.選擇** all **內容片段
1.點選頂端動作列中的「發佈」(Publish)
* *[!DNL Events]內容片段模型和參考事件影像將會自動與內容片段一起發佈。*]**]****[!UICONTROL 

1. 發佈&#x200B;**[!DNL Events API]頁面**。
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選擇&#x200B;**[!DNL Events]**&#x200B;頁面
   1. 點選頂端動作列中的「管理出版物」****
   1. 保留預設的&#x200B;**Publish**&#x200B;動作原樣，點選頂端動作列中的&#x200B;**[!UICONTROL Next]**
   1. 選擇&#x200B;**[!DNL Events]**&#x200B;頁面
   1. 在頂端動作列中點選&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 驗證AEM Publish

1. 在新的Web瀏覽器中，請確定您已登出AEM Publish，並請求下列URL（以`http://localhost:4503`取代執行任何host:port AEM Publish的URL）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些請求應傳回與審閱對應AEM Author結束點時相同的JSON回應。 如果沒有，請確定所有發佈都成功（檢查複製佇列）、[!DNL WKND Mobile] `ui.apps`套件已安裝在AEM Publish上，並檢視`error.log`的AEM Publish。

## 下一步

沒有額外的套件可以安裝。 請確定本節中概述的內容和設定已發佈至AEM Publish，否則後續章節將無法運作。

* [第7章——從行動應用程式使用AEM Content Services](./chapter-7.md)
