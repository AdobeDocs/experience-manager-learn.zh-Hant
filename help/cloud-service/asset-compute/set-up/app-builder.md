---
title: 設定App Builder以提供Asset compute擴充功能
description: asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer主控台中存取App Builder，才能設定和部署這些專案。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 240
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# 設定App Builder

asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer主控台中存取App Builder，才能設定和部署這些專案。

## 在Adobe Developer主控台中建立和設定App Builder{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_設定App Builder的點進（無音訊）_

1. 登入 [Adobe Developer Console](https://console.adobe.io) 使用與布建的Adobe ID相關聯 [帳戶和服務](./accounts-and-services.md). 確定您是 __系統管理員__ 或在 __開發人員角色__ 以取得正確的Adobe組織。
1. 點選以建立應用程式產生器專案 __建立新專案>從範本建立專案> App Builder__

   _若以下其中一個__&#x200B;建立新專案&#x200B;__按鈕或__ App Builder __型別無法使用，這表示您的Adobe組織無法使用 [已布建App Builder](#request-adobe-project-app-builder)._

   + __專案標題__： `WKND AEM Asset Compute`
   + __應用程式名稱__： `wkndAemAssetCompute<YourName>`
      + 此 __應用程式名稱__ 在所有FApp Builderirefly專案中必須是唯一的，且之後不可修改。 在您的公司或組織名稱加上前置詞，並在結尾加上有意義的尾碼是一種好方法，例如： `wkndAemAssetCompute`.
      + 若要自行啟用，通常最好將您的名稱后置至 __應用程式名稱__，例如 `wkndAemAssetComputeJaneDoe` 以避免與其他App Builder專案發生衝突。
   + 在 __工作區__ 新增名為的環境 `Development`
   + 在 __Adobe I/O Runtime__ 確保 __包含每個工作區的執行階段__ 已選取
   + 點選 __儲存__ 儲存專案
1. 在App Builder專案中選取 `Development` 從工作區選擇器
1. 點選 __+新增服務> API__ 以開啟 __新增API__ 精靈，使用此方法新增以下API：

   + __Experience Cloud>Asset compute__
      + 選取 __產生金鑰組__ 然後點選 __產生金鑰組__ 按鈕，並儲存下載的 `config.zip` 到安全位置 [稍後使用](#private-key)
      + 點選 __下一個__
      + 選取產品設定檔 __整合 — Cloud Service__ 然後點選 __儲存已設定的API__
   + __Adobe服務> I/O事件__ 然後點選 __儲存已設定的API__
   + __Adobe服務> I/O管理API__ 然後點選 __儲存已設定的API__

## 存取private.key{#private-key}

設定時 [asset compute API整合](#set-up) 已產生新的金鑰組，且 `config.zip` 檔案已自動下載。 這個 `config.zip` 包含產生的公開憑證和相符專案 `private.key` 檔案。

1. 解壓縮 `config.zip` 到檔案系統的安全位置，作為 `private.key` 是 [稍後使用](../develop/environment-variables.md)
   + 請勿將秘密和私密金鑰新增至Git的安全性事宜。

## 檢閱服務帳戶(JWT)認證

本機會使用此Adobe I/O專案的認證 [asset compute開發工具](../develop/development-tool.md) 若要與Adobe I/O Runtime互動，和需要併入Asset compute專案。 請熟悉「服務帳戶」(JWT)憑證。

![Adobe Developer服務帳戶認證](./assets/app-builder/service-account.png)

1. 從Adobe I/O Project App Builder專案，確認 `Development` 已選取工作區
1. 點選 __服務帳戶(JWT)__ 在 __認證__
1. 檢閱顯示的Adobe I/O證明資料
   + 此 __公開金鑰__ 在底部列出 __private.key__ 中的對應專案 `config.zip` 下載時機 __ASSET COMPUTE API__ 已新增至此專案。
      + 如果私密金鑰遺失或受損，可以移除相符的公開金鑰，並使用此介面在中產生或上傳到Adobe I/O的新金鑰組。
