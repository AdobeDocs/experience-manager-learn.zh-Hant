---
title: 使用Adobe Target可視化體驗撰寫器進行個人化
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: 端對端教學課程，說明如何使用Adobe Target可視化體驗撰寫器(VEC)建立和提供個人化體驗。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# 使用可視化體驗撰寫器進行個人化

在本章中，我們將探索如何使用 **可視化體驗撰寫器** 從Target內拖放、交換及修改網頁的版面和內容。

## 藍本概述

WKND網站首頁以卡片配置形式顯示城市周圍的本地活動或最佳活動。 身為行銷人員，您已獲派修改首頁的工作，方法是重新排列卡片配置，以了解它如何影響使用者參與並促進轉換。

### 相關使用者

在本練習中，需要涉及以下用戶，並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **行銷人員** (Adobe Target/最佳化團隊)

### WKND站點首頁

![AEM Target案例1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 必備條件

* **AEM**
   * [AEM發佈例項](./implementation.md#getting-aem) 在4503年運行
   * [AEM使用Adobe Experience Platform Launch與Adobe Target整合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud布建為 [Adobe Target](https://experiencecloud.adobe.com)

## 行銷人員活動

1. 行銷人員在Adobe Target內建立A/B目標活動。
   1. 從Adobe Target視窗，導覽至 **活動** 標籤。
   2. 按一下 **建立活動** 按鈕，然後選取活動類型為 **A/B測試**

      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選取 **Web** 通道，並選擇 **可視化體驗撰寫器**.
   4. 輸入 **活動URL** 和按一下 **下一個** 以開啟可視化體驗撰寫器。
      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 針對 **可視化體驗撰寫器** 要載入，請啟用 **允許載入不安全指令碼** 並重新載入頁面。
      ![體驗鎖定目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意WKND網站首頁已在可視化體驗撰寫器編輯器中開啟。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **體驗A** 提供預設的WKND首頁，並編輯 **體驗B**.
      ![體驗 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 按一下其中一個卡片版面容器(*最佳烘焙機*)，然後選取 **重新排列** 選項。
      ![容器選擇](assets/personalization-use-case-3/container-selection.png)
   9. 按一下您要重新排列的容器，並將其拖放至所需位置。 讓我們重新安排 *最佳烘焙機* 容器，從第1行第1列到第1行第3列。 現在 *最佳烘焙機* 容器旁邊 *攝影展覽* 容器。
      ![容器交換](assets/personalization-use-case-3/container-swap.png)

      **交換後**
      ![交換的容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同樣地，重新排列其他卡片容器的位置。
      ![交換的容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 我們也會在轉盤元件下方和卡片版面上方新增標題文字。
   12. 按一下轉盤容器，然後選取 **Inset After >HTML** 選項來新增HTML。
      ![新增文字](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![新增文字](assets/personalization-use-case-3/after-changes.png)
   13. 按一下 **下一個** 繼續您的活動。
   14. 選取 **流量分配方法** 手動分配100%流量至 **體驗B**.
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   15. 按一下&#x200B;**下一步**。
   16. 提供 **目標量度** 儲存並關閉A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   17. 提供名稱(**WKND首頁刷新**)，並儲存您的變更。
   18. 在「活動詳細資訊」畫面中，請務必 **啟動** 您的活動。
      ![啟動活動](assets/personalization-use-case-3/save-activity.png)
   19. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，您就會注意到我們新增至WKND首頁重新整理A/B測試活動的變更。
      ![已刷新WKND首頁](assets/personalization-use-case-3/activity-result.png)
   20. 開啟瀏覽器主控台，並檢查網路標籤，以尋找WKND首頁重新整理A/B測試活動的目標回應。
      ![網路活動](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，行銷人員可以透過拖放、切換及修改網頁的版面與內容，使用可視化體驗撰寫器來建立體驗，而不需變更要執行測試的任何代碼。
