---
title: asset computeas a Cloud Service的微服務擴展AEM性
description: 本教程介紹如何建立一個簡單的Asset compute工作人員，該工作人員通過將原始資產裁剪到圓來建立資產格式副本，並應用可配置的對比度和亮度。 雖然工作人員本身是基本的，但本教程使用它來探索建立、開發和部署自定義Asset compute工作人員，以便與as a Cloud ServiceAEM一起使用。
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
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# asset compute微服務可擴充性

因AEM為Cloud Service的Asset computemicroservices支援開發和部署自定義工作程式，這些工作程式用於讀取和操作儲存在（最常見）中的資產的二進位數AEM據，以建立自定義資產格式副本。

而在AEM6.x中，自定AEM義工作流進程用於讀取、轉換和回寫資產格式副本，而在as a Cloud Service的AEMAsset compute工作人員則滿足此需求。

## 你將做什麼

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程介紹如何建立一個簡單的Asset compute工作人員，該工作人員通過將原始資產裁剪到圓來建立資產格式副本，並應用可配置的對比度和亮度。 雖然工作人員本身是基本的，但本教程使用它來探索建立、開發和部署自定義Asset compute工作人員，以便與as a Cloud ServiceAEM一起使用。

### 目標 {#objective}

1. 設定和設定必要的帳戶和服務以構建和部署Asset compute工作人員
1. 建立和配置Asset compute項目
1. 開發生成自定義Asset compute的工作人員
1. 編寫test，並瞭解如何調試自定義Asset compute工作人員
1. 部署Asset compute工作人員，並通過處理配置文AEM件整合其as a Cloud Service作者服務

## 設定

瞭解如何正確準備擴展Asset compute工作人員，瞭解哪些服務和帳戶必須配置和配置，以及本地安裝的軟體進行開發。

### 帳戶和服務預配{#accounts-and-services}

以下帳戶和服務需要設定和訪問，以完成教程、AEMas a Cloud Service開發環境或沙盒程式、對App Builder和MicrosoftAzure Blob儲存的訪問。

+ [提供帳戶和服務](./set-up/accounts-and-services.md)

### 地方發展環境

地方開發Asset compute項目需要一個特定的開發工具集，不同於傳統開發AEM，包括：MicrosoftVisual Studio代碼、Docker Desktop、Node.js和支援npm模組。

+ [設定本地開發環境](./set-up/development-environment.md)

### 應用程式生成器

asset compute項目是特別定義的App Builder項目，因此需要訪問Adobe開發者控制台中的App Builder才能設定和部署這些項目。

+ [設定應用生成器](./set-up/app-builder.md)

## 開發

瞭解如何建立和配置Asset compute項目，然後開發生成定制資產格式副本的定製工作人員。

### 建立新Asset compute項目

asset compute項目包含一個或多個Asset compute工作程式，使用互動式Adobe I/OCLI生成。 asset compute項目是特殊結構化的App Builder項目，而Node.js項目是。

+ [建立新Asset compute項目](./develop/project.md)

### 配置環境變數

在中維護環境變數 `.env` 檔案用於本地開發，用於提供本地開發所需的Adobe I/O憑據和雲儲存憑據。

+ [配置環境變數](./develop/environment-variables.md)

### 配置manifest.yml

asset compute項目包含清單，這些清單定義了項目中所有Asset compute工作人員，以及在部署到Adobe I/O Runtime執行時可用的資源。

+ [配置manifest.yml](./develop/manifest.md)

### 開發員工

開發Asset compute工作程式是擴展Asset compute微服務的核心，因為該工作程式包含生成或協調生成的資產格式副本的自定義代碼。

+ [開發Asset compute工](./develop/worker.md)

### 使用Asset compute開發工具

asset compute開發工具提供本地Web線束，用於部署、執行和預覽工作人員生成的格式副本，支援快速、迭代的Asset compute工作人員開發。

+ [使用Asset compute開發工具](./develop/development-tool.md)

## Test和調試

瞭解如何test自定義Asset compute工作程式以確保對其操作有信心，並調試Asset compute工作程式以瞭解和排除如何執行自定義代碼的問題。

### Test工作人員

asset compute為員工建立test套件提供了一個test框架，使定義test能夠確保正確的行為變得容易。

+ [Test工作人員](./test-debug/test.md)

### 調試工作進程

asset compute工作人員提供不同級別的傳統調試 `console.log(..)` 輸出到整合 __VS代碼__ 和  __wskdebug__，允許開發人員在即時執行工作代碼時逐步完成。

+ [調試工作進程](./test-debug/debug.md)

## 部署

瞭解如何將自定義Asset compute工作AEM人員與as a Cloud Service整合，方法是首先將他們部署到Adobe I/O Runtime，然AEM後通過AEM Assets的「處理配置檔案」從as a Cloud Service作者調用。

### 部署到Adobe I/O Runtime

asset compute工人必須部署到Adobe I/O Runtime，才能與as a Cloud ServiceAEM配合使用。

+ [使用處理配置檔案](./deploy/runtime.md)

### 通過處理配置文AEM件整合員工

一旦部署到Adobe I/O Runtime,Asset compute工人可以通過以下方式AEM在as a Cloud Service註冊 [資產處理配置檔案](../../assets/configuring/processing-profiles.md)。 處理配置檔案則應用於應用於其中資產的資產資料夾。

+ [與處理配AEM置檔案整合](./deploy/processing-profiles.md)

## 進階

這些簡述教程基於前幾章中建立的基礎知識，處理更高級的使用案例。

+ [開發Asset compute元資料工作程式](./advanced/metadata.md) 能將元資料寫回

## 吉圖布上的哥德巴斯

本教程的代碼庫可在Github上找到，網址為：

+ [adobe/aem參考線 — wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始碼不包含必需的 `.env` 或 `config.json` 的子菜單。 必須使用 [帳戶和服務](#accounts-and-services) 的下界。

## 其他資源

以下是各種Adobe資源，它們提供了進一步的資訊和有用的API和SDK，以用於開發Asset compute工作程式。

### 文件

+ [asset compute服務文檔](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開發工具自述檔案](https://github.com/adobe/asset-compute-devtool)
+ [asset compute示例工作人員](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute公地](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe雲Blobstore包裝庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點獲取重試庫](https://github.com/adobe/node-fetch-retry)
+ [asset compute示例工作人員](https://github.com/adobe/asset-compute-example-workers)
