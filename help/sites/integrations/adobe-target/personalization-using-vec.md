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
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---


# 使用Visual Experience Composer進行個人化 {#personalization-vec}

瞭解如何使用視覺體驗撰寫器(VEC)建立A/B測試目標活動。

## 必備條件

若要在AEM網站上使用VEC，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 藍本概觀

WKND網站首頁以資訊卡的形式顯示城市周邊的本地活動或最佳動作。 身為行銷人員，您已獲指派修改首頁的工作，方法是對冒險區摘要進行文字變更，並瞭解它如何改善轉換。

## 使用Visual Experience Composer(VEC)建立A/B測試的步驟

1. 登入 [Adobe Experience Cloud](https://experience.adobe.com/)、點選 __Target__、導覽至「活 __動__ 」標籤

   + 如果您在Experience Cloud儀表板上未看到 __Target__ ，請確定在右上角的組織切換器中選取了正確的Adobe組織，且您的使用者已獲得 [Adobe Admin Console中Target的存取權](https://adminconsole.adobe.com/)。

1. 按一 **下「建立活動** 」按鈕，然後選擇 **A/B測試活動** 。

   ![A/B活動](assets/ab-target-activity.png)

1. 選取「 **Visual Experience Composer** 」選項、提供「活動URL」，然後按「下一 **步」**

   ![活動URL](assets/ab-test-url.png)

1. 在您建立新活動後，Visual Experience Composer會在左側顯示兩個標籤： *體驗A**和B*。 從清單中選取體驗。 您可以使用「新增體驗」按鈕，將新的體驗新 **增至清單** 。

   ![體驗A](assets/experience.png)

1. 在頁面上選取影像或文字以開始進行修改，或使用程式碼編輯器來挑選和HTML元素。

   ![元素](assets/select-element.png)

1. 將文字從 *Camping in Western Australia變更為**Adventures of Australia*。 新增至「體驗」的變更清單會顯示在「修改」下。 您可以按一下並編輯已修改的項目，以檢視其CSS選取器和新增的內容。

   ![冒險](assets/adventures.png)

1. 將體 *驗A重新命名* 為 *Adventure*
1. 同樣地，請更新 *Camping in Western Australia的* Experience B *，以探* 索Australian Wilderness **。

   ![探索](assets/explore.png)

1. 按一 **下** 「下一步」移至「定位」，讓我們在兩個體驗之間將手動流量分配為50-50。

   ![定位](assets/targeting.png)

1. 對於「目標」和設定，選擇「報告來源」作為Adobe Target，並選取「目標」量度作為「轉換」，並執行頁面檢視動作。

   ![目標](assets/goals.png)

1. 為活動提供名稱並儲存。
1. 啟動您儲存的活動，即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新標籤中開啟您的網站頁面（步驟3中的活動URL），您就可以從我們的A/B測試活動中檢視其中一種體驗（冒險或探索）。

   ![目標](assets/publish.png)

## 摘要

在本章中，行銷人員可以使用Visual Experience Composer來建立體驗，方法是拖放、交換和修改網頁的版面配置和內容，而不需變更任何程式碼來執行測試。

## 支援連結

+ [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
