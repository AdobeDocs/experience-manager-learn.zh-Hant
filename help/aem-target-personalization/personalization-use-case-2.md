---
title: 使用Adobe Target進行個人化
seo-title: 使用Adobe Target進行個人化
description: 端對端教學課程，說明如何使用Adobe Target建立和提供個人化體驗。
seo-description: 端對端教學課程，說明如何使用Adobe Target建立和提供個人化體驗。
feature: 體驗片段
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# 使用Adobe Target個人化完整網頁體驗

在上一章中，我們學習了如何使用以體驗片段形式建立並從AEM匯出為HTML選件的內容，在Adobe Target中建立以地理位置為基礎的活動。

在本章中，我們將探討如何建立活動，使用Adobe Target將AEM上托管的網站頁面重新導向至新頁面。

## 藍本概述

WKND網站重新設計了首頁，並想要將其目前的首頁訪客重新導向至新的首頁。 同時，也能了解重新設計的首頁如何協助改善使用者參與度和收入。 身為行銷人員，您已獲指派工作來建立活動，以將訪客重新導向至新首頁。 讓我們探索WKND網站首頁，了解如何使用Adobe Target建立活動。

### 相關使用者

在本練習中，需要涉及以下用戶，並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **行銷人員** (Adobe Target/最佳化團隊)

### WKND站點首頁

![AEM Target案例1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 必備條件

* **AEM**
   * [AEM作者和](./implementation.md#getting-aem) 發佈instancerunning分別在localhost 4502和4503上。
   * [AEM使用Adobe Experience Platform Launch與Adobe Target整合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 內容編輯器活動

1. 行銷人員會與AEM內容編輯器起始WKND首頁重新設計討論，並詳細說明需求。
   * ***要求*** :重新設計WKND網站首頁，採用卡式設計。
2. 接著，AEM內容編輯器會根據需求，以卡片式設計建立新的WKND網站首頁，並發佈新的首頁。

## 行銷人員活動

1. 行銷人員會以重新導向選件作為體驗來建立A/B目標活動，並將100%的網站流量分配給新的首頁，並新增成功目標和量度。
   1. 從Adobe Target視窗，導覽至&#x200B;**Activities**&#x200B;標籤。
   2. 按一下「建立活動&#x200B;**」按鈕，然後選取活動類型為** A/B測試&#x200B;****

      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選取&#x200B;**Web**&#x200B;通道，然後選擇&#x200B;**可視化體驗撰寫器**。
   4. 輸入&#x200B;**活動URL**&#x200B;並按一下&#x200B;**下一步**以開啟可視化體驗撰寫器。
      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 若要載入&#x200B;**可視化體驗撰寫器**，請啟用在瀏覽器上允許載入不安全指令碼&#x200B;**並重新載入您的頁面。**
      ![體驗鎖定目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意WKND網站首頁已在可視化體驗撰寫器編輯器中開啟。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 將游標暫留在&#x200B;**體驗B**上，然後選取「檢視其他選項」。
      ![體驗 B](assets/personalization-use-case-2/redirect-url.png)
   8. 選擇「**重定向到URL**」選項，然後輸入指向新WKND首頁的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![體驗 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** 儲存您的變更，並繼續進行活動建立的後續步驟。
   10. 選擇&#x200B;**流量分配方法**&#x200B;作為手動，並將100%流量分配給&#x200B;**體驗B**。
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   11. 按一下&#x200B;**下一步**。
   12. 為活動提供&#x200B;**目標量度**並儲存及關閉A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   13. 為您的活動提供名稱（**WKND首頁重新設計**），並儲存您的變更。
   14. 在「活動詳細資訊」畫面中，確定&#x200B;**啟動**您的活動。
      ![啟動活動](assets/personalization-use-case-2/ab-activate.png)
   15. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，系統會將您重新導向至重新設計的WKND網站首頁(http://localhost:4503/content/wknd/en1.html)。
      ![WKND首頁重新設計](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，行銷人員可建立活動，使用Adobe Target將AEM上托管的網站頁面重新導向至新頁面。
