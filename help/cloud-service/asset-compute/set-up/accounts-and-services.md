---
title: 設定帳戶和服務以實現Asset compute擴展
description: 開發Asset compute員工需要訪問客戶和服務，AEM包括Microsoft或Amazon提供的as a Cloud Service、應用構建器和雲儲存。
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

本教程要求提供以下服務並通過學習者的Adobe ID訪問。

所有Adobe服務都必須通過同一Adobe組織(使用您的Adobe ID)訪問。

+ [AEM as a Cloud Service ](#aem-as-a-cloud-service)
+ [應用程式生成器](#app-builder)
   + 資源調配可能需要2 - 10天
+ 雲儲存
   + [Azure Blob儲存](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在繼續完成本教程之前，請確保您可以訪問上述所有服務。
> 
> 查看下面有關如何設定和提供所需服務的章節。

## AEM as a Cloud Service {#aem-as-a-cloud-service}

要配置AEM AssetsAEM處理配置檔案以調用自定義Asset compute工作程式，需要訪問as a Cloud Service環境。

理想的情況是，可使用沙盒程式或非沙盒開發環境。

請注意，本AEM地SDK不足以完成本教程，因為本地AEMSDK無法與Asset compute的微服務通信，而是需AEM要真正的as a Cloud Service環境。

## 應用程式生成器{#app-builder}

的 [應用程式生成器](https://developer.adobe.com/app-builder/) 框架用於構建和部署自定義操作到Adobe無伺服器平台Adobe I/O Runtime。 AEMAsset compute項目是專門構建的App Builder項目，它通過處理配置檔案與AEM Assets整合，並提供訪問和處理資產二進位檔案的能力。

要獲得對App Builder的訪問權限，請註冊預覽。

1. [註冊App Builder試用版](https://developer.adobe.com/app-builder/trial/)。
1. 請大約等待2 - 10天，直到通過電子郵件通知您已配置您，然後繼續本教程。
   + 如果您不確定是否已設定，請繼續執行後續步驟，如果您無法建立 __應用程式生成器__ 項目 [Adobe Developer控制台](https://developer.adobe.com/console/) 您尚未設定。

## 雲儲存

本地開發Asset compute項目需要雲儲存。

將Asset compute工作程式部署到Adobe I/O Runtime供as a Cloud Service直接使用AEM時，不嚴格要求此雲儲存，因AEM為提供從中讀取資產和寫入格式副本的雲儲存。

### MicrosoftAzure Blob儲存{#azure-blob-storage}

如果您尚未訪問MicrosoftAzure Blob儲存，請註冊 [免費12個月帳戶](https://azure.microsoft.com/en-us/free/)。

本教程將使用Azure Blob儲存 [AmazonS3](#amazon-s3) 可以同時使用本教程中的少量變體。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_按一下直接設定Azure Blob儲存（無音頻）_

1. 登錄到 [MicrosoftAzure帳戶](https://azure.microsoft.com/en-us/account/)。
1. 導航到 __儲存帳戶__ Azure服務部分
1. 點擊 __+添加__ 建立新的Blob儲存帳戶
1. 新建 __資源組__ 例如： `aem-as-a-cloud-service`
1. 提供 __儲存帳戶名__，例如： `aemguideswkndassetcomput`
   + 的 __儲存帳戶名__  用於 [配置雲儲存](../develop/environment-variables.md) 本地Asset compute開發工具
   + 的 __訪問密鑰__ 與儲存帳戶關聯時也需要 [配置雲儲存](../develop/environment-variables.md)。
1. 將其它所有內容保留為預設，然後點擊 __審閱+建立__ 按鈕
   + （可選）選擇 __位置__ 離你很近。
1. 查看設定請求是否正確，然後點擊 __建立__ 按鈕

### AmazonS3{#amazon-s3}

使用 [MicrosoftAzure Blob儲存](#azure-blob-storage) 不過，建議您完成本教程 [AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可用。

如果使用AmazonS3儲存，請在 [配置項目的環境變數](../develop/environment-variables.md#amazon-s3)。

如果您需要特別為本教程預配雲儲存，建議使用 [Azure Blob儲存](#azure-blob-storage)。
