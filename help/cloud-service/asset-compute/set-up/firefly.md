---
title: 為資產計算擴充性設定Adobe Project Firefly
description: 資產計算應用程式是特別定義的Adobe Project Firefly應用程式，因此需要在Adobe Developer Console中存取Adobe Project Firefly，才能設定和部署它們。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---


# 設定Adobe Project Firefly

資產計算應用程式是特別定義的Adobe Project Firefly應用程式，因此需要在Adobe Developer Console中存取Adobe Project Firefly，才能設定和部署它們。

## 在Adobe Developer Console中建立和設定Adobe Project Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_設定Adobe Project Firefly的點進（無音效）_

1. 使用與布建 [帳戶和服務相關聯的Adobe ID](https://console.adobe.io) ，登入 [Adobe Developer Console](./accounts-and-services.md)。 請確定您是系統管 __理員__ ，或是正確Adobe組織 __的「開發人員角色__ 」。
1. 點選「建立新專案>從範本 __建立專案>專案Firefly」，以建立Firefly專案__

   _如果「__&#x200B;建立新專案&#x200B;__」按鈕或「__ Project Firefly __」類型不可用，表示您的Adobe組織未[布建Project Firefly](#request-adobe-project-firefly)。_

   + __專案標題__: `WKND AEM Asset Compute`
   + __應用程式名稱__: `wkndAemAssetCompute<YourName>`
      + 應用 __程式名稱__ ，在所有Firefly應用程式中必須是唯一的，以後無法修改。 在公司或組織名稱前加上前置詞，加上有意義的後置詞是個好方法，例如： `wkndAemAssetCompute`.
      + 若要自我啟用，通常最好將您的名稱貼至應用程式名 __稱__，例如 `wkndAemAssetComputeJaneDoe` 避免與其他Project Firefly應用程式衝突。
   + 在「工 __作區__ 」(Workspaces)下，新增名為 `Development`
   + 在「 __Adobe I/O Runtime__ 」下，請確 __定已選取「包含執行時期__ 」及每個工作區
   + 點選「 __儲存__ 」以儲存專案
1. 在Adobe Firefly專案中，從工作區選 `Development` 取器中選取
1. 點選 __+ 「新增服務> API__ 」以開啟「 ____ 新增API精靈」，使用此方法新增下列API:

   + __Experience Cloud >資產計算__
      + 選 __取「產生金鑰對__ 」並點選「產生金鑰對 __」按鈕，將下載的內容儲存__ 至安全位置以供 `config.zip`[日後使用](#private-key)
      + 點選下 __一步__
      + 選取產品設定檔 __整合——雲端服務__ ，並點選 __儲存設定的API__
   + __Adobe服務> I/O事件__ ，並點選「儲 __存設定的API」__
   + __「Adobe服務> I/O管理API」__ ，並點選「儲 __存設定的API」__

## 存取private.key{#private-key}

設定資產 [計算API整合時](#set-up) ，會產生新的金鑰對，並自 `config.zip` 動下載檔案。 這包 `config.zip` 含產生的公用憑證和相符 `private.key` 檔案。

1. 在 `config.zip` 稍後使用時，解壓縮至檔案系統 `private.key` 上的 [安全位置](../develop/environment-variables.md)
   + Git絕不能為了安全而增加機密和私密金鑰。

## 查看服務帳戶(JWT)憑據

本機 [Asset Compute Development Tool](../develop/development-tool.md) （資產計算開發工具）會使用此Adobe I/O專案的認證來與Adobe I/O Runtime互動，而且需要將其整合在資產計算應用程式專案中。 熟悉服務帳戶(JWT)認證。

![Adobe開發人員服務帳戶認證](./assets/firefly/service-account.png)

1. 從Adobe I/O Project Firefly專案中，確定已選取 `Development` 工作區
1. 在「認證 __」下點選「服務帳戶(JWT)__ 」 ____
1. 檢視顯示的Adobe I/O認證
   + 底部 __列出的公開金鑰__ ，在將資產計算API新增至此專案時，其已下載的 __公開金鑰中會有private.key__`config.zip`____ 。
      + 如果私密金鑰遺失或洩露，則可移除相符的公開金鑰，並使用此介面在Adobe I/O中產生新的金鑰對或上傳至Adobe I/O。
