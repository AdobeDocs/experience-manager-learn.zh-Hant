---
title: AEM as a Cloud Service 的 Asset Compute 微服務擴展性
description: 本教學課程會逐步解說如何建立一個簡單的 Asset Compute 工作程式，而該工作程式會將原始資產裁切為圓形來建立資產轉譯，並套用可設定的對比和亮度。雖然工作程式本身為基本內容，但本教學課程會使用工作程式來探索如何建立、開發和部署自訂 Asset Compute 工作程式並搭配 AEM as a Cloud Service 使用。
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
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Asset Compute 微服務擴展性

AEM as Cloud Service 的 Asset Compute 微服務支援開發及部署自訂工作程式，而工作程式的用途是讀取和操控儲存在 AEM 中之資產的二進位資料，多半用來建立自訂資產轉譯。

在 AEM 6.x 中，自訂 AEM 工作流程是用來讀取、轉換和寫回資產轉譯，而在 AEM as a Cloud Service 內，Asset Compute 工作程式能滿足這項需求。

## 操作內容

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程會逐步解說如何建立一個簡單的 Asset Compute 工作程式，而該工作程式會將原始資產裁切為圓形來建立資產轉譯，並套用可設定的對比和亮度。雖然工作程式本身為基本內容，但本教學課程會使用工作程式來探索如何建立、開發和部署自訂 Asset Compute 工作程式並搭配 AEM as a Cloud Service 使用。

### 目標 {#objective}

1. 佈建及建立必要的帳戶和服務，藉以建置和部署 Asset Compute 工作程式
1. 建立並設定 Asset Compute 專案
1. 開發一個能產生自訂轉譯的 Asset Compute 工作程式
1. 編寫測試並學習如何對自訂 Asset Compute 工作程式進行偵錯
1. 部署 Asset Compute 工作程式，並透過處理設定檔將其整合至 AEM as a Cloud Service Author 服務

## 建立

了解如何正確地準備擴展 Asset Compute 工作程式，並認識必須佈建和設定哪些服務和帳戶，以及應在本機安裝哪些軟體以進行開發。

### 帳戶和服務佈建{#accounts-and-services}

若要完成本教學課程，需要佈建與存取以下帳戶和服務：AEM as a Cloud Service 開發環境或沙箱方案、App Builder 存取權和 Microsoft Azure Blob Storage。

+ [佈建帳戶和服務](./set-up/accounts-and-services.md)

### 本機開發環境

Asset Compute 專案的本機開發需要特定的開發人員工具集，其與傳統的 AEM 開發不同，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js 和支援的 npm 模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### App Builder

Asset Compute 專案是特別定義的 App Builder 專案，因此必須具備在 Adobe Developer Console 中存取 App Builder 的權限，才能進行設定和部署。

+ [設定 App Builder](./set-up/app-builder.md)

## 開發

了解如何建立和設定 Asset Compute 專案，然後開發能產生客製化資產轉譯的自訂工作程式。

### 建立新的 Asset Compute 專案

Asset Compute 專案包含一個或多個使用互動式 Adobe I/O CLI 產生的 Asset Compute 工作程式。Asset Compute 專案是特殊結構的 App Builder 專案，而 App Builder 專案又屬於 Node.js 專案。

+ [建立新的 Asset Compute 專案](./develop/project.md)

### 設定環境變數

環境變數會保留在 `.env` 檔案中供本機開發使用，並用於提供本機開發所需的 Adobe I/O 認證和雲端儲存空間認證。

+ [設定環境變數](./develop/environment-variables.md)

### 設定 manifest.yml

Asset Compute 專案包含定義專案內包含的所有 Asset Compute 工作程式的清單，以及在部署至 Adobe I/O Runtime 執行時可用的資源。

+ [設定 manifest.yml](./develop/manifest.md)

### 開發工作程式

開發 Asset Compute 工作程式是擴展 Asset Compute 微服務的核心，因為工作程式所包含的自訂程式碼，可以產生或協調產生最終資產轉譯。

+ [開發 Asset Compute 工作程式](./develop/worker.md)

### 使用 Asset Compute 開發工具

Asset Compute 開發工具提供一個本機網頁控制，用於部署、執行和預覽工作程式所產生的轉譯，支援 Asset Compute 工作程式快速的迭代開發。

+ [使用 Asset Compute 開發工具](./develop/development-tool.md)

## 測試與偵錯

了解如何測試自訂 Asset Compute 工作程式，確保其運作的可靠性，並針對 Asset Compute 工作程式進行偵錯，以了解自訂程式碼的執行狀況及進行疑難排解。

### 工作程式測試

Asset Compute 提供一個測試框架，用於建立工作程式的測試套件，以便輕鬆地定義確保正確行為的測試。

+ [工作程式測試](./test-debug/test.md)

### 工作程式偵錯

Asset Compute 工作程式提供各種層級的偵錯，從傳統的 `console.log(..)` 輸出，到與 __VS 程式碼__&#x200B;及 __wskdebug__ 整合，讓開發人員能夠在工作程式執行的同時，即時進行偵錯。

+ [工作程式偵錯](./test-debug/debug.md)

## 部署

了解如何將自訂 Asset Compute 工作程式與 AEM as a Cloud Service 整合，方法是先將其部署至 Adobe I/O Runtime，然後透過 AEM Assets 的處理設定檔，從 AEM as a Cloud Service Author 進行叫用。

### 部署至 Adobe I/O Runtime

Asset Compute 工作程式必須部署至 Adobe I/O Runtime，才可供 AEM as a Cloud Service 使用。

+ [使用處理設定檔](./deploy/runtime.md)

### 透過 AEM 處理設定檔整合工作程式

Asset Compute 工作程式部署至 Adobe I/O Runtime 後，可以透過[資產處理設定檔](../../assets/configuring/processing-profiles.md)在 AEM as a Cloud Service 中註冊。處理設定檔又會套用至套用其中資產的資產資料夾。

+ [與 AEM 處理設定檔整合](./deploy/processing-profiles.md)

## 進階

這些精要的教學課程在前幾章所建置的基本學習基礎上，處理了更進階的使用案例。

+ 開發可以將中繼資料寫回的 [Asset Compute 中繼資料工作程式](./advanced/metadata.md)

## Github 上的程式碼基底

本教學課程的程式碼基底可在 Github 上找到，位於：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 主分支

原始程式碼不包含必要的 `.env` 或者 `config.json` 檔案。這些必須使用您的[帳戶和服務](#accounts-and-services)資訊來新增和設定。

## 其他資源

以下是各種 Adobe 資源，能針對開發 Asset Compute 工作程式提供更多資訊及有用的 API 和 SDK。

### 文件

+ [Asset Compute 服務文件](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=zh-Hant)
+ [Asset Compute 開發工具讀我檔案](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute 工作程式範例](https://github.com/adobe/asset-compute-example-workers)

### API 和 SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper 資料庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch Retry 資料庫](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute 工作程式範例](https://github.com/adobe/asset-compute-example-workers)
