---
title: 為Asset compute擴充性設定本機開發環境
description: 開發Asset compute背景工作(屬於Node.js JavaScript應用程式)需要與傳統AEM開發不同的特定開發工具，包括Node.js和各種npm模組，以及Docker Desktop和Microsoft Visual Studio Code。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# 設定本機開發環境

AdobeAsset compute專案無法與AEM SDK提供的本機AEM執行階段整合，且是使用自己的工具鏈來開發，不同於以AEM Maven專案原型為基礎的AEM應用程式所需的工具鏈。

若要擴充Asset compute微服務，下列工具必須安裝在本機開發人員電腦上。

## 簡略的設定指示

以下是節選設定指示。 這些開發工具的詳細資訊將於下文個別章節中說明。

1. [安裝Docker Desktop](https://www.docker.com/products/docker-desktop)並提取所需的Docker映像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安裝Visual Studio Code](https://code.visualstudio.com/download)
1. [安裝Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 從命令列安裝必要的npm模組和Adobe I/OCLI外掛程式：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

如需有關簡略安裝指示的詳細資訊，請閱讀以下章節。

## 安裝Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download)用於開發及偵錯Asset compute背景工作。 雖然可以使用其他[與JavaScript相容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)來開發背景工作，但只有Visual Studio程式碼可以整合到[偵錯](../test-debug/debug.md)Asset compute背景工作。

本教學課程假設您使用Visual Studio Code，因為它為擴充Asset compute提供了最佳的開發人員體驗。

## 安裝Docker Desktop{#docker}

下載並安裝最新的穩定[Docker Desktop](https://www.docker.com/products/docker-desktop)，因為這是本機[測試](../test-debug/test.md)和[偵錯](../test-debug/debug.md)Asset compute專案所需。

安裝Docker Desktop後，啟動它並從命令列安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦上的開發人員應該確定他們使用Linux容器來處理上述影像。 在[Docker for Windows檔案](https://docs.docker.com/docker-for-windows/)中說明了切換至Linux容器的步驟。

## 安裝Node.js （和npm）{#node-js}

asset compute背景工作程式是以[Node.js](https://nodejs.org/)為基礎，因此需要Node.js 10+ （和npm）才能開發和建置。

+ [以與傳統AEM開發相同的方式安裝Node.js （和npm）](../../local-development-environment/development-tools.md#node-js)。

## 安裝Adobe I/OCLI{#aio}

[安裝Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或&#x200B;__aio__&#x200B;是命令列(CLI) npm模組，可方便使用及與Adobe I/O技術互動，並用於產生和本機開發自訂Asset compute背景工作。

```
$ npm install -g @adobe/aio-cli
```

## 安裝Adobe I/OCLIAsset compute外掛程式{#aio-asset-compute}

[Adobe I/OCLIAsset compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，以便利本機偵錯Asset compute背景工作。

_[wskdebug](#wskdebug)需要Visual Studio Code 1.48.x+才能運作。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝Nrok{#ngrok}

下載並安裝[ngrok](https://www.npmjs.com/package/ngrok) npm模組，提供您本機開發電腦的公開存取權，以利本機偵錯Asset compute背景工作。

```
$ npm install -g ngrok --unsafe-perm=true
```
