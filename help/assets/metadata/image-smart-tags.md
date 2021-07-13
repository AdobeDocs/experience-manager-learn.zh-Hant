---
title: 使用AEM Assets的影像智慧標籤
description: 影像的智慧標籤會根據影像內容自動聰明地新增中繼資料標籤至影像資產，以增強AEM搜尋功能。
topic: 內容管理
feature: 智慧標記
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 645
thumbnail: 17019.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 4%

---


# 影像智慧標籤

AEM Assets的影像智慧標籤可自動將衍生中繼資料標籤新增至影像資產，進而增強AEM Assets的搜尋，更輕鬆快速地尋找正確的影像，改善製作體驗。

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## 為AEM 6.x設定{#set-up}

>[!NOTE]
> 影像的智慧標籤會自動布建給AEM作為Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

使用智慧內容服務之前，請確定下列項目以建立Adobe I/O上的整合：

* 具有組織管理員權限的Adobe ID帳戶
* 貴組織已啟用智慧內容服務

影片詳細說明設定用於智慧標籤影像的Adobe I/O智慧內容服務所需的下列工作。

* 在AEM中建立智慧內容服務設定以產生公開金鑰。 取得公開憑證以進行 OAuth 整合。
* 在Adobe I/O中建立整合，並上傳產生的公開金鑰。
* 使用API金鑰和其他來自Adobe I/O的認證，設定AEM執行個體。
* （可選）在資產上傳時啟用自動標籤。

## 其他資源

* [AEM Assets智慧標籤檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
