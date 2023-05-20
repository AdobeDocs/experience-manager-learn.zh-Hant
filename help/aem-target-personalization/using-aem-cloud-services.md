---
title: 利用Cloud Services將Adobe Experience Manager與Adobe Target整合
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: 逐步講解如何利用AEM Cloud Service將Adobe Experience Manager(AEM)與Adobe Target整合
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---

# 使用AEM舊Cloud Services

在本節中，我們將討論如何使用LegacyCloud ServicesAEM與Adobe Target建立Adobe Experience Manager()。

>[!NOTE]
>
> 與AEMAdobe Target的遺產Cloud Service **僅** 用於建立AEM作者到Adobe Target後端的直接連接，方便從目AEM標發佈內容。 Adobe產品發佈在Adobe Target提供的面向公眾的網站體驗AEM中。

為了使用體驗AEM片段產品來推動您的個性化活動，讓您繼續下一章，並使用舊式雲服務與AEMAdobe Target整合。 需要此整合才能將體驗片段作為AEMHTML/JSON提供的內容從推送到目標，並使目標提供內容與之保持同AEM步。 實施時需要此整合 [概述部分中討論的方案1](./overview.md#personalization-using-aem-experience-fragment)。

## 必備條件

* **AEM**

   * 完成AEM本教程需要使用作者和發佈實例。 如果尚未設定實AEM例，可以執行步驟 [這裡](./implementation.md#set-up-aem)。

* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客戶需要提供Experience Platform Launch和Adobe I/O [Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html) 或聯繫您的系統管理員


### 與AEMAdobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS身份驗證建立Adobe TargetCloud Service(*使用Adobe TargetAPI*)(00:34)
2. 獲取Adobe Target客戶端代碼(01:50)
3. 為Adobe Target建立Adobe IMS配置(02:08)
4. 建立用於在Adobe I/O控制台中訪問目標API的技術帳戶(02:08)
5. 將Adobe TargetCloud Service添AEM加到體驗片段(04:12)

此時，您已成功整合 [與AEMAdobe Target一起使用傳統Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) 詳見選項2。 現在，您應該能夠在中建立體驗片段AEM，並將體驗片段作為HTML優惠或JSON優惠發佈到Adobe Target，然後可用於建立活動。
