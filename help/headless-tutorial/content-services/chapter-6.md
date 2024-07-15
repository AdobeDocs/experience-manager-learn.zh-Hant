---
title: 第6章 — 在AEM Publish上以JSON形式公開內容 — 內容服務
description: AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都在AEM Publish上，以允許行動應用程式的使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 第6章 — 在AEM Publish上公開內容以進行傳遞

AEM Headless教學課程的第6章涵蓋，確保所有必要的套件、設定和內容都在AEM Publish上，以允許行動應用程式使用。

## 發佈AEM內容服務的內容

為透過AEM Content Services驅動事件而建立的設定和內容必須發佈至AEM Publish，以便行動應用程式能夠存取。

由於AEM Content Services是根據設定（內容片段模型、可編輯的範本）、Assets （內容片段、影像）和頁面所建置，因此所有這些片段都會自動享用AEM的內容管理功能，包括：

* 稽核與處理的工作流程
* 以及從AEM Publish的AEM Content Services端點推送和提取內容的啟用/停用

1. 請使用[!UICONTROL 封裝管理員]，確認已在&#x200B;**AEM Publish**&#x200B;上安裝[第1](./chapter-1.md#wknd-mobile-application-packages)章所列的&#x200B;**[!DNL WKND Mobile]應用程式封裝**。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish **[!DNL WKND Mobile Events API]可編輯的範本**
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**
   1. 選取&#x200B;**[!DNL Event API]**&#x200B;範本
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL Publish]**
   1. Publish **範本**&#x200B;和&#x200B;**所有參考** （內容原則、內容原則對應和範本）

1. Publish **[!DNL WKND Mobile Events]內容片段**。

   請注意，這是必要專案，因為事件API使用內容片段清單元件，該元件未特別參考內容片段。

   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 選取所有&#x200B;**[!DNL Event]**&#x200B;內容片段
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 管理出版物]**
   1. 將預設&#x200B;**Publish**&#x200B;動作維持原狀，在頂端動作列中點選「**[!UICONTROL 下一步]**」
   1. 選取&#x200B;**所有**&#x200B;內容片段
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL Publish]**
      * *[!DNL Events]內容片段模型和參考事件影像將會與內容片段一起自動發佈。*

1. Publish **[!DNL Events API]頁面**。
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 網站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選取&#x200B;**[!DNL Events]**&#x200B;頁面
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 管理出版物]**
   1. 將預設&#x200B;**Publish**&#x200B;動作維持原狀，在頂端動作列中點選「**[!UICONTROL 下一步]**」
   1. 選取&#x200B;**[!DNL Events]**&#x200B;頁面
   1. 在頂端動作列中點選&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 驗證AEM Publish

1. 在新的Web瀏覽器中，確定您已登出AEM Publish並要求下列URL (將`http://localhost:4503`取代為執行AEM Publish的任何主機：連線埠)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   這些要求應傳回的JSON回應應與檢閱對應的AEM作者端點時相同。 如果沒有，請確定所有發行集都成功（檢查復寫佇列），[!DNL WKND Mobile] `ui.apps`封裝已安裝在AEM Publish上，並檢閱AEM Publish的`error.log`。

## 下一步

沒有要安裝的額外套件。 請確定本節中概述的內容和設定已發佈到AEM Publish，否則後續章節將無法運作。

* [第7章 — 從行動應用程式使用AEM內容服務](./chapter-7.md)
