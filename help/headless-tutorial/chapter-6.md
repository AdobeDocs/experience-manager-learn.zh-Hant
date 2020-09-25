---
title: AEM Headless快速入門——第6章——將AEM Publish上的內容公開為JSON
description: AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」上，以允許從「行動應用程式」使用。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# 第6章——公開AEM Publish中的內容以進行傳送

AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都位於「AEM發佈」上，以允許「行動應用程式」使用。

## 發佈AEM Content Services的內容

透過AEM Content Services驅動「事件」的設定和內容必須發佈至AEM Publish，讓「行動應用程式」可以存取。

由於AEM Content Services是從「設定」（內容片段模型、可編輯範本）、「資產」（內容片段、影像）和「頁面」建立的，所以這些片段都會自動體驗AEM的內容管理功能，包括：

* 審核與處理的工作流程
* 以及從AEM Publish的AEM Content Services端點推送和拉出內容的啟動／停用

1. 請確定「 **[!DNL WKND Mobile]Application Packages**( [Application Packages](./chapter-1.md#wknd-mobile-application-packages)，列於第1章)」是使用 **Package Manager在** AEM Publish 上安裝的。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈可編 **[!DNL WKND Mobile Events API]輯範本**
   1. 導覽至「 **[!UICONTROL AEM]>工具[!UICONTROL >一般范]本[!UICONTROL >]Templates>[!DNL WKND Mobile]**
   1. 選擇范 **[!DNL Event API]** 本
   1. 在頂端 **[!UICONTROL 動作列中點選]** 「發佈」
   1. 發佈范 **本****和所有參照** （內容原則、內容原則對應和範本）

1. 發佈內 **[!DNL WKND Mobile Events]容片段**。

請注意，這是必要的，因為事件API使用內容片段清單元件，而不是特別引用內容片段。
1.導覽至「AEM **[!UICONTROL >資]產>>[!UICONTROL Assets]>[!DNL WKND Mobile]>[!DNL English][!DNL Events]** J1」。選取所有 **[!DNL Event]** 內容片段1。點選頂端 **[!UICONTROL 動作列]** 1中的「管理出版物」。保留預設 **的「發佈** 」動作原樣，點選頂端動 **[!UICONTROL 作列1中的「下一步]** 」。選取 **所有** content fragments1。點選 **[!UICONTROL 頂端動作列中的Publish]** * 「內容片段模型」和 *[!DNL Events]參照「事件影像」會與內容片段一起自動發佈。*

1. 發佈頁 **[!DNL Events API]面**。
   1. 導覽至 **[!UICONTROL AEM]>網[!UICONTROL 站]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. 選擇頁 **[!DNL Events]** 面
   1. 點選頂端 **[!UICONTROL 動作列中的]** 「管理出版物」
   1. 保留預設的 **「發佈** 」動作原樣，點選頂端動作列中的「 **[!UICONTROL Next]** 」（下一步）
   1. 選擇頁 **[!DNL Events]** 面
   1. 在頂端 **[!DNL Publish]** 動作列中點選

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 驗證AEM Publish

1. 在新的Web瀏覽器中，請確定您已登出AEM Publish，並請求下列URL(取代正在執 `http://localhost:4503` 行的任何主機：埠AEM Publish)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些請求應傳回與審閱對應AEM Author結束點時相同的JSON回應。 如果沒有，請確定所有發佈都成功（請檢查「複製」佇列）, [!DNL WKND Mobile] 此套件 `ui.apps` 已安裝在AEM Publish上，並檢 `error.log` 閱AEM Publish的相關資訊。

## 下一步

沒有額外的套件可以安裝。 請確定本節中概述的內容和設定已發佈至AEM Publish，否則後續章節將無法運作。

* [第7章——從行動應用程式使用AEM Content Services](./chapter-7.md)
