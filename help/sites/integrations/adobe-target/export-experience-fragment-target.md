---
title: 將體驗片段匯出至Adobe Target
description: 瞭解如何將AEM Experience Fragment發佈並匯出為Adobe Target選件。
feature: Experience Fragments
version: Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 237
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 3%

---

# 將體驗片段匯出至Adobe Target {#experience-fragment-target}

瞭解如何將AEM體驗片段匯出為Adobe Target選件。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 後續步驟

+ [使用體驗片段選件建立Target活動](./create-target-activity.md)

## 疑難排解

### 將體驗片段匯出至Target失敗

#### 錯誤

在沒有Adobe Admin Console中的正確許可權的情況下將體驗片段匯出至Adobe Target，會在AEM作者服務上導致以下錯誤：

![Target API UI錯誤](assets/error-target-offer.png)

...以及中的以下記錄訊息 `aemerror` 記錄：

![Target API主控台錯誤](assets/target-console-error.png)

#### 解決方法

1. 登入 [Admin Console](https://adminconsole.adobe.com/) 已使用Adobe Target產品設定檔的管理許可權，但AEM整合
2. 選取 __產品> Adobe Target >產品描述檔__
3. 在 __整合__ 索引標籤中，為您的AEMas a Cloud Service環境選取整合(與Adobe Developer專案相同的名稱)
4. 指派 __編輯者__ 或 __核准者__ 角色

   ![Target API錯誤](assets/target-permissions.png)

將正確的許可權新增至您的Adobe Target整合應可解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
