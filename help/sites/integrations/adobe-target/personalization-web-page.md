---
title: 完整網頁體驗的Personalization
description: 瞭解如何建立Target活動，以使用Adobe Target將AEM網站頁面重新導向至新頁面。
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 完整網頁體驗的Personalization {#personalization-fpe}

瞭解如何建立活動，使用Adobe Target將AEM上託管的網頁重新導向至新頁面。

## 先決條件

若要個人化AEM網站的完整頁面，必須完成下列設定：

1. [將Adobe Target新增至您的AEM網站](./add-target-launch-extension.md)
1. [從標籤觸發Adobe Target呼叫](./load-and-fire-target.md)

## 案例概述

WKND網站重新設計了首頁，並且想要將其目前首頁的訪客重新導向至新的首頁。 同時，也瞭解重新設計的首頁如何有助於提高使用者參與度和收入。 身為行銷人員，您已被指派建立活動，以將訪客重新導向新首頁的任務。 讓我們探索WKND網站首頁，瞭解如何使用Adobe Target建立活動。

## 使用視覺化體驗撰寫器(VEC)建立A/B測試的步驟

1. 登入Adobe Target並導覽至「活動」標籤
1. 按一下&#x200B;**建立活動**&#x200B;按鈕，然後選擇&#x200B;**A/B測試**&#x200B;活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選取&#x200B;**視覺化體驗撰寫器**&#x200B;選項，提供活動URL，然後按一下&#x200B;**下一步**

   ![活動URL](assets/ab-test-url.png)

1. 視覺化體驗撰寫器在您建立活動後在左側顯示兩個標籤： *體驗A*&#x200B;和&#x200B;*體驗B*。 從清單中選取體驗。 您可以使用&#x200B;**新增體驗**&#x200B;按鈕來新增體驗至清單。

   ![體驗選項](assets/experience-options.png)

1. 檢視體驗A可用的選項，然後選取&#x200B;**重新導向至URL**&#x200B;選項，並提供新WKND網站首頁的URL。

   ![重新導向URL](assets/redirect-url.png)

1. 將&#x200B;*體驗A*&#x200B;重新命名為&#x200B;*新的WKND首頁*&#x200B;和&#x200B;*體驗B*&#x200B;重新命名為&#x200B;*WKND首頁*

   ![冒險](assets/new-experiences.png)

1. 按一下[下一步]****&#x200B;移至[目標定位]，並保持兩個體驗之間的手動流量分配為50-50。

   ![目標定位](assets/targeting.png)

1. 針對目標與設定，選擇報表來源為Adobe Target，然後選取目標量度為轉換連同頁面檢視動作。

   ![目標](assets/goals.png)

1. 提供活動的名稱並儲存。
1. 啟用已儲存的活動以即時推送您的變更。

   ![目標](assets/activate.png)

1. 在新標籤中開啟您的網站頁面（步驟3中的活動URL），您應該能夠從A/B測試活動檢視任一體驗（WKND首頁或新WKND首頁）。 `us/en.html`重新導向至`us/home.html`。

   ![目標](assets/redirect-test.png)

## 摘要

身為行銷人員，您可以建立活動，使用Adobe Target將AEM上託管的網頁重新導向至新頁面。

## 支援連結

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
