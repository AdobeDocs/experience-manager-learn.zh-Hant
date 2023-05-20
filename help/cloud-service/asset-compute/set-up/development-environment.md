---
title: 設定本地開發環境以擴展Asset compute
description: 開發Asset compute工作程式（即Node.js JavaScript應用程式）需要與傳統開發不同的特定開發工具AEM，這些工具從Node.js和各種npm模組到Docker Desktop和MicrosoftVisual Studio代碼。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---

# 設定本地開發環境

AdobeAsset compute項目無法與AEMAEMSDK提供的本地運行時整合，並且是使用其自己的工具鏈開發的，與基於Maven項目原型的應用AEM程式所需的工具AEM鏈分開。

要擴展Asset compute微服務，必須在本地開發人員電腦上安裝以下工具。

## 刪節設定說明

下面是一條設定說明。 以下各節將詳細介紹這些開發工具。

1. [安裝Docker Desktop](https://www.docker.com/products/docker-desktop) 並拉出所需的Docker影像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安裝Visual Studio代碼](https://code.visualstudio.com/download)
1. [安裝Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 從命令行安裝所需的npm模組和Adobe I/OCLI插件：

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有關簡述安裝說明的詳細資訊，請閱讀以下各節。

## 安裝Visual Studio代碼{#vscode}

[MicrosoftVisual Studio代碼](https://code.visualstudio.com/download) 用於開發和調試Asset compute工作程式。 其他 [與JavaScript相容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) 可用於開發工作人員，只能將Visual Studio代碼整合到 [調試](../test-debug/debug.md) asset compute工。

本教程假定使用Visual Studio代碼，因為它為擴展Asset compute提供了最佳開發人員體驗。

## 安裝Docker Desktop{#docker}

下載並安裝最新的穩定 [Docker案頭](https://www.docker.com/products/docker-desktop)，因為 [test](../test-debug/test.md) 和 [調試](../test-debug/debug.md) asset compute本地項目。

安裝Docker Desktop後，啟動它並從命令行安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦上的開發人員應確保他們正在使用Linux容器進行上述映像。 有關切換到Linux容器的步驟，請參見 [Docker for Windows文檔](https://docs.docker.com/docker-for-windows/)。

## 安裝Node.js（和npm）{#node-js}

asset compute工人 [節點.js](https://nodejs.org/)基於，因此需要Node.js 10+（和npm）來開發和構建。

+ [安裝Node.js（和npm）](../../local-development-environment/development-tools.md#node-js) 與傳統發展一樣AEM.

## 安裝Adobe I/OCLI{#aio}

[安裝Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)或 __一體__ 是命令行(CLI)npm模組，便於使用和與Adobe I/O技術交互，用於生成和本地開發自定義Asset compute工作程式。

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Adobe I/OCLI 7.1.0版是必需的。 此時不支援Adobe I/OCLI的更高版本。_


## 安裝Adobe I/OCLIAsset compute插件{#aio-asset-compute}

的 [Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝 [Apache OpenWhisk調試](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，用於方便本地調試Asset compute工作程式。

_需要Visual Studio代碼1.48.x+ [wskdebug](#wskdebug) 工作。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝ngrok{#ngrok}

下載並安裝 [恩格羅](https://www.npmjs.com/package/ngrok) npm模組，它提供對本地開發機器的公共訪問，以便於本地調試Asset compute工作程式。

```
$ npm install -g ngrok --unsafe-perm=true
```
