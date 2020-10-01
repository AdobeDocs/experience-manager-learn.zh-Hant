---
title: 使用Visual Experience Composer進行個人化
description: 瞭解如何使用Visual Experience Composer建立Adobe Target活動。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# 使用Visual Experience Composer進行個人化 {#personalization-vec}

瞭解如何使用視覺體驗撰寫器(VEC)建立A/B測試目標活動。


## 藍本概觀

WKND網站首頁以資訊卡的形式顯示城市周邊的本地活動或最佳動作。 身為行銷人員，您已獲指派修改首頁的工作，方法是對冒險區摘要進行文字變更，並瞭解它如何改善轉換。

## 使用Visual Experience Composer(VEC)建立A/B測試的步驟

1. 登入Adobe Target並導覽至「活動」標籤
2. 按一 **下「建立活動** 」按鈕，然後選擇 **A/B測試活動** 。

   ![A/B活動](assets/ab-target-activity.png)

3. 選取「 **Visual Experience Composer** 」選項、提供「活動URL」，然後按「下一 **步」**

   ![活動URL](assets/ab-test-url.png)

4. 在您建立新活動後，Visual Experience Composer會在左側顯示兩個標籤： *體驗A**和B*。 從清單中選取體驗。 您可以使用「新增體驗」按鈕，將新的體驗新 **增至清單** 。

   ![體驗A](assets/experience.png)

5. 在頁面上選取影像或文字以開始進行修改，或使用程式碼編輯器來挑選和HTML元素。

   ![元素](assets/select-element.png)

6. 將文字從 *Camping in Western Australia變更為**Adventures of Australia*。 新增至「體驗」的變更清單會顯示在「修改」下。 您可以按一下並編輯已修改的項目，以檢視其CSS選取器和新增的內容。

   ![冒險](assets/adventures.png)

7. 將體 *驗A重新命名* 為 *Adventure*
8. 同樣地，請更新 *Camping in Western Australia的* Experience B *，以探* 索Australian Wilderness **。

   ![探索](assets/explore.png)

9. 按一 **下** 「下一步」移至「定位」，讓我們在兩個體驗之間將手動流量分配為50-50。

   ![定位](assets/targeting.png)

10. 對於「目標」和設定，選擇「報告來源」作為Adobe Target，並選取「目標」量度作為「轉換」，並執行頁面檢視動作。

   ![目標](assets/goals.png)

11. 為活動提供名稱並儲存。
12. 啟動您儲存的活動，即時推送您的變更。

   ![目標](assets/activate.png)

13. 在新標籤中開啟您的網站頁面（步驟3中的活動URL），您就可以從我們的A/B測試活動中檢視其中一種體驗（冒險或探索）。

   ![目標](assets/publish.png)

## 摘要

在本章中，行銷人員可以使用Visual Experience Composer來建立體驗，方法是拖放、交換和修改網頁的版面配置和內容，而不需變更任何程式碼來執行測試。

## 支援連結

* [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)