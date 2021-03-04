---
title: 設定帳戶和服務以擴充Asset compute
description: 開發Asset compute員工需要存取帳戶和服務，AEM包括Cloud Service、AdobeProject Firefly以及Microsoft或Amazon提供的雲端儲存空間。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# 設定帳戶和服務

本教學課程需要下列服務才能布建並透過學員的Adobe ID存取。

所有Adobe服務都必須透過同一Adobe組織(使用您的Adobe ID)存取。

+ [AEM as a Cloud Service ](#aem-as-a-cloud-service)
+ [Adobe專案FireFly](#adobe-project-firefly)
   + 資源調配需要2 - 10天
+ 雲端儲存空間
   + [Azure Blob儲存空間](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在繼續本教學課程之前，請確定您可存取上述所有服務。
> 
> 檢閱以下章節，瞭解如何設定和布建所需服務。

## AEM as a Cloud Service {#aem-as-a-cloud-service}

要配置「AEMAEM Assets處理配置檔案」以調用自定義Asset compute工作器，必須訪問作為Cloud Service環境的。

最理想的情況是提供沙盒程式或非沙盒開發環境。

請注意，本AEM機SDK不足以完成本教學課程，因為本機AEMSDK無法與Asset compute的Microservices通訊，而需AEM要真的Cloud Service環境。

## AdobeProject Firefly{#adobe-project-firefly}

[Adobe項目Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)框架用於構建和部署自定義操作到Adobe I/O Runtime(Adobe的無伺服器平台)。 AEMAsset compute專案是專門建立的Firefly專案，可透過「處理設定檔」與AEM Assets整合，並提供存取和處理資產二進位檔的功能。

若要存取Project Firefly，請註冊預覽。

1. [註冊專案Firefly預覽](https://adobeio.typeform.com/to/obqgRm)。
1. 請等待約2 - 10天，直到收到電子郵件通知您已布建，然後再繼續教學課程。
   + 如果您不確定是否已布建，請繼續後續步驟，如果您無法在[Adobe開發人員主控台(Developer Console)中建立&#x200B;__專案Firefly__&#x200B;專案，則您仍未布建。](https://console.adobe.io)

## 雲端儲存空間

本端開發Asset compute專案需要雲端儲存空間。

將Asset compute員工部署到Adobe I/O Runtime以作為Cloud Service直AEM接使用時，無需嚴格要求此雲儲存，因AEM為它提供了從中讀取資產和寫入其格式副本的雲儲存。

### Microsoft Azure Blob儲存{#azure-blob-storage}

如果您尚未擁有Microsoft Azure Blob Storage的存取權，請註冊[免費12個月帳戶](https://azure.microsoft.com/en-us/free/)。

本教學課程將使用Azure Blob Storage，但[AmazonS3](#amazon-s3)可以使用，而且只能使用教學課程的小變數。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_點進Azure Blob儲存空間布建（無音訊）_


1. 登入您的[Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/)。
1. 導航至&#x200B;__儲存帳戶__ Azure服務部分
1. 點選&#x200B;__+ Add__&#x200B;以建立新的Blob儲存帳戶
1. 根據需要建立新的&#x200B;__資源組__，例如：`aem-as-a-cloud-service`
1. 提供&#x200B;__儲存帳戶名稱__，例如：`aemguideswkndassetcomput`
   + __儲存帳戶名稱__&#x200B;將用於[設定本機Asset compute開發工具的雲端儲存空間](../develop/environment-variables.md)
   + 當[配置雲儲存](../develop/environment-variables.md)時，還需要與儲存帳戶關聯的&#x200B;__訪問密鑰__。
1. 將所有其他項目保留為預設值，然後點選「檢閱+建立&#x200B;__」按鈕__
   + （可選）選擇靠近您的&#x200B;__位置__。
1. 檢閱布建請求以瞭解正確性，並點選&#x200B;__Create__&#x200B;按鈕（若已確認）

### AmazonS3{#amazon-s3}

建議使用[Microsoft Azure Blob Storage](#azure-blob-storage)來完成本教學課程，但也可使用[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)。

如果使用AmazonS3儲存，請在[配置項目的環境變數](../develop/environment-variables.md#amazon-s3)時指定AmazonS3雲儲存憑據。

如果您需要特別為本教學課程布建雲端儲存空間，我們建議使用[Azure Blob儲存空間](#azure-blob-storage)。
