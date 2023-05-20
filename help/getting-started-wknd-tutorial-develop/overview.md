---
title: 開始使用AEM Sites- WKND教程
description: 瞭解如何為名為WKNDAEM的虛擬生活方式品牌實施網站。 逐步瞭解基本Experience Manager主題，如項目設定、主要原型、核心元件、可編輯模板、客戶端庫和元件開發。
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 5%

---

# 開始使用AEM Sites- WKND教程 {#introduction}

歡迎學習為Adobe Experience Manager()新開發人員設計的多部分教AEM程。 本教程將介紹虛擬生AEM活品牌WKND的網站實施。 此教學課程涵蓋基礎的主題，例如專案設定、核心元件、可編輯的範本、用戶端資料庫以及使用 Adobe Experience Manager Sites 的元件開發。

## 概觀 {#wknd-tutorial-overview}

本多部分教程的目標是教開發人員如何使用Adobe Experience Manager(AEM)的最新標準和技術實施網站。 完成本教程後，開發人員應瞭解中平台的基本基礎和常見設計模式AEM。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 啟動站點項目的選項

啟動一個AEM Sites項目有兩種基本方法。

**項AEM目原型**  — 採用Maven模AEM板生成最小AEM項目的傳統開發方法。 這是預期大量定製的項AEM目和AEMas a Cloud Service項目的推薦方法。 本教程將更深入地瞭解開AEM發。

[使用項目原型啟AEM動本教程](./project-archetype/overview.md)

**站AEM點模板**  — 也稱為快速站點建立，是一種使用預定義站點模板AEM來生成站點的低代碼方法。 使用現成元件和模板快速啟動和運行站點。 使用主題工作流僅使用CSS和JavaScript應用特定於品牌的樣式和自定義。 推薦用於新項目和開發人員。 僅可用於AEMas a Cloud Service。

[使用站點模板啟動教程](./site-template/create-site.md)

## Adobe XDUI套件

為了使本教程更接近真實場景，Adobe有才華的UX設計師使用 [Adobe XD](https://www.adobe.com/products/xd.html)。 在本教程中，各種設計都實現到完全可作者的站AEM點。 特別感謝 **洛倫佐布奧西** 和 **基利安·阿門多拉** 為WKND網站創造了美麗的設計。

下載XDUI工具包：

* [核AEM心元件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 引用網站 {#reference-site}

WKND站點的完成版本也可作為參考： [https://wknd.site/](https://wknd.site/)

本教程介紹開發人員所需的主要開發技AEM能，但 *不* 將整個站點端到端地構建。 完成的參考站點是另一個極好的資源，可以瀏覽和查看AEM更多現成的功能。

要在跳入教程之前test最新代碼，請下載並安裝 **[GitHub最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**。

### 由Adobe Stock提供

WKND參考網站中的許多影像來自 [Adobe Stock](https://stock.adobe.com/) 是第三方材料，如演示資產附加條款所定義， [https://www.adobe.com/legal/terms.html](https://www.adobe.com/tw/legal/terms.html)。 如果您想將Adobe Stock影像用於除查看此演示網站之外的其他目的，例如在網站或營銷材料中顯示該影像，您可以在Adobe Stock購買許可證。

借助Adobe Stock，您可以訪問超過1.4億張高質量、免版稅的影像，包括照片、圖形、視頻和模板，以快速啟動您的創意項目。

## 後續步驟 {#next-steps}

你在等什麼！瞭解如何 [用原型項目生成新的Adobe Experience ManagerAEM項目](./project-archetype/overview.md) 或 [使用站點模板建立站點](./site-template/create-site.md)。
