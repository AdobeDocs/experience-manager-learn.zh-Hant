---
title: asset computeAEMas a Cloud Service的微服務擴充性
description: 本教學課程將逐步說明如何建立簡單的Asset compute背景工作，透過將原始資產裁切至圓圈來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作程式本身是基本的，但本教學課程會使用它來探索如何建立、開發和部署自訂Asset compute工作程式，以便與AEMas a Cloud Service搭配使用。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# asset compute微服務的可擴充性

AEM asCloud Service的Asset compute微服務支援開發及部署自訂背景工作，這些背景工作可用來讀取和操控儲存在AEM中資產的二進位資料，最常用的是建立自訂資產轉譯。

而在AEM 6.x中，自訂AEM工作流程程式是用來讀取、轉換和回寫資產轉譯，在AEMas a Cloud ServiceAsset compute背景工作中，則可滿足此需求。

## 你將做什麼

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步說明如何建立簡單的Asset compute背景工作，透過將原始資產裁切至圓圈來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作程式本身是基本的，但本教學課程會使用它來探索如何建立、開發和部署自訂Asset compute工作程式，以便與AEMas a Cloud Service搭配使用。

### 目標 {#objective}

1. 配置和設定必要的帳戶和服務以構建和部署Asset compute工作人員
1. 建立和設定Asset compute專案
1. 開發可產生自訂轉譯的Asset compute背景工作
1. 針對撰寫測試，並了解如何對自訂Asset compute背景工作進行除錯
1. 部署Asset compute背景工作，並透過處理設定檔整合it AEMas a Cloud Service製作服務

## 設定

了解如何為擴展Asset compute工作做好適當準備，並了解哪些服務和帳戶必須進行配置和配置，以及本地安裝以進行開發的軟體。

### 帳戶和服務設定{#accounts-and-services}

下列帳戶和服務需要布建和存取，才能完成教學課程、AEMas a Cloud Service開發環境或沙箱方案、App Builder和Microsoft Azure Blob儲存。

+ [提供賬戶和服務](./set-up/accounts-and-services.md)

### 本地開發環境

本地開發Asset compute專案需要特定的開發人員工具集，與傳統AEM開發不同，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支援npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### App Builder

asset compute專案是特別定義的App Builder專案，因此需要存取Adobe Developer Console中的App Builder，才能加以設定和部署。

+ [設定應用程式產生器](./set-up/app-builder.md)

## 開發

了解如何建立和設定Asset compute專案，然後開發可產生定制資產轉譯的自訂背景工作。

### 建立新Asset compute專案

asset compute項目包含一個或多個Asset compute工作，使用交互Adobe I/OCLI生成。 asset compute專案是特別結構化的App Builder專案，而這又是Node.js專案。

+ [建立新Asset compute專案](./develop/project.md)

### 設定環境變數

環境變數會維護於 `.env` 檔案供本機開發使用，並可用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。

+ [設定環境變數](./develop/environment-variables.md)

### 設定manifest.yml

asset compute項目包含清單，它定義項目中包含的所有Asset compute工作，以及部署到Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發員工

開發Asset compute背景工作是擴展Asset compute微服務的核心，因為背景工作包含可產生或協調產生的資產轉譯的自訂代碼。

+ [開發Asset compute工作人員](./develop/worker.md)

### 使用Asset compute開發工具

「Asset compute開發工具」提供本地Web工具，用於部署、執行和預覽工作人員生成的格式副本，支援快速、迭代的Asset compute工作人員開發。

+ [使用Asset compute開發工具](./develop/development-tool.md)

## 測試和除錯

了解如何測試自訂Asset compute背景工作以確保對其操作有信心，並對Asset compute背景工作進行除錯，以了解及疑難排解自訂程式碼的執行方式。

### 測試工作人員

asset compute提供測試框架，用於為背景工作建立測試套裝，使定義測試可確保行為正確易行。

+ [測試工作人員](./test-debug/test.md)

### 調試工作

asset compute背景工作提供傳統 `console.log(..)` 輸出，與整合 __與程式碼__ 和  __wskdebug__，可讓開發人員在工作程式碼即時執行時逐步執行。

+ [調試工作](./test-debug/debug.md)

## 部署

了解如何將自訂Asset compute背景工作與AEMas a Cloud Service整合，方法是先將自訂背景工作部署至Adobe I/O Runtime，然後透過AEM Assets的處理設定檔從AEMas a Cloud Service作者叫用。

### 部署至Adobe I/O Runtime

asset compute背景工作必須部署至Adobe I/O Runtime才能與AEMas a Cloud Service搭配使用。

+ [使用處理設定檔](./deploy/runtime.md)

### 透過AEM處理設定檔整合背景工作

部署至Adobe I/O Runtime後，Asset compute背景工作即可透過以下方式在AEMas a Cloud Service中註冊： [資產處理設定檔](../../assets/configuring/processing-profiles.md). 處理設定檔則會套用至套用至其中資產的資產資料夾。

+ [與AEM處理設定檔整合](./deploy/processing-profiles.md)

## 進階

這些概要教學課程以前幾章建立的基礎學習為基礎，處理更進階的使用案例。

+ [開發Asset compute中繼資料背景工作](./advanced/metadata.md) 可將中繼資料寫回

## Github上的程式碼基底

Github上提供本教學課程的程式碼基底：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始碼不包含必要 `.env` 或 `config.json` 檔案。 您必須使用 [帳戶與服務](#accounts-and-services) 資訊。

## 其他資源

以下是各種Adobe資源，可提供進一步資訊，以及開發Asset compute背景工作時所需的有用API和SDK。

### 文件

+ [asset compute服務檔案](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開發工具讀我檔案](https://github.com/adobe/asset-compute-devtool)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute公域](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [AdobeCloud Blobstore包裝函式程式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點提取重試庫](https://github.com/adobe/node-fetch-retry)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)
