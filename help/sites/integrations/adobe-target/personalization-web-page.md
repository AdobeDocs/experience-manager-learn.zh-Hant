---
title: 個人化完整網頁體驗
description: 瞭解如何建立Target活動，以使用Adobe Target將AEM網站頁面重新導向至新頁面。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---


# 個人化完整網頁體驗 {#personalization-fpe}

瞭解如何建立活動，使用Adobe Target將AEM上裝載的網站頁面重新導向至新頁面。

## 必備條件

若要個人化AEM網站的完整頁面，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 藍本概觀

WKND網站重新設計其首頁，並想要將目前的首頁訪客重新導向至新首頁。 同時，也瞭解重新設計的首頁如何協助改善使用者參與度和收入。 身為行銷人員，您已獲指派工作來建立活動，將訪客重新導向至新首頁。 讓我們探索WKND網站首頁，並瞭解如何使用Adobe Target建立活動。

## 使用Visual Experience Composer(VEC)建立A/B測試的步驟

1. 登入Adobe Target並導覽至「活動」標籤
1. 按一 **下「建立活動** 」按鈕，然後選擇 **A/B測試活動** 。

   ![A/B活動](assets/ab-target-activity.png)

1. 選取「 **Visual Experience Composer** 」選項、提供「活動URL」，然後按「下一 **步」**

   ![活動URL](assets/ab-test-url.png)

1. 在您建立新活動後，Visual Experience Composer會在左側顯示兩個標籤： *體驗A**和B*。 從清單中選取體驗。 您可以使用「新增體驗」按鈕，將新的體驗新 **增至清單** 。

   ![體驗選項](assets/experience-options.png)

1. 檢視體驗A的可用選項，然後選取「重 **新導向至URL** 」選項，並提供新WKND網站首頁的URL。

   ![重新導向URL](assets/redirect-url.png)

1. 將 *體驗A重新命名* 為 *新的WKND首頁* ，將 *體驗B* 重新命 *名為WKND首頁*

   ![冒險](assets/new-experiences.png)

1. 按一 **下「下一** 步」移至「定位」，並在兩個體驗之間將「手動」流量分配維持在50-50。

   ![定位](assets/targeting.png)

1. 對於「目標」和設定，選擇「報告來源」作為Adobe Target，並選取「目標」量度作為「轉換」，並執行頁面檢視動作。

   ![目標](assets/goals.png)

1. 為活動提供名稱並儲存。
1. 啟動您儲存的活動，即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新標籤中開啟您的網站頁面（步驟3中的活動URL），您應該可以從我們的A/B測試活動中檢視其中一個體驗（WKND首頁或新的WKND首頁）。 `us/en.html` 重新導向 `us/home.html`至。

   ![目標](assets/redirect-test.png)

## 摘要

身為行銷人員，您可以建立活動，將AEM上裝載的網站頁面重新導向至使用Adobe Target的新頁面。

## 支援連結

* [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

