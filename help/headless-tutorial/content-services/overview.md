---
title: AEM Headless 快速入門 - 內容服務
description: 端對端教學課程，說明如何使用 AEM Headless 來建置和公開內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '327'
ht-degree: 100%

---

# AEM Headless 快速入門 - 內容服務

AEM 的內容服務善用傳統的 AEM 頁面來組成無周邊 REST API 端點，而 AEM 元件會定義或參照要在這些端點上公開的內容。

AEM 內容服務允許使用與在 AEM Sites 中製作網頁時相同的內容抽象化來定義這些 HTTP API 的內容和結構描述。行銷人員使用 AEM Pages 和 AEM Components，可以快速編寫和更新彈性的 JSON API 以利支援任何應用程式。

## 內容服務教學課程

端對端教學課程，說明如何在無周邊 CMS 情境下使用 AEM 建置和公開內容，並供原生行動應用程式使用。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

本教學課程探討如何使用 AEM 內容服務來增強行動應用程式的體驗，讓行動應用程式顯示由 WKND 團隊策劃的事件資訊 (音樂、表演、藝術等)。

本教學課程會涵蓋以下主題：

* 使用內容片段建立代表事件的內容
* 使用 AEM Sites 的範本和頁面定義 AEM 內容服務端點，而這些範本和頁面以 JSON 格式公開事件資料
* 探索如何使用 AEM WCM 核心元件讓行銷人員能夠製作 JSON 端點
* 從行動應用程式使用 AEM 內容服務 JSON
   * 使用 Android 是因為其具備跨平台模擬器，本教學課程的所有使用者 (Windows、macOS 和 Linux) 都可以使用該模擬器來執行原生應用程式。

## GitHub 專案

來源程式碼和內容套件可以在 [AEM Guides：WKND 行動 GitHub 專案](https://github.com/adobe/aem-guides-wknd-mobile)中找到。

若您發現本教學課程或程式碼有問題，請留下 [GitHub 問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 比較 AEM GraphQL 與 AEM 內容服務

|                                | AEM GraphQL API | AEM 內容服務 |
|--------------------------------|:-----------------|:---------------------|
| 結構描述定義 | 結構化內容片段模型 | AEM 元件 |
| 內容 | 內容片段 | AEM 元件 |
| 內容探索 | 透過 GraphQL 查詢 | 透過 AEM 頁面 |
| 傳遞格式 | GraphQL JSON | AEM ComponentExporter JSON |
