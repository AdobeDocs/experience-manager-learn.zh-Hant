---
title: 設定帳戶和服務以提高Asset compute的可擴充性
description: 開發Asset compute背景工作時，需要存取帳戶和服務，包括AEM as aCloud Service、AdobeProject Firefly，以及Microsoft或Amazon提供的雲端儲存空間。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 1%

---


# 設定帳戶和服務

本教學課程需要布建下列服務，並可透過學習者的Adobe ID存取。

所有Adobe服務必須透過相同的Adobe組織(使用您的Adobe ID)存取。

+ [AEM as a Cloud Service ](#aem-as-a-cloud-service)
+ [Adobe項目FireFly](#adobe-project-firefly)
   + 布建可能需要2 - 10天
+ 雲端儲存空間
   + [Azure Blob儲存](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>繼續完成本教學課程之前，請確定您可以存取上述所有服務。
> 
> 檢閱以下章節，了解如何設定及提供所需服務。

## AEM as a Cloud Service {#aem-as-a-cloud-service}

若要設定AEM Assets處理設定檔，以叫用自訂Cloud Service背景工作，必須存取AEM as aAsset compute環境。

理想情況下，您可使用沙箱方案或非沙箱開發環境。

請注意，本機AEM SDK不足以完成本教學課程，因為本機AEM SDK無法與Asset compute微服務通訊，而是需要真正的AEM作為Cloud Service環境。

## AdobeProject Firefly{#adobe-project-firefly}

[AdobeProject Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)架構用於建置自訂動作，並部署至Adobe的無伺服器平台Adobe I/O Runtime。 AEMAsset compute專案是特別建置的Firefly專案，可透過處理設定檔與AEM Assets整合，並提供存取和處理資產二進位檔的功能。

若要取得Project Firefly的存取權，請註冊以進行預覽。

1. [註冊Project Firefly預覽](https://adobeio.typeform.com/to/obqgRm)。
1. 請等待約2 - 10天，直到收到已布建的電子郵件通知，再繼續進行本教學課程。
   + 如果您不確定您是否已布建，請繼續進行後續步驟，如果您無法在[Adobe開發人員控制台](https://console.adobe.io)中建立&#x200B;__Project Firefly__&#x200B;專案，則您仍未布建。

## 雲端儲存空間

本機開發Asset compute專案需要雲端儲存空間。

將Asset compute背景工作部署至Adobe I/O Runtime以供AEM作為Cloud Service直接使用時，並非嚴格要求使用此雲端儲存空間，因為AEM提供的雲端儲存空間可讀取資產，並將其寫入轉譯。

### Microsoft Azure Blob儲存{#azure-blob-storage}

如果您尚無權訪問Microsoft Azure Blob儲存，請註冊[12個月免費帳戶](https://azure.microsoft.com/en-us/free/)。

本教學課程將使用Azure Blob儲存，但[Amazon S3](#amazon-s3)可使用，且僅能使用本教學課程的微幅變更。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_點進設定Azure Blob儲存（無音頻）_


1. 登入您的[Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/)。
1. 導覽至&#x200B;__儲存帳戶__ Azure服務區段
1. 點選&#x200B;__+新增__&#x200B;以建立新的Blob儲存帳戶
1. 視需要建立新的&#x200B;__資源組__，例如：`aem-as-a-cloud-service`
1. 提供&#x200B;__儲存帳戶名稱__，例如：`aemguideswkndassetcomput`
   + __儲存帳戶名稱__&#x200B;將用於為本地Asset compute開發工具[配置雲儲存](../develop/environment-variables.md)
   + 當[配置雲儲存](../develop/environment-variables.md)時，還需要與儲存帳戶關聯的&#x200B;__訪問密鑰__。
1. 將其他項目保留為預設值，然後點選&#x200B;__檢閱+建立__&#x200B;按鈕
   + （可選）選擇您附近的&#x200B;__location__。
1. 檢閱布建請求以了解正確性，然後點選&#x200B;__Create__&#x200B;按鈕（若已確認）

### Amazon S3{#amazon-s3}

建議使用[Microsoft Azure Blob儲存](#azure-blob-storage)來完成本教學課程，但也可使用[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)。

如果使用Amazon S3儲存空間，請在[設定專案的環境變數](../develop/environment-variables.md#amazon-s3)時指定Amazon S3雲端儲存空間憑證。

如果您需要特別為本教學課程布建雲儲存空間，建議您使用[Azure Blob Storage](#azure-blob-storage)。
