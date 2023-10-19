---
title: 從PDF檔案將資料匯入最適化表單
description: 教學課程可透過匯入PDF檔案來填入最適化表單
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 部署範例資產

您可以部署範例資產，讓此解決方案在本機AEM Forms執行個體上運作

* [匯入使用者端資料庫和自訂元件，以透過「封裝管理員」上傳PDF表單](./assets/client-libs-custom-component.zip)
* 使用OSGi Web主控台下載並部署該套件組合[自訂檔案服務套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 使用OSGi Web主控台下載並部署該套件組合 [使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用OSGi Web主控台下載並部署該套件組合[從pdf檔案匯入資料](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* 新增專案 _**DevelopingWithServiceUser.core：getresourceresolver=data**_ 在 _**Apache Sling服務使用者對應程式服務**_ OSGi設定控制檯