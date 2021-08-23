---
title: 個人化完整網頁體驗
description: 了解如何建立Target活動，使用Adobe Target將您的AEM網站頁面重新導向至新頁面。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---


# 個人化完整網頁體驗 {#personalization-fpe}

了解如何建立活動，使用Adobe Target將AEM上托管的網站頁面重新導向至新頁面。

## 必備條件

若要個人化AEM網站的完整頁面，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 藍本概觀

WKND網站重新設計了首頁，並想要將其目前的首頁訪客重新導向至新的首頁。 同時，也能了解重新設計的首頁如何協助改善使用者參與度和收入。 身為行銷人員，您已獲指派工作來建立活動，以將訪客重新導向至新首頁。 讓我們探索WKND網站首頁，了解如何使用Adobe Target建立活動。

## 使用可視化體驗撰寫器(VEC)建立A/B測試的步驟

1. 登入Adobe Target並導覽至「活動」標籤
1. 按一下「**建立活動**」按鈕，然後選擇「**A/B測試**」活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選取&#x200B;**可視化體驗撰寫器**&#x200B;選項，提供活動URL，然後按一下&#x200B;**下一步**

   ![活動URL](assets/ab-test-url.png)

1. 在您建立新活動後，可視化體驗撰寫器會在左側顯示兩個標籤：*體驗A*&#x200B;和&#x200B;*體驗B*。 從清單中選取體驗。 您可以使用&#x200B;**新增體驗**&#x200B;按鈕，將新體驗新增至清單。

   ![體驗選項](assets/experience-options.png)

1. 檢視體驗A可用的選項，然後選取&#x200B;**重新導向至URL**&#x200B;選項，並提供新WKND網站首頁的URL。

   ![重新導向URL](assets/redirect-url.png)

1. 將&#x200B;*體驗A*&#x200B;重新命名為&#x200B;*新WKND首頁*，並將&#x200B;*體驗B*&#x200B;重新命名為&#x200B;*WKND首頁*

   ![冒險](assets/new-experiences.png)

1. 按一下&#x200B;**Next**&#x200B;以移至「鎖定目標」，並在兩個體驗之間保留50-50的手動流量分配。

   ![定位](assets/targeting.png)

1. 針對「目標」和設定，選擇「報表來源」作為「Adobe Target」，並選取「目標量度」作為「轉換」，並執行頁面檢視動作。

   ![目標](assets/goals.png)

1. 提供活動名稱並儲存。
1. 啟動您儲存的活動，即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新索引標籤中開啟您的網站頁面（步驟3中的活動URL），您應該可以從我們的A/B測試活動中檢視其中一個體驗（WKND首頁或新的WKND首頁）。 `us/en.html` 重新導向至 `us/home.html`。

   ![目標](assets/redirect-test.png)

## 摘要

身為行銷人員，您可以建立活動，使用Adobe Target將AEM上托管的網站頁面重新導向至新頁面。

## 支援連結

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

