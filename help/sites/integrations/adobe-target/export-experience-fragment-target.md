---
title: 將體驗片段匯出至Adobe Target
description: 瞭解如何將AEM體驗片段發佈和匯出為Adobe Target選件。
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service， AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
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

...以及`aemerror`記錄檔中的下列記錄檔訊息：

![目標API主控台錯誤](assets/target-console-error.png)

#### 解決方法

1. 以Admin Console產品設定檔的管理許可權登入[Adobe Target](https://adminconsole.adobe.com/)，但使用AEM整合
2. 選取&#x200B;__產品> Adobe Target >產品設定檔__
3. 在&#x200B;__整合__&#x200B;標籤下方，選取您AEM as a Cloud Service環境的整合(與Adobe Developer專案相同的名稱)
4. 指派&#x200B;__編輯者__&#x200B;或&#x200B;__核准者__&#x200B;角色

   ![目標API錯誤](assets/target-permissions.png)

將正確的許可權新增至您的Adobe Target整合應可解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
