---
title: 使用Visual Experience Composer實現個性化
description: 瞭解如何使用Visual Experience Composer建立Adobe Target活動。
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 1%

---

# 使用Visual Experience Composer實現個性化 {#personalization-vec}

瞭解如何使用Visual Experience Composer(VEC)建立A/BTest目標活動。

## 必備條件

要在網站上使AEM用VEC，必須完成以下設定：

1. [將Adobe Target添AEM加到網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 方案概述

WKND網站首頁以資訊卡的形式顯示本地活動或在城市周圍最好的活動。 作為營銷人員，您已分配了修改首頁的任務，方法是對冒險部分預告進行文本更改，並瞭解它如何改進轉換。

## 使用Visual Experience Composer(VEC)建立A/Btest的步驟

1. 登錄到 [Adobe Experience Cloud](https://experience.adobe.com/)，點擊 __目標__，導航至 __活動__ 頁籤

   + 如果你看不到 __目標__ 在Experience Cloud控制板上，確保在右上角的「組織切換器」中選擇了正確的Adobe組織，並且已授予用戶對中的「目標」的訪問權限 [Adobe Admin Console](https://adminconsole.adobe.com/)。

1. 按一下 **建立活動** 按鈕，然後選擇 **A/BTest** 活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選擇 **視覺體驗作曲家** 選項，提供活動URL，然後按一下 **下一個**

   ![活動URL](assets/ab-test-url.png)

1. 建立新活動後，Visual Experience Composer在左側顯示兩個頁籤： *體驗A* 和 *經驗B*。 從清單中選擇體驗。 您可以使用 **添加體驗** 按鈕

   ![體驗A](assets/experience.png)

1. 選擇頁面上的影像或文本以開始進行修改，或使用代碼編輯器可以選取和HTML元素。

   ![元素](assets/select-element.png)

1. 更改文本 *西澳大利亞露營* 至 *澳大利亞歷險記*。 添加到「體驗」(Experience)的更改清單顯示在「修改」(Modifies)下。 可以按一下並編輯修改的項，以查看其CSS選擇器和添加到它的新內容。

   ![冒險](assets/adventures.png)

1. 更名 *體驗A* 至 *冒險*
1. 同樣，更新上的文本 *經驗B* 從 *西澳大利亞露營* 至 *探索澳大利亞荒野*。

   ![瀏覽](assets/explore.png)

1. 按一下 **下一個** 轉到目標並讓我們在兩個體驗之間手動分配50-50的流量。

   ![定位](assets/targeting.png)

1. 對於「目標」和設定，選擇「報告」源作為「Adobe Target」，然後選擇「目標」度量作為「轉換」，並執行頁面視圖操作。

   ![目標](assets/goals.png)

1. 提供活動名稱並保存。
1. 激活已保存的活動以即時推送更改。

   ![目標](assets/activate.png)

1. 在新頁籤中開啟網站頁（步驟3中的活動URL），您應該能夠從我們的A/Btest活動中查看其中一種體驗（冒險或瀏覽）。

   ![目標](assets/publish.png)

## 摘要

在本章中，商家通過拖放、交換和修改網頁的佈局和內容而無需更改任何代碼來運行test，從而能夠使用Visual Experience Composer建立體驗。

## 支援連結

+ [Adobe Experience Cloud調試器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud調試器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
