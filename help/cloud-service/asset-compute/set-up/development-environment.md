---
title: 設定本機開發環境以擴充Asset compute
description: 開發Asset compute背景工作（屬於Node.js JavaScript應用程式）需要與傳統AEM開發不同的特定開發工具，包括Node.js和各種npm模組，以及Docker Desktop和Microsoft Visual Studio Code。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: fd72f3c85db8a56ec8abfd1609da53492ee54be2
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 0%

---


# 設定本機開發環境

AdobeAsset compute專案無法與AEM SDK提供的本機AEM執行階段整合，且是使用其專屬的工具鏈來開發，而與AEM應用程式根據AEM Maven專案原型所需的工具鏈不同。

要擴展Asset compute微服務，必須在本地開發人員電腦上安裝以下工具。

## 概要設定說明

以下是設定指示的橋梁。 以下各節將詳細說明這些開發工具。

1. [安裝Docker Desktop](https://www.docker.com/products/docker-desktop) 並提取所需的Docker映像：

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

有關概要安裝說明的詳細資訊，請閱讀以下各節。

## 安裝Visual Studio代碼{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) 用於開發和調試Asset compute工作。雖然可以使用其他與[JavaScript相容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)來開發工作器，但只有Visual Studio Code可以整合到[debug](../test-debug/debug.md)Asset compute工作器。

本教學課程假設使用Visual Studio Code，因為它為擴充Asset compute提供最佳開發人員體驗。

## 安裝Docker案頭{#docker}

下載並安裝最新、穩定的[Docker Desktop](https://www.docker.com/products/docker-desktop)，因為這是本地[test](../test-debug/test.md)和[debug](../test-debug/debug.md)Asset compute項目所需的。

安裝Docker Desktop後，啟動它，然後從命令行安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦上的開發人員應確定他們使用Linux容器來處理上述影像。 [Docker for Windows文檔](https://docs.docker.com/docker-for-windows/)中介紹了切換到Linux容器的步驟。

## 安裝Node.js（和npm）{#node-js}

asset compute背景工作是以[Node.js](https://nodejs.org/)為基礎，因此需要Node.js 10+（和npm）來開發和建置。

+ [以與傳統AEM開發相同的方式安裝Node.js（和npm）](../../local-development-environment/development-tools.md#node-js) 。

## 安裝Adobe I/OCLI{#aio}

[安裝Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或aios  ____ 命令行(CLI)npm模組，該模組便於使用和與Adobe I/O技術交互，並用於生成和本地開發自定義Asset compute工作。

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Adobe I/OCLI 7.1.0版是必需的。目前不支援更新版本的Adobe I/OCLI。_


## 安裝Adobe I/OCLIAsset compute插件{#aio-asset-compute}

[Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，以便於Asset compute背景工作的本地調試。

_Wskdebugto工作需要Visual Studio Code 1.48. [](#wskdebug) x+。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝ngrok{#ngrok}

下載並安裝[ngrok](https://www.npmjs.com/package/ngrok) npm模組，該模組提供對本地開發電腦的公用訪問，以便於Asset compute工作程式的本地調試。

```
$ npm install -g ngrok --unsafe-perm=true
```
