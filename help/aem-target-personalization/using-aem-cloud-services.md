---
title: 使用Cloud Services整合Adobe Experience Manager與Adobe Target
seo-title: 使用舊版Cloud Services整合Adobe Experience Manager(AEM)與Adobe Target
description: 逐步說明如何使用AEMCloud Service將Adobe Experience Manager(AEM)與Adobe Target整合
seo-description: 逐步說明如何使用AEMCloud Service將Adobe Experience Manager(AEM)與Adobe Target整合
feature: 體驗片段
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# 使用AEM舊版Cloud Services

在本節中，我們將討論如何使用舊版Cloud Services，將Adobe Experience Manager(AEM)與Adobe Target設定。

>[!NOTE]
>
> 具有Adobe Target的AEM舊版Cloud Service為&#x200B;**only**，用來建立將AEM作者直接傳送至Adobe Target後端連線，以利將內容從AEM發佈至Target。 AdobeLaunch可在AEM提供的公開網站體驗上公開Adobe Target。

若要使用AEM體驗片段選件來支援您的個人化活動，讓您繼續前往下一章，並使用舊版雲端服務整合AEM與Adobe Target。 若要將體驗片段從AEM推送至Target，做為HTML/JSON選件，並使Target選件與AEM保持同步，此整合是必要的。 實作[概述區段](./overview.md#personalization-using-aem-experience-fragment)中討論的案例1需要此整合。

## 必備條件

* **AEM**

   * 完成本教學課程需要AEM製作和發佈例項。 如果您尚未設定AEM例項，可以依照[此處](./implementation.md#set-up-aem)的步驟操作。

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 需要為客戶提供[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)的Experience Platform Launch和Adobe I/O，或聯繫您的系統管理員



### 整合AEM與Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用AdobeIMS驗證(*使用Adobe Target API*)建立Adobe TargetCloud Service(00:34)
2. 取得Adobe Target用戶端代碼(01:50)
3. 建立Adobe Target的AdobeIMS設定(02:08)
4. 建立技術帳戶以在Adobe I/O主控台中存取Target API (02:08)
5. 將Adobe TargetCloud Service新增至AEM Experience片段(04:12)

此時，您已使用舊版Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)成功將[AEM與Adobe Target整合，如選項2所述。 您現在應該可以在AEM中建立體驗片段，並將體驗片段以HTML選件或JSON選件發佈至Adobe Target，然後便可用來建立活動。
