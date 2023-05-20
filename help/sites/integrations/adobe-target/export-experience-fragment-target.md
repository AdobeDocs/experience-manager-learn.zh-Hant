---
title: 將經驗片段導出到Adobe Target
description: 瞭解如何按Adobe Target提供的AEM方式發佈和導出體驗片段。
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
ht-degree: 3%

---

# 向Adobe Target出口經驗 {#experience-fragment-target}

瞭解如何按Adobe TargetAEM提供的方式導出體驗片段。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 後續步驟

+ [使用體驗片段優惠建立目標活動](./create-target-activity.md)

## 疑難排解

### 將體驗片段導出到目標失敗

#### 錯誤

將體驗片段導出到Adobe Target時，在Adobe Admin Console沒有正確權限，導致AEM作者服務出現以下錯誤：

    ![目標API UI錯誤](assets/error-target-offer.png)

...以及以下日誌消息 `aemerror` 日誌：

    ![目標API控制台錯誤](assets/target-console-error.png)

#### 解析度

1. 登錄到 [Admin Console](https://adminconsole.adobe.com/) 已使用Adobe Target產品配置檔案的管理權AEM限
2. 選擇 __產品>Adobe Target>產品概要__
3. 下 __整合__ 頁籤，選擇AEMas a Cloud Service環境的整合(與Adobe I/O項目同名)
4. 分配 __編輯器__ 或 __批准者__ 角色

   ![目標API錯誤](assets/target-permissions.png)

向您的Adobe Target整合添加正確權限應解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud調試器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud調試器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
