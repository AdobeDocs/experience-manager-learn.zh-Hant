---
title: 第6章——將AEM Publish上的內容公開為JSON —— 內容服務
description: 「無頭教學AEM課程」的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」中，以允許從「行動應用程式」使用。
feature: 內容片段、API
topic: 無頭、內容管理
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# 第6章——公開AEM Publish中的內容以進行傳送

「無頭教學AEM課程」的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」中，以允許「行動應用程式」使用。

## 發佈內容服AEM務內容

為透過Content Services驅動「事件」而建立AEM的設定和內容必須發佈至AEM Publish，讓「行動應用程式」可以存取它。

由於AEMContent Services是從設定（內容片段模型、可編輯範本）、資產（內容片段、影像）和頁面建立的，所以這些片段都會自動享有內容AEM管理功能，包括：

* 審核與處理的工作流程
* 以及從AEM Publish的「內容服務」端點推送和拉出內容AEM的啟動／停用

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
* *[!DNL Events]內容片段模型和參考事件影像將會自動與內容片段一起發佈。*]******

1. 發佈&#x200B;**[!DNL Events API]頁面**。
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL  Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
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

* [第7章——從行動應AEM用程式使用內容服務](./chapter-7.md)
