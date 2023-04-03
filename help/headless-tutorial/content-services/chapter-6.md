---
title: 第6章 — 將AEM發佈中的內容公開為JSON — 內容服務
description: AEM Headless教學課程的第6章涵蓋確保所有必要套件、設定和內容都位於AEM Publish上，以允許從行動應用程式使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 第6章 — 公開AEM發佈中的內容以供傳送

AEM Headless教學課程的第6章涵蓋確保所有必要套件、設定和內容都位於AEM Publish上，以允許行動應用程式使用。

## 發佈AEM Content Services的內容

您必須將透過AEM Content Services來驅動事件的設定和內容發佈至AEM Publish，行動應用程式才能存取。

因為AEM Content Services是從設定（內容片段模型、可編輯的範本）、資產（內容片段、影像）和頁面建置而成，所以這些片段都會自動享有AEM內容管理功能，包括：

* 審核與處理的工作流程
* 從AEM Publish的AEM Content Services端點推送和提取內容的啟用/停用

1. 確保 **[!DNL WKND Mobile]應用程式包**，列於 [第1章](./chapter-1.md#wknd-mobile-application-packages)，則安裝在 **AEM發佈** 使用 [!UICONTROL 封裝管理員].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈 **[!DNL WKND Mobile Events API]可編輯的範本**
   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**
   1. 選取 **[!DNL Event API]** 範本
   1. 點選 **[!UICONTROL 發佈]** 在頂端動作列中
   1. 發佈 **範本** 和 **所有參照** （內容策略、內容策略映射和模板）

1. 發佈 **[!DNL WKND Mobile Events]內容片段**.

   請注意，由於事件API使用內容片段清單元件，而不明確參考內容片段，因此此為必要項目。

   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 選取所有 **[!DNL Event]** 內容片段
   1. 點選 **[!UICONTROL 管理出版物]** 在頂端動作列中
   1. 保留預設值 **發佈** 動作原樣，點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 選擇 **all** 內容片段
   1. 點選 **[!UICONTROL 發佈]** 在頂端動作列中
      * *此 [!DNL Events] 內容片段模型和參考事件影像將會與內容片段一起自動發佈。*

1. 發佈 **[!DNL Events API]頁面**.
   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 網站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選取 **[!DNL Events]** 頁面
   1. 點選 **[!UICONTROL 管理出版物]** 在頂端動作列中
   1. 保留預設值 **發佈** 動作原樣，點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 選取 **[!DNL Events]** 頁面
   1. 點選 **[!DNL Publish]** 在頂端動作列中

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 驗證AEM發佈

1. 在新的網頁瀏覽器中，確認您已登出AEM Publish，並要求下列URL(以取代 `http://localhost:4503` 適用於任何主機：埠AEM發佈正在執行)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些要求應會傳回與審核對應AEM作者端點時相同的JSON回應。 如果未成功，請確保所有發佈都成功（請檢查複製隊列）, [!DNL WKND Mobile] `ui.apps` 套件安裝在AEM發佈上，並檢閱 `error.log` （適用於AEM發佈）。

## 下一步

沒有要安裝的額外軟體包。 請確定本節概述的內容和設定已發佈至AEM Publish，否則後續章節將無法運作。

* [第7章 — 從行動應用程式使用AEM內容服務](./chapter-7.md)
