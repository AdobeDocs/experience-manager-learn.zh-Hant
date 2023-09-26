---
title: 開始使用AEM Sites - WKND教學課程
description: 瞭解如何為名為WKND的虛構生活風格品牌實作AEM網站。 取得基本Experience Manager主題的逐步解說，例如專案設定、maven原型、核心元件、可編輯的範本、使用者端資料庫和元件開發。
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 5%

---

# 開始使用AEM Sites - WKND教學課程 {#introduction}

{{edge-delivery-services}}

歡迎使用專為Adobe Experience Manager (AEM)的新手開發人員設計的多部分教學課程。 此教學課程會逐步引導您為虛擬生活風格品牌WKND實作AEM網站。 此教學課程涵蓋基礎的主題，例如專案設定、核心元件、可編輯的範本、用戶端資料庫以及使用 Adobe Experience Manager Sites 的元件開發。

## 概觀 {#wknd-tutorial-overview}

此多部分教學課程的目標是教導開發人員如何使用Adobe Experience Manager (AEM)中的最新標準和技術來實作網站。 完成本教學課程後，開發人員應瞭解平台的基本基礎和AEM中的常見設計模式。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## 啟動Sites專案的選項

啟動AEM Sites專案有兩種基本方法。

**AEM專案原型**  — 傳統的AEM開發方法，方法是使用Maven範本產生最小的AEM專案。 這是針對AEM 6.5/6.4專案和AEMas a Cloud Service專案建議的方法，可預期大量自訂。 此教學課程提供對AEM開發的更深入探討。

[以AEM專案原型開始教學課程](./project-archetype/overview.md)

**AEM網站範本**  — 也稱為「快速網站建立」，這是一種使用預先定義的網站範本產生AEM網站的低程式碼方法。 使用現成可用的元件和範本，讓網站快速上線運作。 使用主題化工作流程，僅以CSS和JavaScript套用品牌特定樣式和自訂。 建議新專案和開發人員使用。 僅適用於AEMas a Cloud Service。

[使用網站範本開始教學課程](./site-template/create-site.md)

## Adobe XD UI套件

為了讓本教學課程更接近真實案例Adobe的天才UX設計人員使用為網站建立模型 [Adobe XD](https://www.adobe.com/products/xd.html). 在教學課程中，各種設計片段皆實施至可完整撰寫的AEM網站。 特別感謝 **洛倫佐·布西** 和 **基利安·阿門多拉** 他創造了WKND網站的美觀設計。

下載XD UI套件：

* [AEM核心元件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI套件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 引用網站 {#reference-site}

WKND網站的完成版本也可作為參考使用： [https://wknd.site/](https://wknd.site/)

此教學課程涵蓋AEM開發人員所需的主要開發技能，但將 *非* 端對端建置整個網站。 完成的參考網站是另一個探索和檢視更多AEM立即可用功能的絕佳資源。

若要在跳至教學課程之前測試最新的程式碼，請下載並安裝 **[GitHub最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### 由Adobe Stock提供

WKND參考網站中的許多影像來自 [Adobe Stock](https://stock.adobe.com/) 和是示範資產中定義的第三方資料，其他條款位於 [https://www.adobe.com/legal/terms.html](https://www.adobe.com/tw/legal/terms.html). 如果您想將Adobe Stock影像用於檢視此示範網站以外的其他用途，例如於網站上或行銷資料中展示，您可以購買Adobe Stock上的授權。

透過Adobe Stock，您可以存取超過1.4億張高品質、免版稅的影像，包括像片、圖片、影片和範本，以快速啟動您的創意專案。

## 後續步驟 {#next-steps}

您還在等什麼?!瞭解如何 [使用AEM專案原型產生新的Adobe Experience Manager專案](./project-archetype/overview.md) 或 [使用網站範本建立網站](./site-template/create-site.md).
