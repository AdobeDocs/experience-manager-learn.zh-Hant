---
title: 設定帳戶和服務以提高Asset compute的可擴充性
description: 開發Asset compute背景工作時，需要存取帳戶和服務，包括Microsoft或Amazon提供的AEMas a Cloud Service、App Builder和雲端儲存空間。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# 設定帳戶和服務

本教學課程需要布建下列服務，並可透過學習者的Adobe ID存取。

所有Adobe服務都必須透過相同的Adobe組織(使用您的Adobe ID)存取。

+ [AEM as a Cloud Service ](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 布建可能需要2 - 10天
+ 雲端儲存空間
   + [Azure Blob儲存](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>繼續完成本教學課程之前，請確定您可以存取上述所有服務。
> 
> 檢閱以下章節，說明如何設定及提供所需服務。

## AEM as a Cloud Service {#aem-as-a-cloud-service}

若要設定AEM Assets處理設定檔，以叫用自訂Asset compute背景工作，必須存取AEMas a Cloud Service環境。

理想情況下，您可使用沙箱方案或非沙箱開發環境。

請注意，本機AEM SDK不足以完成本教學課程，因為本機AEM SDK無法與Asset compute微服務通訊，而是需要真正的AEMas a Cloud Service環境。

## App Builder{#app-builder}

此 [App Builder](https://developer.adobe.com/app-builder/) framework可用來建置自訂動作，並將其部署至Adobe的無伺服器平台Adobe I/O Runtime。 AEMAsset compute專案是特別建置的App Builder專案，可透過處理設定檔與AEM Assets整合，並提供存取及處理資產二進位檔的功能。

若要取得App Builder的存取權，請註冊以進行預覽。

1. [註冊App Builder試用版](https://developer.adobe.com/app-builder/trial/).
1. 請等待約2 - 10天，直到收到已布建的電子郵件通知，再繼續進行本教學課程。
   + 如果您不確定您是否已布建，請繼續進行後續步驟，如果您無法建立 __App Builder__ 項目 [Adobe Developer Console](https://developer.adobe.com/console/) 您尚未布建。

## 雲端儲存空間

本機開發Asset compute專案需要雲端儲存空間。

當Asset compute背景工作部署至Adobe I/O Runtime以供AEMas a Cloud Service直接使用時，並非嚴格要求使用此雲端儲存空間，因為AEM提供的雲端儲存空間可讀取資產，並將其寫入轉譯。

### Microsoft Azure Blob儲存{#azure-blob-storage}

如果您尚無權存取Microsoft Azure Blob儲存，請註冊 [免費12個月帳戶](https://azure.microsoft.com/en-us/free/).

不過，本教學課程將使用Azure Blob儲存 [Amazon S3](#amazon-s3) 只能使用本教學課程的微幅變數。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_點進設定Azure Blob儲存（無音頻）_

1. 登入 [Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/).
1. 導覽至 __儲存帳戶__ Azure服務部分
1. 點選 __+新增__ 建立新Blob儲存帳戶的方式
1. 建立新 __資源組__ 視需要，例如： `aem-as-a-cloud-service`
1. 提供 __儲存帳戶名稱__，例如： `aemguideswkndassetcomput`
   + 此 __儲存帳戶名稱__  用於 [配置雲儲存](../develop/environment-variables.md) 在本地Asset compute開發工具中
   + 此 __存取金鑰__ 與儲存帳戶相關聯時，亦需 [配置雲儲存](../develop/environment-variables.md).
1. 將其他項目保留為預設值，然後點選 __檢閱+建立__ 按鈕
   + （可選）選擇 __位置__ 離你很近。
1. 檢閱布建請求以了解正確性，然後點選 __建立__ 按鈕

### Amazon S3{#amazon-s3}

使用 [Microsoft Azure Blob儲存](#azure-blob-storage) 不過，建議您完成本教學課程 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可使用。

若使用Amazon S3儲存空間，請在 [設定專案的環境變數](../develop/environment-variables.md#amazon-s3).

如果您需要特別針對本教學課程布建雲端儲存空間，建議您使用 [Azure Blob儲存](#azure-blob-storage).
