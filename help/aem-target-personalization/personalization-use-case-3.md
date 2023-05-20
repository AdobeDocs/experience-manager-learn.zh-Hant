---
title: 使用Adobe TargetVisual Experience Composer個性化
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: 一個端到端教程，介紹如何使用Adobe TargetVisual Experience Composer(VEC)建立和提供個性化體驗。
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

# 使用Visual Experience Composer實現個性化

在本章中，我們將探討使用 **視覺體驗作曲家** 通過從「目標」中拖放、交換和修改網頁的佈局和內容。

## 方案概述

WKND網站首頁以卡佈局的形式顯示城市周圍的本地活動或最佳活動。 作為營銷人員，您已分配了修改首頁的任務，方法是重新安排卡佈局，以查看它對用戶參與和驅動器轉換的影響。

### 涉及的用戶

在本練習中，需要讓以下用戶參與並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **營銷商** (Adobe Target/優化團隊)

### WKND網站首頁

![目標AEM方案1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 必備條件

* **AEM**
   * [AEM發佈實例](./implementation.md#getting-aem) 運行於4503
   * [利AEM用Adobe Experience Platform Launch與Adobe Target](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud [Adobe Target](https://experiencecloud.adobe.com)

## 營銷商活動

1. Marketer在Adobe Target內建立A/B目標活動。
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
   7. **體驗A** 提供預設的WKND首頁，讓我們編輯 **經驗B**。
      ![體驗 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 按一下其中一個卡佈局容器(*貝斯特斯*) **重新排列** 的雙曲餘切值。
      ![容器選擇](assets/personalization-use-case-3/container-selection.png)
   9. 按一下要重新排列的容器，並將其拖放到所需位置。 讓我們重新安排 *貝斯特斯* 容器從第1行第1列到第1行第3列。 現在 *貝斯特斯* 容器旁邊 *攝影展* 容器。
      ![容器交換](assets/personalization-use-case-3/container-swap.png)

      **交換後**
      ![已交換容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同樣，重新排列其它卡容器的位置。
      ![已交換容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 我們還要在傳送器元件下和卡佈局上方添加標題文本。
   12. 按一下旋轉選取容器並選擇 **在後設定>HTML** 的子菜單。
      ![新增文字](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![新增文字](assets/personalization-use-case-3/after-changes.png)
   13. 按一下 **下一個** 繼續您的活動。
   14. 選擇 **流量分配方法** 手動並允許100%的流量 **經驗B**。
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   15. 按一下&#x200B;**下一步**。
   16. 提供 **目標度量** 「Activity（活動）」和「Save（保存）」並關閉A/BTest。
      ![A/BTest目標度量](assets/personalization-use-case-2/goal-metric.png)
   17. 提供名稱(**WKND首頁刷新**)，並保存更改。
   18. 在「活動詳細資訊」螢幕中，確保 **激活** 您的活動。
      ![激活活動](assets/personalization-use-case-3/save-activity.png)
   19. 導航到WKND首頁(http://localhost:4503/content/wknd/en.html)，您會注意到我們添加到WKND首頁刷新A/BTest活動中的更改。
      ![已刷新WKND首頁](assets/personalization-use-case-3/activity-result.png)
   20. 開啟瀏覽器控制台，並檢查網路頁籤以查找WKND首頁刷新A/BTest活動的目標響應。
      ![網路活動](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，商家通過拖放、交換和修改網頁的佈局和內容而無需更改任何代碼來運行test，從而能夠使用Visual Experience Composer建立體驗。
