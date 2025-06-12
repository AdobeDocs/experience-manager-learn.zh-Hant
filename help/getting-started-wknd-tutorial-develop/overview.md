---
title: AEM Sites 快速入門 - WKND 教學課程
description: 了解如何為虛構生活風格品牌 WKND 實施 AEM 網站。透過逐步解說，了解 Experience Manager 的基本主題，例如專案設定、maven 原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 100%

---

# AEM Sites 快速入門 - WKND 教學課程 {#introduction}

{{traditional-aem}}

歡迎使用專為初次接觸 Adobe Experience Manager (AEM) 的開發人員設計的多部分教學課程。此教學課程會逐步解說為虛構生活風格品牌 WKND 實施 AEM 網站的情形。此教學課程涵蓋基礎主題，例如專案設定、核心元件、可編輯的範本、用戶端程式庫，以及使用 Adobe Experience Manager Sites 進行元件開發。

## 概觀 {#wknd-tutorial-overview}

這個由多個部分組成的教學課程的目標是指導開發人員，如何使用 Adobe Experience Manager (AEM) 中最新的標準和技術來實施一個網站。完成本教學課程後，開發人員應能了解平台的基礎知識和 AEM 中常見的設計模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 開始 Sites 專案的選項

啟動 AEM Sites 專案有兩種基本方法。

**AEM 專案原型** - 使用 Maven 範本產生最小 AEM 專案來進行 AEM 開發的傳統方法。對於需要大量自訂的 AEM 6.5/6.4 專案和 AEM as a Cloud Service 專案，建議採用此方法。本教學課程深入介紹 AEM 開發。

[使用 AEM 專案原型開始教學課程](./project-archetype/overview.md)

**AEM 網站範本**：也稱為快速網站建立，是使用預先定義的網站範本產生 AEM 網站的少量程式碼方法。使用現成可用的元件和範本，快速建立並執行網站。使用主題工作流程，僅使用 CSS 和 JavaScript 來套用品牌特定的樣式和自訂內容。推薦新專案和開發人員採用。僅適用於 AEM as a Cloud Service。

[使用網站範本開始教學課程](./site-template/create-site.md)

## Adobe XD 使用者介面套件

為了讓本教學課程更貼近真實情境，Adobe 才華洋溢的 UX 設計師使用 [Adobe XD](https://www.adobe.com/tw/products/xd.html) 為網站建立模型。在本教學課程的過程中，我們會將實作設計的各個部分，使其成為一個完全可以製作的 AEM 網站。特別感謝 **Lorenzo Buosi** 和 **Kilian Amendola** 為 WKND 網站創造精美的設計。

下載 XD 使用者介面套件：

* [AEM 核心元件使用者介面套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND 使用者介面套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 參照網站 {#reference-site}

亦提供 WKND 網站的完成版本供您參考：[https://wknd.site/](https://wknd.site/)

本教學課程涵蓋 AEM 開發人員所需的主要開發技能，但&#x200B;*不會*&#x200B;從頭到尾建立整個網站。已完成的參考網站是探索和了解更多 AEM 現成可用功能的另一個重要資源。

若要在進入教學課程之前測試最新程式碼，請下載和安裝 **[GitHub 提供的最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**。

### 由 Adobe Stock 支援

WKND 參考網站中的許多影像來自 [Adobe Stock](https://stock.adobe.com/)，並屬於示範資產附帶條款 (參閱 [https://www.adobe.com/tw/legal/terms.html](https://www.adobe.com/tw/legal/terms.html)) 中定義的第三方素材。除了在這個示範網站檢視之外，如果您想要使用 Adobe Stock 影像作其他用途，例如在網站上展示或在行銷素材中運用，則可以在 Adobe Stock 上購買授權。

透過 Adobe Stock，您可以存取超過 1.4 億項高品質、免版稅的影像，包括照片、圖形、影片和範本，能快速展開您的創意專案。

## 後續步驟 {#next-steps}

還在等什麼？！了解如何[使用 AEM 專案原型產生新的 Adobe Experience Manager 專案](./project-archetype/overview.md)或[使用網站範本建立網站](./site-template/create-site.md)。
