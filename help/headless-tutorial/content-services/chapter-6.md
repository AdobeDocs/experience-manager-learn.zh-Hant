---
title: 第6章 — 將AEM上的內容公開為JSON — 內容服務
description: 「無頭」教程AEM的第6章涉及確保AEM Publish上的所有必要包、配置和內容都可以從移動應用中使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 第6章 — 公開AEM發佈以供遞送的內容

「無頭」教程AEM的第6章涉及確保AEM Publish上所有必要的包、配置和內容都可供移動應用使用。

## 發佈內容AEM服務

必須將為通過Content Services驅動事件而建立AEM的配置和內容發佈到AEM Publish，以便Mobile App可以訪問它。

因AEM為Content Services是從配置（內容片段模型、可編輯模板）、資產（內容片段、影像）和頁面構建的，所有這些部分都可自動享AEM受內容管理功能，包括：

* 用於審閱和處理的工作流
* 並激活/取消激活AEM Publish的Content Services端點中AEM的推送和拉取內容

1. 確保 **[!DNL WKND Mobile]應用程式套件**，列出 [第一章](./chapter-1.md#wknd-mobile-application-packages)，安裝於 **AEM發佈** 使用 [!UICONTROL 包管理器]。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈 **[!DNL WKND Mobile Events API]可編輯模板**
   1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常規] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 選擇 **[!DNL Event API]** 模板
   1. 點擊 **[!UICONTROL 發佈]** 的子菜單。
   1. 發佈 **模板** 和 **所有引用** （內容策略、內容策略映射和模板）

1. 發佈 **[!DNL WKND Mobile Events]內容片段**。

   請注意，這是必需的，因為事件API使用內容片段清單元件，該元件不專門引用內容片段。

   1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 選擇所有 **[!DNL Event]** 內容片段
   1. 點擊 **[!UICONTROL 管理發布]** 的子菜單。
   1. 離開預設值 **發佈** 按原樣操作，點擊 **[!UICONTROL 下一個]** 的子菜單。
   1. 選擇 **全部** 內容片段
   1. 點擊 **[!UICONTROL 發佈]** 的子菜單。
      * *的 [!DNL Events] 內容片段模型和引用事件影像將自動與內容片段一起發佈。*

1. 發佈 **[!DNL Events API]頁**。
   1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 站點] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選擇 **[!DNL Events]** 頁
   1. 點擊 **[!UICONTROL 管理發布]** 的子菜單。
   1. 離開預設值 **發佈** 按原樣操作，點擊 **[!UICONTROL 下一個]** 的子菜單。
   1. 選擇 **[!DNL Events]** 頁
   1. 點擊 **[!DNL Publish]** 的子菜單。

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 驗證AEM發佈

1. 在新的Web瀏覽器中，確保您已註銷AEM發佈並請求以下URL(替換 `http://localhost:4503` 對於運行的主機：埠AEM發佈)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些請求應返回與審閱相應的AEM作者端點時相同的JSON響應。 如果它們不成功，請確保所有發佈都成功（檢查複製隊列）, [!DNL WKND Mobile] `ui.apps` 軟體包安裝在AEM發佈上，並查看 `error.log` 為AEM發佈。

## 下一步

沒有要安裝的額外包。 確保本節中概述的內容和配置已發佈到AEM Publish，否則後續章節將不起作用。

* [第7章 — 從移AEM動應用程式使用內容服務](./chapter-7.md)
