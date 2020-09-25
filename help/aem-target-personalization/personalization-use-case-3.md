---
title: 使用Adobe Target Visual Experience Composer進行個人化
seo-title: 使用Adobe Target Visual Experience Composer(VEC)進行個人化
description: 教學課程的端對端說明如何使用Adobe Target Visual Experience Composer(VEC)建立和提供個人化體驗。
seo-description: 教學課程的端對端說明如何使用Adobe Target Visual Experience Composer(VEC)建立和提供個人化體驗。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# 使用Visual Experience Composer進行個人化

在本章中，我們將探索如何使用 **Visual Experience Composer** ，從Target中拖放、交換和修改網頁的版面配置和內容來建立體驗。

## 藍本概觀

WKND網站首頁以卡片版面的形式顯示城市周邊的當地活動或最佳工作。 身為行銷人員，您已獲得指派工作來修改首頁，方法是重新排列卡片版面，以瞭解它如何影響使用者參與度並推動轉換。

### 相關使用者

在本練習中，需要有下列用戶參與，並執行一些可能需要管理訪問權限的任務。

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **行銷人員** （Adobe Target /最佳化團隊）

### WKND網站首頁

![AEM Target藍本1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 必備條件

* **AEM**
   * [AEM發佈例項](./implementation.md#getting-aem) （執行於4503）
   * [AEM使用Adobe Experience Platform Launch與Adobe Target整合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建 [Adobe Target](https://experiencecloud.adobe.com)

## 行銷人員活動

1. 行銷人員會在Adobe Target中建立A/B目標活動。
   1. 從Adobe Target視窗，導覽至「活 **動** 」標籤。
   2. 按一 **下「建立活動** 」按鈕，並選取活動類型 **為A/B測試**

      ![Adobe Target —— 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選取網 **路頻道** ，然後選擇 **視覺體驗撰寫器**。
   4. 輸入「 **活動URL** 」並按一 **下「下一步** 」以開啟「視覺體驗撰寫器」。
      ![Adobe Target —— 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 若要 **載入Visual Experience Composer** ，請啟用 **在瀏覽器上允許載入不安全指令碼** ，然後重新載入頁面。
      ![體驗定位活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意，WKND網站首頁在Visual Experience Composer編輯器中開啟。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **體驗A** 提供預設的WKND首頁，讓我們編輯體驗 **B的內容版面**。
      ![體驗 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 按一下其中一個卡片版面容器(*Best Roasters*)，然後選取「 **** 重新排列」選項。
      ![容器選擇](assets/personalization-use-case-3/container-selection.png)
   9. 按一下您要重新排列的容器，並拖放至所需位置。 讓我們將 *Best Roasters* （最佳烘焙器）容器從第1列第1列重新排列為第1列第3列。 現在， *Best Roasters* （最佳烘焙機）容器將緊鄰 *攝影展* 。
      ![容器交換](assets/personalization-use-case-3/container-swap.png)

      **交換後**
      ![已交換容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同樣地，重新排列其他卡片容器的位置。
      ![已交換容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 我們也可以在轉盤元件下方和卡片版面上方新增標題文字。
   12. 按一下轉盤容器，然後選取「在 **後面插入> HTML** 」選項以新增HTML。
      ![新增文字](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![新增文字](assets/personalization-use-case-3/after-changes.png)
   13. 按一 **下「下** 一步」繼續您的活動。
   14. 選擇流 **量分配方法** ，將流量分配給 **體驗B**。
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   15. 按一下&#x200B;**下一步**。
   16. 提供 **活動的目標量度** ，並儲存及關閉您的A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   17. 為活動提供名&#x200B;**稱(WKND首頁重新整理**)，並儲存您的變更。
   18. 在「活動詳細資訊」畫面中，請確定「啟 **動** 」活動。
      ![啟動活動](assets/personalization-use-case-3/save-activity.png)
   19. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，您會注意到我們新增至WKND首頁重新整理A/B測試活動的變更。
      ![已重新整理WKND首頁](assets/personalization-use-case-3/activity-result.png)
   20. 開啟您的瀏覽器主控台，並檢查網路標籤，以尋找WKND首頁重新整理A/B測試活動的目標回應。
      ![網路活動](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，行銷人員可以使用Visual Experience Composer來建立體驗，方法是拖放、交換和修改網頁的版面配置和內容，而不需變更任何程式碼來執行測試。
