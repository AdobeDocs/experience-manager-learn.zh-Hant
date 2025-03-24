---
title: AEM as a Cloud Service的Asset Compute微服務擴充性
description: 本教學課程將逐步解說如何建立簡易的Asset Compute背景工作，此背景工作將原始資產裁切至圓圈以建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身至關重要，但本教學課程會使用它來探索建立、開發和部署自訂Asset Compute背景工作，以與AEM as a Cloud Service搭配使用。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Asset Compute微服務的擴充性

AEM as Cloud Service的Asset Compute微服務支援開發及部署自訂背景工作，用於讀取和操控儲存在AEM中之資產的二進位資料，繼而多半用來建立自訂資產轉譯。

在AEM 6.x中，自訂AEM工作流程用於讀取、轉換和回寫資產轉譯，而在AEM as a Cloud Service Asset Compute中，工作流程可滿足此需求。

## 您將要執行的動作

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步解說如何建立簡易的Asset Compute背景工作，此背景工作將原始資產裁切至圓圈以建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身至關重要，但本教學課程會使用它來探索建立、開發和部署自訂Asset Compute背景工作，以與AEM as a Cloud Service搭配使用。

### 目標 {#objective}

1. 布建及設定必要的帳戶和服務以建置和部署Asset Compute背景工作
1. 建立及設定Asset Compute專案
1. 開發可產生自訂轉譯的Asset Compute背景工作
1. 編寫測試並瞭解如何對自訂Asset Compute背景工作進行偵錯
1. 部署Asset Compute Worker，並透過處理設定檔整合AEM as a Cloud Service Author服務

## 設定

瞭解如何正確準備延伸Asset Compute背景工作，並瞭解必須布建和設定哪些服務和帳戶，以及本機安裝哪些軟體以供開發。

### 帳戶和服務布建{#accounts-and-services}

若要完成教學課程、AEM as a Cloud Service開發環境或沙箱計畫、存取App Builder和Microsoft Azure Blob儲存空間，下列帳戶和服務需要布建和存取權。

+ [布建帳戶和服務](./set-up/accounts-and-services.md)

### 本機開發環境

Asset Compute專案的本機開發需要特定的開發人員工具集，不同於傳統的AEM開發，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支援的npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### App Builder

Asset Compute專案是特別定義的App Builder專案，因此需要在Adobe Developer Console中存取App Builder才能設定和部署它們。

+ [設定App Builder](./set-up/app-builder.md)

## 開發

瞭解如何建立和設定Asset Compute專案，然後開發自訂背景工作，以產生客製化資產轉譯。

### 建立新的Asset Compute專案

Asset Compute專案包含一或多個Asset Compute背景工作，並使用互動式Adobe I/O CLI產生。 Asset Compute專案是專門結構化的App Builder專案，這些專案又稱為Node.js專案。

+ [建立新的Asset Compute專案](./develop/project.md)

### 設定環境變數

環境變數會保留在`.env`檔案中以供本機開發使用，並用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。

+ [設定環境變數](./develop/environment-variables.md)

### 設定manifest.yml

Asset Compute專案包含的資訊清單會定義專案內包含的所有Asset Compute背景工作，以及部署至Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發背景工作

開發Asset Compute背景工作是擴充Asset Compute微服務的核心，因為背景工作包含產生或協調產生結果資產轉譯的自訂程式碼。

+ [開發Asset Compute背景工作](./develop/worker.md)

### 使用Asset Compute開發工具

Asset Compute開發工具提供本機Web配線，用於部署、執行和預覽工作者產生的轉譯，支援快速的反複式Asset Compute工作者開發。

+ [使用Asset Compute開發工具](./develop/development-tool.md)

## 測試和除錯

瞭解如何測試自訂Asset Compute背景工作以對其作業充滿信心，以及偵錯Asset Compute背景工作以瞭解自訂程式碼的執行方式並疑難排解。

### 測試背景工作

Asset Compute提供測試架構，可讓員工建立測試套件，進而定義可輕鬆確保正常行為的測試。

+ [測試背景工作](./test-debug/test.md)

### 對背景工作進行偵錯

Asset Compute背景工作提供從傳統`console.log(..)`輸出，到與&#x200B;__VS Code__&#x200B;和&#x200B;__wskdebug__&#x200B;整合的各種偵錯層級，讓開發人員在背景工作程式碼即時執行時能夠逐步執行程式碼。

+ [對背景工作進行偵錯](./test-debug/debug.md)

## 部署

瞭解如何將自訂Asset Compute背景工作與AEM as a Cloud Service整合，方法是先將自訂背景工作部署至Adobe I/O Runtime，然後透過AEM as a Cloud Service的處理設定檔從AEM Assets作者進行叫用。

### 部署至Adobe I/O Runtime

Asset Compute背景工作必須部署至Adobe I/O Runtime，才能與AEM as a Cloud Service搭配使用。

+ [使用處理設定檔](./deploy/runtime.md)

### 透過AEM處理設定檔整合背景工作

部署至Adobe I/O Runtime後，Asset Compute工作者可以透過[AEM as a Cloud Service處理設定檔](../../assets/configuring/processing-profiles.md)在Assets中註冊。 處理設定檔接著會套用至套用至其中資產的資產資料夾。

+ [與AEM處理設定檔整合](./deploy/processing-profiles.md)

## 進階

這些簡短的教學課程會根據前幾章中建立的基礎學習來處理更進階的使用案例。

+ [開發可將中繼資料寫回的Asset Compute中繼資料背景工作](./advanced/metadata.md)

## Github上的程式碼基底

此教學課程的程式碼基底可在Github上取得，網址為：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始程式碼未包含必要的`.env`或`config.json`檔案。 必須使用您的[帳戶和服務](#accounts-and-services)資訊來新增及設定這些專案。

## 其他資源

下列各種Adobe資源可提供進一步資訊，以及用於開發Asset Compute背景工作者的實用API和SDK。

### 文件

+ [Asset Compute服務檔案](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Asset Compute開發工具Readme](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore包裝函式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點擷取重試資料庫](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)
