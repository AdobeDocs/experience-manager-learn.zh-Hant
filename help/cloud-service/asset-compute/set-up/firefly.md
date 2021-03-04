---
title: 設定Adobe專案Firefly以擴充Asset compute
description: asset compute專案是特別定義的Adobe專案Firefly專案，因此需要在Adobe開發人員主控台中存取Adobe專案Firefly，才能設定和部署專案。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---


# 設定Adobe專案Firefly

asset compute專案是特別定義的Adobe專案Firefly專案，因此需要在Adobe開發人員主控台中存取Adobe專案Firefly，才能設定和部署專案。

## 在Adobe開發人員主控台中建立和設定Adobe專案Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_設定Adobe專案Firefly的點進（無音效）_

1. 使用與已布建的[帳戶和服務](./accounts-and-services.md)相關聯的Adobe ID，登入[Adobe開發人員控制台](https://console.adobe.io)。 請確定您是&#x200B;__系統管理員__&#x200B;或&#x200B;__開發人員角色__&#x200B;中的，以取得正確的Adobe組織。
1. 點選&#x200B;__「建立新專案>從範本建立專案>專案Firefly__」以建立Firefly專案

   _如果「__&#x200B;建立新&#x200B;__專案」按鈕或「__&#x200B;專案&#x200B;__反向連結類型」不可用，表示您的Adobe組織未 [布建Project Firefly](#request-adobe-project-firefly)。_

   + __專案標題__:  `WKND AEM Asset Compute`
   + __應用程式名稱__:  `wkndAemAssetCompute<YourName>`
      + __應用程式名稱__&#x200B;在所有Firefly專案中都必須是唯一的，以後無法修改。 在公司或組織名稱前加上前置詞，加上有意義的後置詞是個好方法，例如：`wkndAemAssetCompute`。
      + 為了自我啟用，通常最好將您的名稱張貼至&#x200B;__應用程式名稱__，例如`wkndAemAssetComputeJaneDoe`，以避免與其他Project Firefly專案發生衝突。
   + 在&#x200B;__Workspaces__&#x200B;下添加名為`Development`的新環境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，確保選擇了「包含運行時和每個工作區&#x200B;__」__
   + 點選&#x200B;__儲存__&#x200B;以儲存專案
1. 在AdobeFirefly專案中，從工作區選擇器中選取`Development`
1. 點選「__+新增服務> API__」以開啟「新增API __精靈」，使用此方法新增下列API:__

   + __Experience Cloud>Asset compute__
      + 選擇&#x200B;__產生鍵對__&#x200B;並點選&#x200B;__產生鍵對__&#x200B;按鈕，將下載的`config.zip`儲存至安全位置，以供稍後使用](#private-key)[
      + 點選&#x200B;__Next__
      + 選擇產品配置檔案&#x200B;__整合-Cloud Service__&#x200B;並點選&#x200B;__保存配置的API__
   + __Adobe服務> I/O事件並點__ 選儲存 __已設定的API__
   + __Adobe服務> I/O管理__ API並點選「儲 __存設定的API」__

## 訪問private.key{#private-key}

設定[Asset computeAPI整合](#set-up)時，會產生新的金鑰對，並自動下載`config.zip`檔案。 此`config.zip`包含生成的公共證書和匹配的`private.key`檔案。

1. 將`config.zip`解壓縮至檔案系統上的安全位置，因為`private.key`是[稍後使用的](../develop/environment-variables.md)
   + Git絕不能為了安全而增加機密和私密金鑰。

## 查看服務帳戶(JWT)憑據

本Adobe I/O項目的認證由當地[Asset compute開發工具](../develop/development-tool.md)使用，以與Adobe I/O Runtime互動，並需要納入Asset compute項目。 熟悉服務帳戶(JWT)認證。

![Adobe開發人員服務帳戶認證](./assets/firefly/service-account.png)

1. 從「Adobe I/O項目Firefly」專案，確定已選取`Development`工作區
1. 在&#x200B;__憑據__&#x200B;下點選&#x200B;__服務帳戶(JWT)__
1. 查看顯示的Adobe I/O憑據
   + 底部列出的&#x200B;__公鑰__&#x200B;在&#x200B;__Asset computeAPI__&#x200B;被添加到此項目時下載的`config.zip`__private.key__&#x200B;對應項。
      + 如果私鑰丟失或洩露，則可以刪除匹配的公鑰，並使用此介面在Adobe I/O中生成或上傳新密鑰對。
