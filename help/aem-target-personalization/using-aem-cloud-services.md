---
title: 使用Cloud Services將Adobe Experience Manager與Adobe Target整合
seo-title: 使用舊版AEMCloud Services將Adobe Experience Manager()與Adobe Target整合
description: 逐步逐步逐步逐步說明如何使用Cloud Service將Adobe Experience Manager()與Adobe TargetAEM整合
seo-description: 逐步逐步逐步逐步說明如何使用Cloud Service將Adobe Experience Manager()與Adobe TargetAEM整合
feature: 體驗片段
topic: 個性化
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 4%

---


# 使用舊AEM版Cloud Services

在本節中，我們將討論如何使用舊式Cloud Services與Adobe Experience Manager(AEM)建立合作關係。

>[!NOTE]
>
> 與AEMAdobe Target的舊Cloud Service僅&#x200B;****&#x200B;用於建立直接AEM作者與Adobe Target後端連線，以利內容從Target發AEM布。 Adobe啟動可讓Adobe Target在由公眾提供的面向公眾的網站體驗中公開AEM。

為了使用AEMExperience Fragment選件來推動您的個人化活動，讓您進入下一章，並使用舊版雲端服務AEM與Adobe Target整合。 必須進行此整合，才能將體驗片段AEM推送至Target做為HTML/JSON選件，並讓目標選件與之保持同AEM步。 實作概述部分](./overview.md#personalization-using-aem-experience-fragment)中討論的[藍本1時，需要進行此項整合。

## 必備條件

* **AEM**

   * 製作AEM和發佈例項是完成本教學課程的必要條件。 如果您尚未設定實例AEM，則可以按照[此處](./implementation.md#set-up-aem)的步驟操作。

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客戶需要從[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)向系統管理員提供Experience Platform Launch和Adobe I/O



### 與AEMAdobe Target整合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用AdobeIMS驗證建立Adobe TargetCloud Service(*使用Adobe TargetAPI*)(00:34)
2. 取得Adobe Target用戶端代碼(01:50)
3. 為Adobe Target建立AdobeIMS配置(02:08)
4. 建立在Adobe I/O主控台中存取Target API的技術帳戶(02:08)
5. 將Adobe TargetCloud Service加入AEM體驗片段(04:12)

目前，您已使用舊版Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)成功將[AEM與Adobe Target整合，如選項2中所述。 您現在應該可以在中建立體驗片段AEM，並將體驗片段發佈為HTML選件或JSON選件至Adobe Target，然後可用來建立活動。
