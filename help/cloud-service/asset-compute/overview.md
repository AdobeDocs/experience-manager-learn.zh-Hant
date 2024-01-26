---
title: AEMas a Cloud Service的Asset compute微服務擴充性
description: 本教學課程將逐步解說如何建立簡易的Asset compute背景工作，此背景工作將原始資產裁切至圓圈以建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身至關重要，但本教學課程會使用它來探索建立、開發和部署自訂Asset compute背景工作，以與AEMas a Cloud Service搭配使用。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 333
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# asset compute微服務的擴充性

AEM as a Cloud Service的Asset compute微服務支援開發及部署自訂背景工作，用於讀取和操控儲存在AEM中之資產的二進位資料，繼而多半用來建立自訂資產轉譯。

在AEM 6.x中，自訂AEM工作流程用於讀取、轉換和回寫資產轉譯，而在AEMas a Cloud ServiceAsset compute背景工作則滿足此需求。

## 您將要執行的動作

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步解說如何建立簡易的Asset compute背景工作，此背景工作將原始資產裁切至圓圈以建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身至關重要，但本教學課程會使用它來探索建立、開發和部署自訂Asset compute背景工作，以與AEMas a Cloud Service搭配使用。

### 目標 {#objective}

1. 布建及設定必要的帳戶和服務，以建置及部署Asset compute工作者
1. 建立及設定Asset compute專案
1. 開發可產生自訂轉譯的Asset compute背景工作
1. 編寫測試，並瞭解如何對自訂Asset compute背景工作進行偵錯
1. 部署Asset compute背景工作，並透過處理設定檔將其整合AEMas a Cloud Service作者服務

## 設定

瞭解如何正確準備擴充Asset compute工作者，並瞭解必須布建和設定哪些服務和帳戶，以及本機安裝軟體以供開發。

### 帳戶和服務布建{#accounts-and-services}

若要完成教學課程、AEMas a Cloud Service開發環境或沙箱程式、存取App Builder和Microsoft Azure Blob儲存空間，下列帳戶和服務需要布建和存取權。

+ [布建帳戶和服務](./set-up/accounts-and-services.md)

### 本機開發環境

asset compute專案的本機開發需要特定開發人員工具集，不同於傳統AEM開發，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支援的npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### App Builder

asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer主控台中存取App Builder，才能設定和部署這些專案。

+ [設定App Builder](./set-up/app-builder.md)

## 開發

瞭解如何建立和設定Asset compute專案，然後開發自訂背景工作，以產生客製化資產轉譯。

### 建立新的Asset compute專案

asset compute專案包含一或多個Asset compute背景工作，並使用互動式Adobe I/OCLI產生。 asset compute專案是特別結構化的App Builder專案，這些專案又稱為Node.js專案。

+ [建立新的Asset compute專案](./develop/project.md)

### 設定環境變數

環境變數會保留在 `.env` 檔案以供本機開發使用，並用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。

+ [設定環境變數](./develop/environment-variables.md)

### 設定manifest.yml

asset compute專案包含的資訊清單會定義專案內包含的所有Asset compute背景工作，以及部署至Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發背景工作

開發Asset compute背景工作是擴充Asset compute微服務的核心，因為背景工作包含產生或協調產生結果資產轉譯的自訂程式碼。

+ [開發Asset compute背景工作](./develop/worker.md)

### 使用Asset compute開發工具

「Asset compute開發工具」提供本機Web配線，用於部署、執行和預覽工作者產生的轉譯，支援快速和反複的Asset compute工作者開發。

+ [使用Asset compute開發工具](./develop/development-tool.md)

## 測試和除錯

瞭解如何測試自訂Asset compute背景工作以對他們的作業充滿信心，並偵錯Asset compute背景工作以瞭解自訂計畫碼的執行方式並對其進行疑難排解。

### 測試背景工作

asset compute提供測試架構，可讓員工建立測試套件，進而定義可輕鬆確保正確行為的測試。

+ [測試背景工作](./test-debug/test.md)

### 對背景工作進行偵錯

asset compute背景工作提供各種層級的除錯功能，從傳統的除錯方式脫穎而出 `console.log(..)` 輸出，與整合 __VS程式碼__ 和  __wskdebug__，可讓開發人員在程式碼即時執行時逐步執行程式碼。

+ [對背景工作進行偵錯](./test-debug/debug.md)

## 部署

瞭解如何透過先將自訂Asset compute背景工作部署到AEMas a Cloud Service，然後透過Adobe I/O Runtime的處理設定檔從AEMas a Cloud Service作者叫用，將自訂部署背景工作與AEM Assets整合。

### 部署至Adobe I/O Runtime

asset compute背景工作必須部署至Adobe I/O Runtime才能與AEMas a Cloud Service搭配使用。

+ [使用處理設定檔](./deploy/runtime.md)

### 透過AEM處理設定檔整合背景工作

部署到Adobe I/O Runtime後，Asset compute背景工作可以透過在AEMas a Cloud Service註冊 [資產處理設定檔](../../assets/configuring/processing-profiles.md). 處理設定檔接著會套用至套用至其中資產的資產資料夾。

+ [與AEM處理設定檔整合](./deploy/processing-profiles.md)

## 進階

這些簡短的教學課程會根據前幾章中建立的基礎學習來處理更進階的使用案例。

+ [開發Asset compute中繼資料背景工作](./advanced/metadata.md) 可以將中繼資料寫回

## Github上的程式碼基底

此教學課程的程式碼基底可在Github上取得，網址為：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始程式碼未包含必要的 `.env` 或 `config.json` 檔案。 您必須使用新增和設定這些專案 [帳戶和服務](#accounts-and-services) 資訊。

## 其他資源

下列各種Adobe資源可提供進一步資訊，以及用於開發Asset compute工作者的實用API和SDK。

### 文件

+ [asset compute服務檔案](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開發工具Readme](https://github.com/adobe/asset-compute-devtool)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [ASSET COMPUTESDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute公域](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe雲端Blobstore包裝函式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點擷取重試程式庫](https://github.com/adobe/node-fetch-retry)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)
