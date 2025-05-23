---
title: 使用Cloud Service整合Adobe Experience Manager與Adobe Target
description: 如何使用AEM Cloud Service將Adobe Experience Manager (AEM)與Adobe Target整合的逐步解說
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 337
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 1%

---

# 使用AEM舊版Cloud Service

在本節中，我們將討論如何使用舊版Cloud Service以Adobe Target設定Adobe Experience Manager (AEM)。

>[!NOTE]
>
> 具有Adobe Target的AEM舊版Cloud Service僅&#x200B;**是**，用來建立直接的AEM Author到Adobe Target後端連線，以方便將內容從AEM發佈到Target。 Adobe Experience Platform中的標籤是用來在公開的AEM網站體驗上公開Adobe Target。

為了使用AEM體驗片段選件來強化您的個人化活動，請繼續下一章，並使用舊版雲端服務整合AEM與Adobe Target。 從AEM推送體驗片段做為HTML/JSON選件至Target，以及讓Target選件與AEM保持同步時，需要這項整合。 實作概觀區段[&#128279;](./overview.md#personalization-using-aem-experience-fragment)中討論的案例1需要這項整合。

## 先決條件

* **AEM**

   * 您必須有AEM作者和發佈例項，才能完成本教學課程。 如果您尚未設定您的AEM執行個體，您可以依照步驟[這裡](./implementation.md#set-up-aem)。

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建下列解決方案的Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > 客戶需要布建來自[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)的資料收集和Adobe I/O，或聯絡您的系統管理員

### 將AEM與Adobe Target整合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS驗證建立Adobe TargetCloud Service(*使用Adobe Target API*) (00:34)
2. 取得Adobe Target使用者端代碼(01:50)
3. 為Adobe Target建立Adobe IMS設定(02:08)
4. 建立技術帳戶以存取Adobe I/O控制檯中的Target API (02:08)
5. 將Adobe TargetCloud Service新增到AEM體驗片段(04:12)

此時，您已順利使用舊版Cloud Service[&#128279;](./using-aem-cloud-services.md#integrating-aem-target-options)將AEM與Adobe Target整合（如選項2所述）。 您現在應該能夠在AEM中建立體驗片段，並將體驗片段發佈為HTML選件或JSON選件至Adobe Target，然後可以使用建立活動。
