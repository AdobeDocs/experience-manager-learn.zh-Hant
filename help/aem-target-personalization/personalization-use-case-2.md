---
title: 使用Adobe Target個性化
seo-title: Personalization using Adobe Target
description: 一個端到端教程，介紹如何使用Adobe Target建立和提供個性化體驗。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# 利用Adobe Target實現全網頁體驗個性化

在上一章中，我們學習了如何在Adobe Target內使用建立為「經驗片段」並從中導出為「HTML優惠」的內容建立基於AEM地理位置的活動。

在本章中，我們將探討建立活動，以使用Adobe Target將托管在上的網AEM站頁重定向到新頁面。

## 方案概述

WKND網站重新設計其首頁，並希望將其當前首頁訪問者重定向到新首頁。 同時，還要瞭解重新設計的首頁如何幫助改善用戶參與和收入。 作為營銷人員，您已分配了建立活動以將訪問者重定向到新首頁的任務。 讓我們瀏覽WKND網站首頁並瞭解如何使用Adobe Target建立活動。

### 涉及的用戶

在本練習中，需要讓以下用戶參與並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **營銷商** (Adobe Target/優化團隊)

### WKND網站首頁

![目標AEM方案1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 必備條件

* **AEM**
   * [AEM作者和發佈實例](./implementation.md#getting-aem) 分別運行在localhost 4502和4503上。
   * [利AEM用Adobe Experience Platform Launch與Adobe Target](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 內容編輯器活動

1. Marketer將啟動與內容編輯器的WKND首頁重新設計討AEM論，並詳細說明要求。
   * ***要求*** :重新設計WKND網站首頁，採用基於卡的設計。
2. 然後，根據AEM這些要求，內容編輯器建立一個新的WKND網站首頁，其設計基於卡，並發佈新首頁。

## 營銷商活動

1. Marketer建立A/B目標活動，將重定向優惠作為「體驗」，並在添加成功目標和指標後，將100%的網站流量分配給新首頁。
   1. 從您的Adobe Target窗口，導航到 **活動** 頁籤。
   2. 按一下 **建立活動** 按鈕，然後選擇活動類型為 **A/BTest**

      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選擇 **Web** 選擇 **視覺體驗作曲家**。
   4. 輸入 **活動URL** 按一下 **下一個** 開啟視覺體驗作曲家。
      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 對於 **視覺體驗作曲家** 載入，啟用 **允許載入不安全指令碼** 重新載入頁面。
      ![體驗目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意在Visual Experience Composer編輯器中開啟的WKND網站首頁。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 懸停於 **經驗B** 並選擇「查看其他選項」。
      ![體驗 B](assets/personalization-use-case-2/redirect-url.png)
   8. 選擇 **重定向至URL** 的子菜單。 (http://localhost:4503/content/wknd/en1.html)
      ![體驗 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保存** 繼續執行「活動建立」的後續步驟。
   10. 選擇 **流量分配方法** 手動並允許100%的流量 **經驗B**。
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   11. 按一下&#x200B;**下一步**。
   12. 提供 **目標度量** 「Activity（活動）」和「Save（保存）」並關閉A/BTest。
      ![A/BTest目標度量](assets/personalization-use-case-2/goal-metric.png)
   13. 提供名稱(**WKND首頁重新設計**)，並保存更改。
   14. 在「活動詳細資訊」螢幕中，確保 **激活** 您的活動。
      ![激活活動](assets/personalization-use-case-2/ab-activate.png)
   15. 導航到WKND首頁(http://localhost:4503/content/wknd/en.html)，您將被重定向到重新設計的WKND網站首頁(http://localhost:4503/content/wknd/en1.html)。
      ![重新設計WKND首頁](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，商家能夠建立活動以使用Adobe Target將托管在上的網站頁AEM重定向到新頁面。
