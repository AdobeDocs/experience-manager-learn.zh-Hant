---
title: 從PDF檔案將資料匯入最適化表單
description: 教學課程可透過匯入PDF檔案來填入最適化表單
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 部署範例資產

您可以部署範例資產，讓此解決方案在本機AEM Forms執行個體上運作

* [匯入使用者端資料庫和自訂元件，以透過「封裝管理員」上傳PDF表單](./assets/client-libs-custom-component.zip)
* 使用OSGi Web主控台[自訂檔案服務套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)下載並部署該套裝
* 使用OSGi Web主控台[使用服務使用者套件開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)下載及部署套件
* 使用OSGi Web主控台[從pdf檔案匯入資料](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)下載並部署套件組合
* 在&#x200B;_**Apache Sling Service使用者對應程式服務**_ OSGi設定主控台中，新增專案&#x200B;_**DevelopingWithServiceUser.core：getresourceresolver=data**_
