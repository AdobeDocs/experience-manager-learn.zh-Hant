---
title: 為Asset compute擴充性設定本機開發環境
description: 開發Asset compute背景工作（屬於Node.js JavaScript應用程式）需要與傳統AEM開發不同的特定開發工具，包括Node.js和各種npm模組，以及Docker Desktop和Microsoft Visual Studio Code。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 140
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# 設定本機開發環境

AdobeAsset compute專案無法與AEM SDK提供的本機AEM執行階段整合，且是使用自己的工具鏈來開發，不同於以AEM Maven專案原型為基礎的AEM應用程式所需的工具鏈。

若要擴充Asset compute微服務，下列工具必須安裝在本機開發人員電腦上。

## 簡略的設定指示

以下是節選設定指示。 這些開發工具的詳細資訊將於下文個別章節中說明。

1. [安裝Docker Desktop](https://www.docker.com/products/docker-desktop) 並提取所需的Docker影像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安裝Visual Studio Code](https://code.visualstudio.com/download)
1. [安裝Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 從命令列安裝必要的npm模組和Adobe I/OCLI外掛程式：

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

如需有關簡略安裝指示的詳細資訊，請閱讀以下章節。

## 安裝Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) 用於開發和偵錯Asset compute背景工作。 其他 [與JavaScript相容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) 可用於開發背景工作，只有Visual Studio Code可以整合至 [偵錯](../test-debug/debug.md) asset compute背景工作。

本教學課程假設您使用Visual Studio Code，因為它為擴充Asset compute提供了最佳的開發人員體驗。

## 安裝Docker Desktop{#docker}

下載並安裝最新的穩定版 [Docker案頭](https://www.docker.com/products/docker-desktop)，因為此專案須為 [測試](../test-debug/test.md) 和 [偵錯](../test-debug/debug.md) 在本機Asset compute專案。

安裝Docker Desktop後，啟動它並從命令列安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦上的開發人員應該確定他們使用Linux容器來處理上述影像。 有關切換至Linux容器的步驟說明，請參閱 [適用於Windows的Docker檔案](https://docs.docker.com/docker-for-windows/).

## 安裝Node.js （和npm）{#node-js}

asset compute背景工作是 [Node.js](https://nodejs.org/)-based，因此需要Node.js 10+ （和npm）才能開發和建置。

+ [安裝Node.js （和npm）](../../local-development-environment/development-tools.md#node-js) 和傳統AEM開發的方式相同。

## 安裝Adobe I/OCLI{#aio}

[安裝Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或 __aio__ 是一個命令列(CLI) npm模組，可促進與Adobe I/O技術的使用和互動，並用於產生和本機開發自訂Asset compute背景工作。

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_需要Adobe I/OCLI 7.1.0版。 目前不支援更新版本的Adobe I/OCLI。_


## 安裝Adobe I/OCLIAsset compute外掛程式{#aio-asset-compute}

此 [Adobe I/OCLIAsset compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝 [Apache OpenWhisk偵錯](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，可方便Asset compute背景工作者的本機除錯。

_需要Visual Studio Code 1.48.x+才能使用 [wskdebug](#wskdebug) 才能運作。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝Nrok{#ngrok}

下載並安裝 [Ngrok](https://www.npmjs.com/package/ngrok) npm模組，可提供您本機開發電腦的公開存取權，以利對Asset compute工作者進行本機偵錯。

```
$ npm install -g ngrok --unsafe-perm=true
```
