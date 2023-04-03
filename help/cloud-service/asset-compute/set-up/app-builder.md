---
title: 設定App Builder以提升Asset compute擴充性
description: asset compute專案是特別定義的App Builder專案，因此需要存取Adobe Developer Console中的App Builder，才能加以設定和部署。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# 設定應用程式產生器

asset compute專案是特別定義的App Builder專案，因此需要存取Adobe Developer Console中的App Builder，才能加以設定和部署。

## 在Adobe Developer Console中建立和設定App Builder{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_點進設定App Builder（無音訊）_

1. 登入 [Adobe Developer Console](https://console.adobe.io) 使用與布建之 [帳戶與服務](./accounts-and-services.md). 確定您是 __系統管理員__ 或 __開發人員角色__ 以取得正確的Adobe組織。
1. 點選「 」以建立App Builder專案 __從範本> App Builder建立新專案>專案__

   _若__&#x200B;建立新專案&#x200B;__按鈕或__ App Builder __類型無法使用，這表示您的Adobe組織不可用 [已布建App Builder](#request-adobe-project-app-builder)._

   + __專案標題__: `WKND AEM Asset Compute`
   + __應用程式名稱__: `wkndAemAssetCompute<YourName>`
      + 此 __應用程式名稱__ 在所有FApp Builderrefly專案中必須是唯一的，且以後無法修改。 在公司或組織名稱前加上前置詞，並以有意義的尾碼加上後置詞是個不錯的方法，例如： `wkndAemAssetCompute`.
      + 若要自行啟用，通常最好將您的名稱后置至 __應用程式名稱__，例如 `wkndAemAssetComputeJaneDoe` 以避免與其他App Builder專案衝突。
   + 在 __工作區__ 新增新環境，命名為 `Development`
   + 在 __Adobe I/O Runtime__ 確保 __包含每個工作區的執行階段__ 已選取
   + 點選 __儲存__ 儲存專案
1. 在App Builder專案中，選取 `Development` 從工作區選取器
1. 點選 __+新增服務> API__ 開啟 __新增API__ 精靈中，使用此方法來新增下列API:

   + __Experience Cloud>Asset compute__
      + 選擇 __產生金鑰組__ 然後點選 __產生鍵對__ 按鈕，然後保存下載的 `config.zip` 安全地點 [稍後使用](#private-key)
      + 點選 __下一個__
      + 選取產品設定檔 __整合 — Cloud Service__ 點選 __儲存已設定的API__
   + __Adobe服務> I/O事件__ 點選 __儲存已設定的API__
   + __Adobe服務> I/O管理API__ 點選 __儲存已設定的API__

## 存取private.key{#private-key}

設定 [asset computeAPI整合](#set-up) 產生新的密鑰對，並 `config.zip` 檔案已自動下載。 此 `config.zip` 包含產生的公開憑證和比對 `private.key` 檔案。

1. 解壓縮 `config.zip` 將檔案系統上的安全位置 `private.key` is [稍後使用](../develop/environment-variables.md)
   + Git絕不應以安全性為考量新增機密和私密金鑰。

## 查看服務帳戶(JWT)憑據

本機會會使用此Adobe I/O專案的認證 [asset compute開發工具](../develop/development-tool.md) 與Adobe I/O Runtime互動，且需要整合至Asset compute專案。 請熟悉服務帳戶(JWT)憑證。

![Adobe Developer服務帳戶憑證](./assets/app-builder/service-account.png)

1. 在Adobe I/O專案應用程式產生器專案中，確定 `Development` 已選取工作區。
1. 點選 __服務帳戶(JWT)__ 在 __憑證__
1. 查看顯示的Adobe I/O憑據
   + 此 __公開金鑰__ 在底部有 __private.key__ 對應 `config.zip` 下載時間 __asset computeAPI__ 已新增至此專案。
      + 如果私密金鑰丟失或洩露，則可以刪除匹配的公鑰，並使用此介面在中生成或上傳到Adobe I/O的新密鑰對。
