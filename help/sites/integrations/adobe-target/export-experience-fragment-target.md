---
title: 將體驗片段匯出至Adobe Target
description: 瞭解如何將AEM體驗片段發佈及匯出為Adobe Target選件。
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---


# 將體驗片段匯出至Adobe Target {#experience-fragment-target}

瞭解如何將AEM體驗片段匯出為Adobe Target選件。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 後續步驟

+ [使用體驗片段選件建立目標活動](./create-target-activity.md)

## 疑難排解

### 將體驗片段匯出至Target失敗

#### 錯誤

將「體驗片段」匯出至Adobe Target，但Adobe Admin Console沒有正確的權限，會導致AEM Author服務發生下列錯誤：

    ![Target API UI錯誤](assets/error-target-offer.png)

...和`aemerror`日誌中的以下日誌消息：

    ![Target API Console錯誤](assets/target-console-error.png)

#### 解析度

1. 使用Adobe Target產品設定檔的管理權限登入[管理控制台](https://adminconsole.adobe.com/)，但是AEM整合
2. 選擇&#x200B;__產品> Adobe Target >產品描述檔__
3. 在「__Integrations__」標籤下，選取AEM的整合為雲端服務環境（與Adobe I/O專案相同）
4. 指派&#x200B;__Editor__&#x200B;或&#x200B;__Approver__&#x200B;角色

   ![目標API錯誤](assets/target-permissions.png)

將正確的權限新增至Adobe Target整合應能解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)