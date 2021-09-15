---
title: 將體驗片段匯出至Adobe Target
description: 了解如何發佈和匯出AEM體驗片段為Adobe Target選件。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 2%

---

# 將體驗片段匯出至Adobe Target {#experience-fragment-target}

了解如何將AEM體驗片段匯出為Adobe Target選件。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 後續步驟

+ [使用體驗片段選件建立Target活動](./create-target-activity.md)

## 疑難排解

### 將體驗片段匯出至Target失敗

#### 錯誤

在Adobe Admin Console中未具有正確權限的情況下，將體驗片段匯出至Adobe Target，會在AEM製作服務上造成下列錯誤：

    ![Target API UI錯誤](assets/error-target-offer.png)

...和`aemerror`日誌中的以下日誌消息：

    ![Target API控制台錯誤](assets/target-console-error.png)

#### 解析度

1. 使用Adobe Target產品設定檔的管理權限登入[Admin Console](https://adminconsole.adobe.com/)但使用AEM整合
2. 選取&#x200B;__產品> Adobe Target >產品設定檔__
3. 在&#x200B;__整合__&#x200B;標籤下，選取AEM作為Cloud Service環境的整合(與Adobe I/O專案相同名稱)
4. 指派&#x200B;__Editor__&#x200B;或&#x200B;__Approver__&#x200B;角色

   ![Target API錯誤](assets/target-permissions.png)

將正確權限新增至您的Adobe Target整合應可解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
