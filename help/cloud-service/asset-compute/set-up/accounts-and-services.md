---
title: 為資產計算擴充性設定帳戶和服務
description: 開發資產計算工作者需要存取帳戶和服務，包括AEM作為雲端服務、Adobe Project Firefly以及Microsoft或Amazon提供的雲端儲存空間。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# 設定帳戶和服務

本教學課程需要提供下列服務，並透過學員的Adobe ID加以存取。

所有Adobe服務都必須透過相同的Adobe組織，使用您的Adobe ID才能存取。

+ [AEM 雲端服務](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + 資源調配需要2 - 10天
+ 雲端儲存空間
   + [Azure Blob儲存空間](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在繼續本教學課程之前，請確定您可存取上述所有服務。
> 
> 檢閱以下章節，瞭解如何設定和布建所需服務。

## AEM 雲端服務{#aem-as-a-cloud-service}

必須存取AEM作為雲端服務環境，才能設定AEM資產處理設定檔以叫用自訂資產計算工作者。

最理想的情況是提供沙盒程式或非沙盒開發環境。

請注意，本機AEM SDK不足以完成本教學課程，因為本機AEM SDK無法與Asset Compute microservices通訊，因此需要真正的AEM做為雲端服務環境。

## Adobe Project Firefly{#adobe-project-firefly}

Adobe [Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) framework可用來建立自訂動作，並將之部署至Adobe的無伺服器平台Adobe I/O Runtime。 AEM Asset Compute專案是特別建立的Firefly專案，可透過「處理設定檔」與AEM Assets整合，並提供存取和處理資產二進位檔的功能。

若要存取Project Firefly，請註冊預覽。

1. [註冊專案Firefly預覽](https://adobeio.typeform.com/to/obqgRm)。
1. 請等待約2 - 10天，直到收到電子郵件通知您已布建，然後再繼續教學課程。
   + 如果您不確定您是否已布建，請繼續後續步驟，如果您無法在 __Adobe Developer Console中建立__ Project Firefly [專案](https://console.adobe.io) ，您仍未布建。

## 雲端儲存空間

本地開發資產計算專案需要雲端儲存空間。

當資產計算工作者部署至Adobe I/O Runtime以供AEM直接用作雲端服務時，並非嚴格要求此雲端儲存空間，因為AEM提供的雲端儲存空間可讀取資產並寫入其轉譯。

### Microsoft Azure Blob儲存空間{#azure-blob-storage}

如果您尚未擁有Microsoft Azure Blob儲存空間的存取權，請註冊12個月 [免費帳戶](https://azure.microsoft.com/en-us/free/)。

本教學課程將使用Azure Blob Storage，但 [Amazon S3](#amazon-s3) ，也只能使用本教學課程的小變數。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_點進Azure Blob儲存空間布建（無音訊）_


1. 登入您的 [Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/)。
1. 導覽至「儲 __存帳戶__ Azure服務」區段
1. 點選 __+新增__ ，以建立新的Blob儲存帳戶
1. 根據需要 __建立新的資源組__ ，例如： `aem-as-a-cloud-service`
1. 提供存 __儲帳戶名稱__，例如： `aemguideswkndassetcomput`
   + 儲 __存帳戶名稱__ ，將用於為本機資 [產計算開發工具配置雲端儲存空間](../develop/environment-variables.md) 。
   + 設定 __雲端儲存__ ，也需要與儲存帳戶相 [關的存取金鑰](../develop/environment-variables.md)。
1. 將其他項目保留為預設值，然後點選「 __Review +建立__ 」按鈕
   + （可選）選擇 __靠近您__ 的位置。
1. 檢視布建要求是否正確，若符合要求，請點選「 __建立__ 」按鈕

### Amazon S3{#amazon-s3}

建議 [使用Microsoft Azure Blob Storage](#azure-blob-storage) ，以完成本教學課程，但 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可使用。

如果使用Amazon S3儲存，請在配置項目的環境變數時指 [定Amazon S3雲儲存憑據](../develop/environment-variables.md#amazon-s3)。

如果您需要特別為本教學課程布建雲端儲存空間，我們建議您使用 [Azure Blob儲存空間](#azure-blob-storage)。
