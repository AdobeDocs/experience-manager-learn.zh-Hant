---
title: 全網頁體驗的個性化
description: 瞭解如何建立目標活動，以使用AEMAdobe Target將網站頁重定向到新頁。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# 全網頁體驗的個性化 {#personalization-fpe}

瞭解如何建立活動，以使用Adobe Target將承載的網站頁AEM重定向到新頁面。

## 必備條件

為了個性化網站的AEM全頁，必須完成以下設定：

1. [將Adobe Target添AEM加到網站](./add-target-launch-extension.md)
1. [從Launch觸發Adobe Target呼叫](./load-and-fire-target.md)

## 方案概述

WKND網站重新設計其首頁，並希望將其當前首頁訪問者重定向到新首頁。 同時，還要瞭解重新設計的首頁如何幫助改善用戶參與和收入。 作為營銷人員，您已分配了建立活動以將訪問者重定向到新首頁的任務。 讓我們瀏覽WKND網站首頁並瞭解如何使用Adobe Target建立活動。

## 使用Visual Experience Composer(VEC)建立A/Btest的步驟

1. 登錄Adobe Target並導航到「活動」頁籤
1. 按一下 **建立活動** 按鈕，然後選擇 **A/BTest** 活動

   ![A/B活動](assets/ab-target-activity.png)

1. 選擇 **視覺體驗作曲家** 選項，提供活動URL，然後按一下 **下一個**

   ![活動URL](assets/ab-test-url.png)

1. 建立新活動後，Visual Experience Composer在左側顯示兩個頁籤： *體驗A* 和 *經驗B*。 從清單中選擇體驗。 您可以使用 **添加體驗** 按鈕

   ![體驗選項](assets/experience-options.png)

1. 查看經驗A的可用選項，然後選擇 **重定向至URL** 選項，並提供新WKND網站首頁的URL。

   ![重定向URL](assets/redirect-url.png)

1. 更名 *體驗A* 至 *新建WKND首頁* 和 *經驗B* 至 *WKND首頁*

   ![冒險](assets/new-experiences.png)

1. 按一下 **下一個** 轉到目標並在兩個體驗之間保持手動流量分配50-50。

   ![定位](assets/targeting.png)

1. 對於「目標」和設定，選擇「報告」源作為「Adobe Target」，然後選擇「目標」度量作為「轉換」，並執行頁面視圖操作。

   ![目標](assets/goals.png)

1. 提供活動名稱並保存。
1. 激活已保存的活動以即時推送更改。

   ![目標](assets/activate.png)

1. 在新頁籤中開啟網站頁（步驟3中的活動URL），您應該能夠從我們的A/Btest活動中查看其中任何一種體驗（WKND首頁或新的WKND首頁）。 `us/en.html` 重定向 `us/home.html`。

   ![目標](assets/redirect-test.png)

## 摘要

作為營銷者，您能夠建立活動，以使用Adobe Target將托管在上的網AEM站頁重定向到新頁面。

## 支援連結

* [Adobe Experience Cloud調試器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud調試器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
