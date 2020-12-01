---
title: 使用Adobe Target進行個人化
seo-title: 使用Adobe Target進行個人化
description: 教學課程的端對端說明如何使用Adobe Target建立和提供個人化體驗。
seo-description: 教學課程的端對端說明如何使用Adobe Target建立和提供個人化體驗。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---


# 使用Adobe Target個人化完整網頁體驗

在上一章中，我們學習如何使用以「體驗片段」建立並從AEM匯出為「HTML選件」的內容，在Adobe Target中建立以地理位置為基礎的活動。

在本章中，我們將探索建立活動，以使用Adobe Target將AEM上代管的網站頁面重新導向至新頁面。

## 藍本概觀

WKND網站重新設計其首頁，並想要將目前的首頁訪客重新導向至新首頁。 同時，也瞭解重新設計的首頁如何協助改善使用者參與度和收入。 身為行銷人員，您已獲指派工作來建立活動，將訪客重新導向至新首頁。 讓我們探索WKND網站首頁，並瞭解如何使用Adobe Target建立活動。

### 相關使用者

在本練習中，需要有下列用戶參與，並執行一些可能需要管理訪問權限的任務。

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **行銷人員** （Adobe Target /最佳化團隊）

### WKND網站首頁

![AEM Target藍本1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 必備條件

* **AEM**
   * [AEM作者和發佈](./implementation.md#getting-aem) 例項取消分別位於localhost 4502和4503。
   * [AEM使用Adobe Experience Platform Launch與Adobe Target整合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建下列解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

## 內容編輯器活動

1. 行銷人員會與AEM內容編輯器開始WKND首頁重新設計討論，並詳細說明需求。
   * ***要求*** :重新設計WKND網站首頁，並提供卡片設計。
2. 然後，AEM Content Editor會根據需求建立新的WKND網站首頁，並以卡片為基礎進行設計，並發佈新的首頁。

## 行銷人員活動

1. 行銷人員會建立A/B目標活動，將重新導向選件當做體驗，並在新增成功目標和量度後，將100%的網站流量分配給新首頁。
   1. 從Adobe Target視窗，導覽至&#x200B;**活動**&#x200B;標籤。
   2. 按一下「建立活動」按鈕，並選取活動類型為「**A/B測試」******

      ![Adobe Target —— 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選擇&#x200B;**Web**&#x200B;頻道，然後選擇&#x200B;**Visual Experience Composer**。
   4. 輸入&#x200B;**活動URL**，然後按一下&#x200B;**下一步**以開啟Visual Experience Composer。
      ![Adobe Target —— 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 若要載入&#x200B;**Visual Experience Composer**，請啟用瀏覽器上的「允許載入不安全指令碼&#x200B;**」並重新載入頁面。**
      ![體驗定位活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意，WKND網站首頁在Visual Experience Composer編輯器中開啟。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 將滑鼠指標暫留在「體驗B **」上，並選取「檢視其他選項」。**
      ![體驗 B](assets/personalization-use-case-2/redirect-url.png)
   8. 選擇「**重新導向至URL**」選項，然後輸入新WKND首頁的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![體驗 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保** 存您的更改並繼續「活動建立」的後續步驟。
   10. 選擇&#x200B;**流量分配方法**&#x200B;作為手動流量，將100%流量分配給&#x200B;**體驗B**。
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   11. 按一下&#x200B;**下一步**。
   12. 為活動提供&#x200B;**目標量度**，並儲存並關閉A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   13. 為活動提供名稱（**WKND首頁重新設計**），並儲存您的變更。
   14. 在「活動詳細資訊」螢幕中，確保&#x200B;**激活**您的活動。
      ![啟動活動](assets/personalization-use-case-2/ab-activate.png)
   15. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，您將被重新導向至重新設計的WKND網站首頁(http://localhost:4503/content/wknd/en1.html)。
      ![WKND首頁重新設計](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，行銷人員可以建立活動，將AEM上裝載的網站頁面重新導向至使用Adobe Target的新頁面。
