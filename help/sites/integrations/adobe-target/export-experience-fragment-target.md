---
title: 將體驗片段匯出至Adobe Target
description: 瞭解如何將體驗片段發AEM布及匯出為Adobe Target優惠。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 4%

---


# 將體驗片段匯出至Adobe Target{#experience-fragment-target}

瞭解如何將體驗AEM片段匯出為Adobe Target優惠。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 後續步驟

+ [使用體驗片段選件建立目標活動](./create-target-activity.md)

## 疑難排解

### 將體驗片段匯出至Target失敗

#### 錯誤

將「體驗片段」匯出至Adobe Target，但沒有Adobe Admin Console的正確權限，會在AEM Author服務上造成下列錯誤：

    ![Target API UI錯誤](assets/error-target-offer.png)

...和`aemerror`日誌中的以下日誌消息：

    ![Target API Console錯誤](assets/target-console-error.png)

#### 解析度

1. 使用Adobe Target產品配置檔案的管理權限登錄[Admin Console](https://adminconsole.adobe.com/)，但整合AEM。
2. 選擇&#x200B;__產品>Adobe Target>產品概要檔案__
3. 在&#x200B;__Integrations__&#x200B;標籤下，選取您的整合作AEM為Cloud Service環境(與Adobe I/O項目相同名稱)
4. 指派&#x200B;__Editor__&#x200B;或&#x200B;__Approver__&#x200B;角色

   ![目標API錯誤](assets/target-permissions.png)

將正確權限新增至您的Adobe Target整合應能解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)