---
title: 設定Asset Compute擴充性的帳戶和服務
description: 開發Asset Compute背景工作時需要存取帳戶和服務，包括AEM as a Cloud Service、App Builder，以及Microsoft或Amazon提供的雲端儲存空間。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# 設定帳戶和服務

本教學課程需要布建下列服務，並可透過學習者的Adobe ID存取。

所有Adobe服務都必須透過相同的Adobe組織，使用您的Adobe ID進行存取。

+ [AEM as a Cloud Service ](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 布建作業可能需要2到10天的時間
+ 雲端儲存空間
   + [Azure Blob儲存體](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>繼續學習本教學課程之前，請先確認您具備上述所有服務的存取權。
> 
> 請檢閱以下章節，瞭解如何設定及布建必要的服務。

## AEM as a Cloud Service {#aem-as-a-cloud-service}

需要存取AEM as a Cloud Service環境，才能設定AEM Assets處理設定檔以叫用自訂Asset Compute背景工作。

理想情況下，沙箱計畫或非沙箱開發環境可供使用。

請注意，本機AEM SDK並不足以完成本教學課程，因為本機AEM SDK無法與Asset Compute微服務通訊，而是需要真正的AEM as a Cloud Service環境。

## App Builder{#app-builder}

[App Builder](https://developer.adobe.com/app-builder/)架構是用來建置自訂動作並部署至Adobe的無伺服器平台Adobe I/O Runtime。 AEM Asset Compute專案是特別建立的App Builder專案，可透過處理設定檔與AEM Assets整合，並提供存取及處理資產二進位檔的功能。

若要存取App Builder，請註冊預覽。

1. [註冊App Builder試用版](https://developer.adobe.com/app-builder/trial/)。
1. 請等候約2 - 10天，直到透過電子郵件通知您已布建帳戶，再繼續本教學課程。
   + 如果您不確定是否已布建，請繼續後續步驟，如果您無法在[App Builder](https://developer.adobe.com/console/)中建立&#x200B;__Adobe Developer Console__&#x200B;專案，您仍尚未布建。

## 雲端儲存空間

Asset Compute專案的本機開發需要雲端儲存空間。

將Asset Compute背景工作部署到Adobe I/O Runtime以供AEM as a Cloud Service直接使用時，此雲端儲存空間並非絕對必要，因為AEM提供用來讀取資產及寫入轉譯的雲端儲存空間。

### Microsoft Azure Blob儲存體{#azure-blob-storage}

如果您尚無法存取Microsoft Azure Blob Storage，請註冊[免費的12個月帳戶](https://azure.microsoft.com/en-us/free/)。

本教學課程將使用Azure Blob儲存體，但也可使用[Amazon S3](#amazon-s3)，且只能在本教學課程中稍作變動。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_布建Azure Blob儲存體的點進（無音訊）_

1. 登入您的[Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/)。
1. 瀏覽至&#x200B;__儲存體帳戶__ Azure服務區段
1. 點選&#x200B;__+新增__&#x200B;以建立新的Blob儲存體帳戶
1. 視需要建立新的&#x200B;__資源群組__，例如： `aem-as-a-cloud-service`
1. 提供&#x200B;__儲存體帳戶名稱__，例如： `aemguideswkndassetcomput`
   + 用於在本機Asset Compute開發工具中[設定雲端儲存空間](../develop/environment-variables.md)的&#x200B;__儲存體帳戶名稱__
   + 在[設定雲端儲存空間](../develop/environment-variables.md)時，也需要與儲存體帳戶相關聯的&#x200B;__存取金鑰__。
1. 保留其他專案為預設值，然後點選「__檢閱+建立__」按鈕
   + 選擇性地選取靠近您的&#x200B;__位置__。
1. 檢閱布建要求是否正確，如果符合要求，請點選&#x200B;__建立__&#x200B;按鈕

### Amazon S3{#amazon-s3}

建議您使用[Microsoft Azure Blob Storage](#azure-blob-storage)完成本教學課程，不過也可以使用[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)。

如果使用Amazon S3儲存空間，請在[設定專案的環境變數](../develop/environment-variables.md#amazon-s3)時，指定Amazon S3雲端儲存空間認證。

如果您需要為此教學課程專門布建雲端儲存空間，建議您使用[Azure Blob儲存空間](#azure-blob-storage)。
