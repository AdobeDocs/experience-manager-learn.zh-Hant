---
title: AEM雲端服務的資產計算微服務擴充性
description: 本教學課程將逐步介紹如何建立簡單的「資產計算」工作者，透過將原始資產裁切至社交圈來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作者本身是基本的，但本教學課程會使用它來探索建立、開發和部署自訂資產計算工作者，以便與AEM搭配使用做為雲端服務。
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# 資產計算微服務擴充性

AEM作為Cloud Service的Asset Compute microservices支援自訂工作者的開發與部署，這些工作者用來讀取和控制儲存在AEM中的資產的二進位資料，最常用的是，用來建立自訂資產轉譯。

而在AEM 6.x中，自訂的AEM Workflow流程是用來讀取、轉換和回寫資產轉譯，而在AEM中，Cloud Service Asset Compute工作者可滿足此需求。

## 您將做的事

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步介紹如何建立簡單的「資產計算」工作者，透過將原始資產裁切至社交圈來建立資產轉譯，並套用可設定的對比度和亮度。 雖然工作者本身是基本的，但本教學課程會使用它來探索建立、開發和部署自訂資產計算工作者，以便與AEM搭配使用做為雲端服務。

### 目標 {#objective}

1. 配置和設定必要的帳戶和服務以構建和部署資產計算員工
1. 建立和配置資產計算項目
1. 開發可生成自定義格式副本的Asset Compute工作器
1. 編寫測試，並瞭解如何對自訂資產計算工作者進行除錯
1. 部署Asset Compute工作者，並透過「處理設定檔」將其整合為雲端服務作者服務

## 設定

瞭解如何正確準備擴展資產計算工作，並瞭解哪些服務和帳戶必須進行配置和配置，以及本地安裝的軟體以進行開發。

### 帳戶和服務布建{#accounts-and-services}

下列帳戶和服務需要布建和存取權，才能完成教學課程AEM（雲端服務開發環境或沙盒程式）、Adobe Project Firefly和Microsoft Azure Blob儲存空間的存取。

+ [提供帳戶和服務](./set-up/accounts-and-services.md)

### 當地開發環境

當地開發Asset Compute專案需要特定的開發人員工具集，這不同於傳統的AEM開發，包括：Microsoft Visual Studio代碼、Docker Desktop、Node.js和支援npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### Adobe Project Firefly

「資產計算」專案是特別定義的Adobe Project Firefly專案，因此，您必須在Adobe Developer Console中存取Adobe Project Firefly，才能設定和部署這些項目。

+ [設定Adobe Project Firefly](./set-up/firefly.md)

## 開發

瞭解如何建立和設定資產計算專案，然後開發自訂員工，以產生定制資產轉譯。

### 建立新的資產計算專案

資產計算項目（包含一個或多個資產計算工作者）使用互動式Adobe I/O CLI生成。 「資產計算」專案是特殊結構化的Adobe Project Firefly專案，而Node.js專案則是專案。

+ [建立新的資產計算專案](./develop/project.md)

### 配置環境變數

環境變數會保留在檔 `.env` 案中以進行本端開發，並用來提供本端開發所需的Adobe I/O憑證和雲端儲存憑證。

+ [配置環境變數](./develop/environment-variables.md)

### 設定manifest.yml

「資產計算」項目包含清單，可定義項目中包含的所有「資產計算」工作者，以及部署到Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發員工

開發資產計算工作器是擴展資產計算微服務的核心，因為該工作器包含生成或協調生成的資產轉譯的定製代碼。

+ [開發資產計算員工](./develop/worker.md)

### 使用資產計算開發工具

Asset Compute Development Tool提供本機Web控管，用於部署、執行和預覽工作者生成的轉譯，支援快速、反覆的Asset Compute Worker開發。

+ [使用資產計算開發工具](./develop/development-tool.md)

## 測試與除錯

瞭解如何測試自訂資產計算工作者以確保其運作，並對資產計算工作者進行除錯，以瞭解並疑難排解如何執行自訂代碼。

### 測試工作者

「資產計算」提供測試框架，用於為員工建立測試套裝，使定義測試能確保行為正確易行。

+ [測試工作者](./test-debug/test.md)

### 對工作者進行除錯

資產計算工作者提供從傳統輸出到與 `console.log(..)` VS Code __和__ wskdebug ____&#x200B;整合的各種除錯層級，讓開發人員在執行工作者程式碼時可逐步執行。

+ [對工作者進行除錯](./test-debug/debug.md)

## 部署

瞭解如何將自訂資產計算工作者與AEM整合為雲端服務，方法是先將他們部署至Adobe I/O Runtime，再透過AEM Assets的「處理設定檔」從AEM叫用為雲端服務作者。

### 部署至Adobe I/O Runtime

資產計算工作者必須部署至Adobe I/O Runtime，才能與AEM搭配使用，做為雲端服務。

+ [使用處理描述檔](./deploy/runtime.md)

### 透過AEM處理設定檔整合工作者

部署至Adobe I/O Runtime後，資產計算工作者即可透過資產處理設定檔在AEM中註冊為雲 [端服務](../../assets/configuring/processing-profiles.md)。 處理設定檔則套用至套用至資產資料夾的資產。

+ [與AEM處理設定檔整合](./deploy/processing-profiles.md)

## 進階

這些簡略的教學課程以前幾章中建立的基礎學習為基礎，處理更進階的使用案例。

+ [開發資產計算元資料工作器](./advanced/metadata.md) ，該工作器可將元資料寫回

## Github上的代碼庫

教學課程的程式碼基底可在Github取得，網址為：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

原始碼不包含必要 `.env` 或檔 `config.json` 案。 必須使用您的帳戶和服務資訊來 [新增和設定這些](#accounts-and-services) 。

## 其他資源

以下是各種Adobe資源，可提供進一步資訊和有用的API和SDK，以開發「資產計算」工作人員。

### 文件

+ [資產計算服務文檔](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [資產計算開發工具自述檔案](https://github.com/adobe/asset-compute-devtool)
+ [資產計算示例工作程式](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [資產計算SDK](https://github.com/adobe/asset-compute-sdk)
   + [資產計算共用](https://github.com/adobe/asset-compute-commons)
   + [資產計算XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore包裝函式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點擷取重試程式庫](https://github.com/adobe/node-fetch-retry)
+ [資產計算示例工作程式](https://github.com/adobe/asset-compute-example-workers)
