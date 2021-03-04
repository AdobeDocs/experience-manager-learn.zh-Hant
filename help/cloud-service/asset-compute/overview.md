---
title: asset compute微服務AEM的擴充性，
description: 本教學課程將逐步介紹如何建立簡單的Asset compute工作者，透過將原始資產裁切至圓形來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作者本身是基本的，但本教學課程使用它來探索如何建立、開發和部署自訂Asset compute工作者，以便與作AEM為Cloud Service一起使用。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# asset compute微服務擴充性

因AEM為Cloud Service的Asset compute微服務支援開發和部署定制工作者，這些工作者用於讀取和操作儲存在（最常見）中的資產的二進位資料AEM，以建立定制資產轉譯。

而在AEM6.x中，自訂工AEM作流程則用於讀取、轉換和回寫資產轉譯，因為AEMCloud ServiceAsset compute工作者可以滿足此需求。

## 您將做的事

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步介紹如何建立簡單的Asset compute工作者，透過將原始資產裁切至圓形來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作者本身是基本的，但本教學課程使用它來探索如何建立、開發和部署自訂Asset compute工作者，以便與作AEM為Cloud Service一起使用。

### 目標{#objective}

1. 配置和設定必要的帳戶和服務以構建和部署Asset compute員工
1. 建立和設定Asset compute專案
1. 開發可產生自訂轉譯的Asset compute工作者
1. 編寫測試，並瞭解如何對自訂Asset compute工作者除錯
1. 部署Asset compute工作者，並透過處理設定檔將其AEM整合為Cloud Service作者服務

## 設定

瞭解如何正確準備擴充Asset compute工作，並瞭解哪些服務和帳戶必須進行布建和設定，以及本機安裝的軟體以進行開發。

### 帳戶和服務設定{#accounts-and-services}

下列帳戶和服務需要布建和存取權，才能完成教學課程(AEMCloud Service開發環境或沙盒程式)存取AdobeProject Firefly和Microsoft Azure Blob儲存。

+ [提供帳戶和服務](./set-up/accounts-and-services.md)

### 當地開發環境

地方開發Asset compute專案需要特定的開發人員工具集，這與傳統開發AEM不同，包括：Microsoft Visual Studio代碼、Docker Desktop、Node.js和支援npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### AdobeProject Firefly

asset compute專案是特別定義的Adobe專案Firefly專案，因此需要在Adobe開發人員主控台中存取Adobe專案Firefly，才能設定和部署專案。

+ [設定Adobe專案Firefly](./set-up/firefly.md)

## 開發

瞭解如何建立和設定Asset compute專案，然後開發自訂工作者，以產生定制資產轉譯。

### 建立新的Asset compute專案

asset compute項目(包含一個或多個Asset compute工作者)使用交互Adobe I/OCLI生成。 asset compute項目是特殊結構化的Adobe項目Firefly項目，而Node.js項目又是Node.js項目。

+ [建立新的Asset compute專案](./develop/project.md)

### 配置環境變數

環境變數會保留在`.env`檔案中以進行本端開發，並用來提供本端開發所需的Adobe I/O憑證和雲端儲存憑證。

+ [配置環境變數](./develop/environment-variables.md)

### 設定manifest.yml

asset compute項目包含清單，其中定義了項目中包含的所有Asset compute工作人員，以及部署到Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發員工

開發Asset compute工作者是擴展Asset compute微服務的核心，因為工作者包含生成或協調生成的資產轉譯的定製代碼。

+ [培養Asset compute員工](./develop/worker.md)

### 使用Asset compute開發工具

asset compute開發工具提供本機Web控管，用於部署、執行和預覽工作者產生的轉譯，支援快速、反覆的Asset compute工作者開發。

+ [使用Asset compute開發工具](./develop/development-tool.md)

## 測試與除錯

瞭解如何測試自訂Asset compute工作者以確保其運作，並偵錯Asset compute工作者以瞭解並疑難排解自訂代碼的執行方式。

### 測試工作者

asset compute提供測試架構，以建立適用於工作者的測試套裝，讓定義測試可確保行為正確易行。

+ [測試工作者](./test-debug/test.md)

### 對工作者進行除錯

asset compute工作者提供從傳統`console.log(..)`輸出到與&#x200B;__VS代碼__&#x200B;和&#x200B;__wskdebug__&#x200B;整合的各種除錯層級，讓開發人員在工作者代碼執行時逐步執行。

+ [對工作者進行除錯](./test-debug/debug.md)

## 部署

瞭解如何將自訂Asset compute工作者整AEM合為Cloud Service，先將他們部署至Adobe I/O Runtime，再透過AEM Assets的「處理設定檔」以AEMCloud Service作者身分叫用。

### 部署至Adobe I/O Runtime

asset compute工人必須被部署到Adobe I/O Runtime，以AEM作為Cloud Service。

+ [使用處理描述檔](./deploy/runtime.md)

### 透過處理設定檔AEM整合員工

部署到Adobe I/O Runtime後，Asset compute員工可以通AEM過[資產處理配置檔案](../../assets/configuring/processing-profiles.md)以Cloud Service方式註冊。 處理設定檔則套用至套用至資產資料夾的資產。

+ [與處理設定AEM檔整合](./deploy/processing-profiles.md)

## 進階

這些簡略的教學課程，以前幾章中建立的基礎學習為基礎，處理更進階的使用案例。

+ [開發Asset compute中繼](./advanced/metadata.md) 資料工作器，可將中繼資料寫回

## Github上的代碼庫

教學課程的程式碼基底可在Github取得，網址為：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始碼不包含所需的`.env`或`config.json`檔案。 必須使用[帳戶和服務](#accounts-and-services)資訊添加和配置這些。

## 其他資源

以下是各種Adobe資源，可為開發Asset compute工人提供進一步資訊和有用的API和SDK。

### 文件

+ [asset compute服務檔案](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開發工具自述檔案](https://github.com/adobe/asset-compute-devtool)
+ [asset compute示例員工](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute公域](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe雲Blobstore包裝函式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點讀取重試庫](https://github.com/adobe/node-fetch-retry)
+ [asset compute示例員工](https://github.com/adobe/asset-compute-example-workers)
