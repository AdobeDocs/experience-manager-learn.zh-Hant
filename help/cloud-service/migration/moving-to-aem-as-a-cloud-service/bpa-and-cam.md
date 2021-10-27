---
title: 設定BPA和CAM項目
description: 了解Best Practice Analyzer和Cloud Acceleration Manager如何提供自訂指南，以便移轉至AEMas a Cloud Service。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Best Practice Analyzer和Cloud Acceleration Manager

了解Best Practice Analyzer(BPA)和Cloud Acceleration Manager(CAM)如何提供自訂指南，以便移轉至AEMas a Cloud Service。 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## 評估就緒性

![BPA和CAM高級圖](assets/bpa-cam-diagram.png)

應將BPA套件安裝在原地複製的生產AEM 6.x環境上。 BPA將生成一個報告，然後可以上載到CAM中，該報告將指導需要進行的關鍵活動，以便移動到AEMas a Cloud Service。

### 關鍵活動

* 複製生產6.x環境。 當您移轉內容和重構程式碼時，複製生產環境對於測試各種工具和變更非常有用。
* 從下載最新的BPA工具 [Software Distribution入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 並安裝在AEM 6.x複製環境上。
* 使用BPA工具產生可上傳至Cloud Acceleration Manager(CAM)的報表。 CAM可透過 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* 使用CAM提供對當前代碼庫和環境進行哪些更新的指導，以便轉到AEMas a Cloud Service。
