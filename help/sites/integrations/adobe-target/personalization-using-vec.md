---
title: 使用可視化體驗撰寫器進行個人化
description: 了解如何使用可視化體驗撰寫器建立Adobe Target活動。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# 使用可視化體驗撰寫器進行個人化 {#personalization-vec}

了解如何使用可視化體驗撰寫器(VEC)建立A/B測試Target活動。

## 必備條件

若要在AEM網站上使用VEC，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 藍本概述

WKND網站首頁以資訊卡的形式顯示城市周圍的當地活動或最佳活動。 身為行銷人員，您已獲指派工作來修改首頁，方法是對冒險區段預告進行文字變更，並了解其如何改善轉換。

## 使用可視化體驗撰寫器(VEC)建立A/B測試的步驟

1. 登入[Adobe Experience Cloud](https://experience.adobe.com/)，點選&#x200B;__Target__，導覽至&#x200B;__活動__&#x200B;標籤

   + 如果您在Experience Cloud控制面板上未看到&#x200B;__Target__，請確認在右上角的組織切換器中已選取正確的Adobe組織，並確認您的使用者已獲得[Adobe Admin Console](https://adminconsole.adobe.com/)中的Target存取權。

1. 按一下「**建立活動**」按鈕，然後選擇「**A/B測試**」活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選取&#x200B;**可視化體驗撰寫器**&#x200B;選項，提供活動URL，然後按一下&#x200B;**下一步**

   ![活動URL](assets/ab-test-url.png)

1. 在您建立新活動後，可視化體驗撰寫器會在左側顯示兩個標籤：*體驗A*&#x200B;和&#x200B;*體驗B*。 從清單中選取體驗。 您可以使用&#x200B;**新增體驗**&#x200B;按鈕，將新體驗新增至清單。

   ![體驗A](assets/experience.png)

1. 在頁面上選取要開始進行修改的影像或文字，或使用可使用程式碼編輯器來挑選和HTML元素。

   ![元素](assets/select-element.png)

1. 將文字從「*Camping in Western Australia*」變更為「*Adventura Of Australia*」。 新增至體驗的變更清單將顯示在修改下。 您可以按一下並編輯修改的項目，以檢視其CSS選取器以及新增的新內容。

   ![冒險](assets/adventures.png)

1. 將&#x200B;*體驗A*&#x200B;重新命名為&#x200B;*Adventure*
1. 同樣地，將&#x200B;*體驗B*&#x200B;上的文字從&#x200B;*Camping in Western Australia*&#x200B;更新為&#x200B;*Explore the Australian Wilderness*。

   ![探索](assets/explore.png)

1. 按一下&#x200B;**Next**&#x200B;以移至「鎖定目標」，並讓兩個體驗之間的手動流量分配維持50-50。

   ![定位](assets/targeting.png)

1. 針對「目標」和設定，選擇「報表來源」作為「Adobe Target」，並選取「目標量度」作為「轉換」，並執行頁面檢視動作。

   ![目標](assets/goals.png)

1. 提供活動名稱並儲存。
1. 啟動您儲存的活動，即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新索引標籤中開啟您的網站頁面（步驟3中的活動URL），您應該可以從我們的A/B測試活動中檢視其中一個體驗（探險或探索）。

   ![目標](assets/publish.png)

## 摘要

在本章中，行銷人員可以透過拖放、切換及修改網頁的版面與內容，使用可視化體驗撰寫器來建立體驗，而不需變更要執行測試的任何代碼。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
