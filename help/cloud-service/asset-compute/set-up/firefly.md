---
title: 為資產計算擴充性設定Adobe Project Firefly
description: 「資產計算」專案是特別定義的Adobe Project Firefly專案，因此，您必須在Adobe Developer Console中存取Adobe Project Firefly，才能設定和部署這些項目。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# 設定Adobe Project Firefly

「資產計算」專案是特別定義的Adobe Project Firefly專案，因此，您必須在Adobe Developer Console中存取Adobe Project Firefly，才能設定和部署這些項目。

## 在Adobe Developer Console中建立和設定Adobe Project Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_設定Adobe Project Firefly的點進（無音效）_

1. 使用與布建的[帳戶和服務](./accounts-and-services.md)相關聯的Adobe ID登入[Adobe Developer Console](https://console.adobe.io)。 請確定您是&#x200B;__系統管理員__&#x200B;或&#x200B;__開發人員角色__&#x200B;中正確的Adobe組織。
1. 點選&#x200B;__「建立新專案>從範本建立專案>專案Firefly__」以建立Firefly專案

   _如果「建__&#x200B;立新&#x200B;__專案」按鈕或__「專案&#x200B;__Firephytype」不可用，表示您的Adobe組織未 [布建Project Firefly](#request-adobe-project-firefly)。_

   + __專案標題__:  `WKND AEM Asset Compute`
   + __應用程式名稱__:  `wkndAemAssetCompute<YourName>`
      + __應用程式名稱__&#x200B;在所有Firefly專案中都必須是唯一的，以後無法修改。 在公司或組織名稱前加上前置詞，加上有意義的後置詞是個好方法，例如：`wkndAemAssetCompute`。
      + 為了自我啟用，通常最好將您的名稱張貼至&#x200B;__應用程式名稱__，例如`wkndAemAssetComputeJaneDoe`，以避免與其他Project Firefly專案發生衝突。
   + 在&#x200B;__Workspaces__&#x200B;下添加名為`Development`的新環境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，請確定已選取「包含執行階段及每個工作區&#x200B;__」__
   + 點選&#x200B;__儲存__&#x200B;以儲存專案
1. 在Adobe Firefly專案中，從工作區選擇器中選取`Development`
1. 點選「__+新增服務> API__」以開啟「新增API __精靈」，使用此方法新增下列API:__

   + __Experience Cloud >資產計算__
      + 選擇&#x200B;__產生鍵對__&#x200B;並點選&#x200B;__產生鍵對__&#x200B;按鈕，將下載的`config.zip`儲存至安全位置，以供稍後使用](#private-key)[
      + 點選&#x200B;__Next__
      + 選擇產品設定檔&#x200B;__Integrations - Cloud Service__，然後點選&#x200B;__Save configured API__
   + __「Adobe服務> I/O事件」，然__ 後點選「儲 __存設定的API」__
   + __Adobe服務> I/O管理__ API並點選「儲 __存設定的API」__

## 訪問private.key{#private-key}

設定[資產計算API整合](#set-up)時，會產生新的金鑰對，並自動下載`config.zip`檔案。 此`config.zip`包含生成的公共證書和匹配的`private.key`檔案。

1. 將`config.zip`解壓縮至檔案系統上的安全位置，因為`private.key`是[稍後使用的](../develop/environment-variables.md)
   + Git絕不能為了安全而增加機密和私密金鑰。

## 查看服務帳戶(JWT)憑據

本Adobe I/O專案的認證由本機[資產計算開發工具](../develop/development-tool.md)使用，以與Adobe I/O Runtime互動，且必須整合在資產計算專案中。 熟悉服務帳戶(JWT)認證。

![Adobe開發人員服務帳戶認證](./assets/firefly/service-account.png)

1. 從Adobe I/O Project Firefly專案，確定已選取`Development`工作區
1. 在&#x200B;__憑據__&#x200B;下點選&#x200B;__服務帳戶(JWT)__
1. 檢視顯示的Adobe I/O認證
   + 底部列出的&#x200B;__公鑰__&#x200B;在&#x200B;__資產計算API__&#x200B;添加到此項目時下載的`config.zip`__private.key__&#x200B;對應項。
      + 如果私密金鑰遺失或洩露，則可移除相符的公開金鑰，並使用此介面在Adobe I/O中產生新的金鑰對或上傳至Adobe I/O。
