---
title: 為Asset compute擴充性設定App Builder
description: asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer Console中存取App Builder才能設定和部署它們。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# 設定App Builder

asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer Console中存取App Builder才能設定和部署它們。

## 在Adobe Developer Console中建立和設定App Builder{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_設定App Builder的點進（無音訊）_

1. 使用與已布建[帳戶和服務](./accounts-and-services.md)相關聯的Adobe ID登入[Adobe Developer Console](https://console.adobe.io)。 請確定您是&#x200B;__系統管理員__&#x200B;或屬於正確Adobe組織的&#x200B;__開發人員角色__。
1. 點選「__建立新專案>從範本建立專案> App Builder__」以建立App Builder專案

   _如果__&#x200B;建立新專案&#x200B;__按鈕或__ App Builder __型別無法使用，表示您的Adobe組織未[布建為App Builder](#request-adobe-project-app-builder)。_

   + __專案標題__： `WKND AEM Asset Compute`
   + __應用程式名稱__： `wkndAemAssetCompute<YourName>`
      + __應用程式名稱__&#x200B;在所有FApp Builderirefly專案中必須是唯一的，且之後不可修改。 在您的公司或組織名稱加上前置字元，並以有意義的尾碼加上前置字元是一種好方法，例如： `wkndAemAssetCompute`。
      + 若要自行啟用，通常最好將您的名稱后置至&#x200B;__應用程式名稱__ （例如`wkndAemAssetComputeJaneDoe`），以避免與其他App Builder專案發生衝突。
   + 在&#x200B;__工作區__&#x200B;下新增名稱為`Development`的環境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;底下，確定已選取&#x200B;__包含每個工作區的執行階段__
   + 點選&#x200B;__儲存__&#x200B;以儲存專案
1. 在App Builder專案中，從工作區選擇器中選取`Development`
1. 點選「__+新增服務> API__」以開啟「__新增API__」精靈，使用此方法新增下列API：

   + __Experience Cloud>Asset compute__
      + 選取「__產生金鑰組__」並點選「__產生金鑰組__」按鈕，然後將下載的`config.zip`儲存到安全的位置，以供[稍後使用](#private-key)
      + 點選&#x200B;__下一步__
      + 選取產品設定檔&#x200B;__整合 — Cloud Service__，然後點選&#x200B;__儲存設定的API__
   + __Adobe服務> I/O事件__&#x200B;並點選&#x200B;__儲存已設定的API__
   + __Adobe服務> I/O管理API__&#x200B;並點選&#x200B;__儲存已設定的API__

## 存取private.key{#private-key}

設定[Asset computeAPI整合](#set-up)時，已產生新的金鑰組，且已自動下載`config.zip`檔案。 此`config.zip`包含產生的公開憑證和相符的`private.key`檔案。

1. 將`config.zip`解壓縮至檔案系統上的安全位置，因為`private.key`稍後會[使用](../develop/environment-variables.md)
   + 請勿將秘密和私密金鑰新增至Git的安全性事宜。

## 檢閱服務帳戶(JWT)認證

本機[Adobe I/O開發工具](../develop/development-tool.md)使用此Asset compute專案的認證來與Adobe I/O Runtime互動，並且需要合併到Asset compute專案中。 請熟悉「服務帳戶」(JWT)憑證。

![Adobe Developer服務帳戶認證](./assets/app-builder/service-account.png)

1. 從Adobe I/O專案App Builder專案中，確定已選取`Development`工作區
1. 點選&#x200B;__認證__&#x200B;下的&#x200B;__服務帳戶(JWT)__
1. 檢閱顯示的Adobe I/O證明資料
   + 在底部列出的&#x200B;__公開金鑰__&#x200B;在將&#x200B;__Asset computeAPI__&#x200B;新增至此專案時，其下載的`config.zip`中的&#x200B;__private.key__&#x200B;對應專案。
      + 如果私密金鑰遺失或受損，可以移除相符的公開金鑰，並使用此介面在中產生或上傳到Adobe I/O的新金鑰組。
