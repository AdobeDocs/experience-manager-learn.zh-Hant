---
title: 第6章 — 將AEM發佈中的內容公開為JSON — 內容服務
description: AEM Headless教學課程的第6章涵蓋確保所有必要套件、設定和內容都位於AEM Publish上，以允許從行動應用程式使用。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# 第6章 — 公開AEM發佈中的內容以供傳送

AEM Headless教學課程的第6章涵蓋確保所有必要套件、設定和內容都位於AEM Publish上，以允許行動應用程式使用。

## 發佈AEM Content Services的內容

您必須將透過AEM Content Services來驅動事件的設定和內容發佈至AEM Publish，行動應用程式才能存取。

因為AEM Content Services是從設定（內容片段模型、可編輯的範本）、資產（內容片段、影像）和頁面建置而成，所以這些片段都會自動享有AEM內容管理功能，包括：

* 審核與處理的工作流程
* 從AEM Publish的AEM Content Services端點推送和提取內容的啟用/停用

1. 確保&#x200B;**[!DNL WKND Mobile]應用程式套件**（列於[Chapter 1](./chapter-1.md#wknd-mobile-application-packages)中）是使用[!UICONTROL 套件管理器]安裝在&#x200B;**AEM Publish**&#x200B;上。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈&#x200B;**[!DNL WKND Mobile Events API]可編輯的範本**
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**
   1. 選擇&#x200B;**[!DNL Event API]**&#x200B;模板
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 發佈]**
   1. 發佈&#x200B;**template**&#x200B;和&#x200B;**所有引用**（內容策略、內容策略映射和模板）

1. 發佈&#x200B;**[!DNL WKND Mobile Events]內容片段**。

請注意，由於事件API使用內容片段清單元件，而不明確參考內容片段，因此此為必要項目。
1.導覽至**[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1.選取所有**[!DNL Event]**內容片段
1.點選頂端動作列中的**[!UICONTROL 管理出版物]**
1.將預設的**Publish**&#x200B;動作維持原狀，點選頂端動作列中的&#x200B;**[!UICONTROL Next]**
1.選取**all**內容片段
1.點選頂端動作列中的**[!UICONTROL 發佈]**
* *將自動發佈[!DNL Events]內容片段模型和引用事件影像以及內容片段。*

1. 發佈&#x200B;**[!DNL Events API]頁面**。
   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選擇&#x200B;**[!DNL Events]**&#x200B;頁
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 管理出版物]**
   1. 將預設的&#x200B;**Publish**&#x200B;動作維持原狀，點選頂端動作列中的&#x200B;**[!UICONTROL Next]**
   1. 選擇&#x200B;**[!DNL Events]**&#x200B;頁
   1. 點選頂端動作列中的&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 驗證AEM發佈

1. 在新的Web瀏覽器中，確認您已登出AEM Publish，並要求下列URL（以`http://localhost:4503`取代任何主機：連接埠AEM Publish正在執行）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些要求應會傳回與審核對應AEM作者端點時相同的JSON回應。 若未成功，請確定所有發佈皆成功（檢查復寫佇列）,[!DNL WKND Mobile] `ui.apps`套件已安裝在AEM Publish上，並檢閱`error.log`以取得AEM Publish。

## 下一步

沒有要安裝的額外軟體包。 請確定本節概述的內容和設定已發佈至AEM Publish，否則後續章節將無法運作。

* [第7章 — 從行動應用程式使用AEM內容服務](./chapter-7.md)
