---
title: 設定AdobeProject Firefly以提升Asset compute擴充性
description: asset compute專案是特別定義的AdobeProject Firefly專案，因此需要在Adobe開發人員控制台中存取AdobeProject Firefly，才能加以設定和部署。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# 設定Adobe專案Firefly

asset compute專案是特別定義的AdobeProject Firefly專案，因此需要在Adobe開發人員控制台中存取AdobeProject Firefly，才能加以設定和部署。

## 在Adobe開發人員控制台中建立和設定Adobe專案Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_點進設定AdobeProject Firefly（無音訊）_

1. 使用與已布建[帳戶和服務](./accounts-and-services.md)相關聯的Adobe ID，登入[Adobe開發人員控制台](https://console.adobe.io)。 確保您是&#x200B;__系統管理員__，或&#x200B;__開發人員角色__&#x200B;中，以取得正確的Adobe組織。
1. 點選&#x200B;__從範本建立新專案>專案> Project Firefly__，建立Firefly專案

   _如果「建__&#x200B;立新&#x200B;__專案」按鈕或「專__&#x200B;案&#x200B;__反向連結類型」無法使用，這表示您的Adobe組 [織未布建Project Firefly](#request-adobe-project-firefly)。_

   + __專案標題__:  `WKND AEM Asset Compute`
   + __應用程式名稱__:  `wkndAemAssetCompute<YourName>`
      + __應用程式名稱__&#x200B;在所有Firefly專案中必須是唯一的，且以後無法修改。 在公司或組織的名稱加上前置詞，並以有意義的尾碼加上後置詞是個不錯的方法，例如：`wkndAemAssetCompute`。
      + 若要自行啟用，通常最好將您的名稱貼文至&#x200B;__應用程式名稱__，例如`wkndAemAssetComputeJaneDoe`，以避免與其他Project Firefly專案發生衝突。
   + 在&#x200B;__Workspaces__&#x200B;下，新增名為`Development`的新環境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，確保已選取每個工作區&#x200B;__的__&#x200B;包含執行階段
   + 點選&#x200B;__儲存__&#x200B;以儲存專案
1. 在AdobeFirefly專案中，從工作區選取器中選取`Development`
1. 點選&#x200B;__+新增服務> API__&#x200B;以開啟&#x200B;__新增API__&#x200B;精靈，使用此方法來新增下列API:

   + __Experience Cloud>Asset compute__
      + 選擇「__生成密鑰對__」，點選「__生成密鑰對__」按鈕，並將下載的`config.zip`保存到安全位置，以供[稍後使用](#private-key)
      + 點選&#x200B;__Next__
      + 選取產品設定檔&#x200B;__整合 — Cloud Service__&#x200B;並點選&#x200B;__儲存已設定的API__
   + __Adobe服務> I/O事件，然__ 後點選「儲 __存已設定的API」__
   + __Adobe服務> I/O管理API，然__ 後點選「儲 __存已設定API」__

## 存取private.key{#private-key}

設定[Asset computeAPI整合](#set-up)時，產生新的金鑰組，並自動下載`config.zip`檔案。 此`config.zip`包含生成的公用證書和匹配的`private.key`檔案。

1. 將`config.zip`解壓縮至檔案系統上的安全位置，因為`private.key`是[稍後使用](../develop/environment-variables.md)
   + Git絕不應以安全性為考量新增機密和私密金鑰。

## 查看服務帳戶(JWT)憑據

本機[Adobe I/O開發工具](../develop/development-tool.md)會使用此Asset compute專案的認證來與Adobe I/O Runtime互動，且需要整合至Asset compute專案。 請熟悉服務帳戶(JWT)憑證。

![Adobe開發人員服務帳戶憑證](./assets/firefly/service-account.png)

1. 在Adobe I/OProject Firefly專案中，確定已選取`Development`工作區
1. 點選&#x200B;__Credentials__&#x200B;底下的&#x200B;__服務帳戶(JWT)__
1. 查看顯示的Adobe I/O憑據
   + 底部列出的&#x200B;__公開金鑰__&#x200B;在&#x200B;__Asset computeAPI__&#x200B;新增至此專案時，下載的`config.zip`中對應的&#x200B;__private.key__。
      + 如果私密金鑰丟失或洩露，則可以刪除匹配的公鑰，並使用此介面在中生成或上傳到Adobe I/O的新密鑰對。
