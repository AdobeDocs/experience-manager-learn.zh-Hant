---
title: 使用雲端服務將Adobe Experience Manager與Adobe Target整合
seo-title: 使用舊版雲端服務整合Adobe Experience Manager(AEM)與Adobe Target
description: 逐步逐步逐步逐步說明如何使用AEM Cloud服務將Adobe Experience Manager(AEM)與Adobe Target整合
seo-description: 逐步逐步逐步逐步說明如何使用AEM Cloud服務將Adobe Experience Manager(AEM)與Adobe Target整合
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# 使用AEM Legacy Cloud Services

在本節中，我們將討論如何使用舊版雲端服務與Adobe Target一起設定Adobe Experience Manager(AEM)。

>[!NOTE]
>
> 含Adobe Target的AEM Legacy Cloud服務是&#x200B;**only**，用來建立直接AEM作者與Adobe Target後端連線，以利將內容從AEM發佈至Target。 Adobe Launch是使用AEM提供的面向公眾的網站體驗來公開Adobe Target。

若要使用AEM體驗片段選件來強化您的個人化活動，讓您繼續下一章，並使用舊版雲端服務將AEM與Adobe Target整合。 必須進行此整合，才能將「體驗片段」從AEM推送至Target作為HTML/JSON選件，並讓目標選件與AEM保持同步。 實作概述部分](./overview.md#personalization-using-aem-experience-fragment)中討論的[藍本1時，需要進行此項整合。

## 必備條件

* **AEM**

   * AEM作者和發佈例項是完成本教學課程的必要工具。 如果您尚未設定AEM例項，請依照[這裡](./implementation.md#set-up-aem)的步驟進行。

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建下列解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客戶需要從[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)布建Experience Platform Launch和Adobe I/O，或聯絡您的系統管理員



### 整合AEM與Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS驗證建立Adobe Target Cloud服務（*使用Adobe Target API*）(00:34)
2. 取得Adobe Target用戶端代碼(01:50)
3. 建立Adobe Target的Adobe IMS設定(02:08)
4. 建立在Adobe I/O Console中存取Target API的技術帳戶(02:08)
5. 將Adobe Target Cloud服務新增至AEM Experience片段(04:12)

目前，您已使用舊版雲端服務成功將[AEM與Adobe Target整合，如選項2所述。 ](./using-aem-cloud-services.md#integrating-aem-target-options)您現在應該可以在AEM中建立體驗片段，並將體驗片段發佈為HTML選件或JSON選件至Adobe Target，然後可用來建立活動。
