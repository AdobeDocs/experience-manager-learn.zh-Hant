---
title: 使用視覺化體驗撰寫器進行個人化
description: 瞭解如何使用視覺化體驗撰寫器建立Adobe Target活動。
version: Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 141
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# 使用視覺化體驗撰寫器進行個人化 {#personalization-vec}

瞭解如何使用視覺化體驗撰寫器(VEC)建立A/B測試Target活動。

## 先決條件

若要在AEM網站上使用VEC，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從標籤觸發Adobe Target呼叫](./load-and-fire-target.md)

## 案例概述

WKND網站首頁會以資訊卡的形式顯示當地活動或城市周圍的最佳做法。 身為行銷人員，您已被指派修改首頁的任務，方法是變更冒險區段Teaser的文字，並瞭解它如何改善轉換。

## 使用視覺化體驗撰寫器(VEC)建立A/B測試的步驟

1. 登入 [Adobe Experience Cloud](https://experience.adobe.com/)，點選 __Target__，導覽至 __活動__ 標籤

   + 如果您沒有看到 __Target__ 在Experience Cloud控制面板上，確保在右上方的組織切換器中選取正確的Adobe組織，且該使用者已被授予在中存取Target的許可權 [Adobe Admin Console](https://adminconsole.adobe.com/).

1. 按一下 **建立活動** 按鈕，然後選擇 **A/B測試** 活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選取 **視覺化體驗撰寫器** 選項，提供活動URL，然後按一下 **下一個**

   ![活動URL](assets/ab-test-url.png)

1. 視覺化體驗撰寫器在您建立活動後，會在左側顯示兩個標籤： *體驗A* 和 *體驗B*. 從清單中選取體驗。 您可以使用將體驗新增至清單 **新增體驗** 按鈕。

   ![體驗A](assets/experience.png)

1. 在頁面上選取影像或文字，以開始進行修改，或使用程式碼編輯器來挑選及HTML元素。

   ![元素](assets/select-element.png)

1. 變更文字來源 *西澳洲露營* 至 *澳洲歷險記*. 「修改」下會顯示新增至體驗的變更清單。 您可以按一下並編輯修改的專案，以檢視其CSS選取器以及新增至該專案的新內容。

   ![冒險](assets/adventures.png)

1. 重新命名 *體驗A* 至 *冒險*
1. 同樣地，更新上的文字 *體驗B* 從 *西澳洲露營* 至 *探索澳洲荒野*.

   ![探索](assets/explore.png)

1. 按一下 **下一個** 移至目標定位，並保持兩個體驗之間的手動流量分配50至50。

   ![目標定位](assets/targeting.png)

1. 針對目標與設定，選擇報表來源為Adobe Target，然後選取目標量度為轉換連同頁面檢視動作。

   ![目標](assets/goals.png)

1. 提供活動的名稱並儲存。
1. 啟用已儲存的活動以即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新標籤中開啟您的網站頁面（步驟3中的活動URL），您應該能夠從A/B測試活動檢視任一體驗（探索或探索）。

   ![目標](assets/publish.png)

## 摘要

在本章中，行銷人員可以拖放、交換及修改網頁的版面和內容，不使用變更任何程式碼來執行測試，藉以使用視覺化體驗撰寫器建立體驗。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
