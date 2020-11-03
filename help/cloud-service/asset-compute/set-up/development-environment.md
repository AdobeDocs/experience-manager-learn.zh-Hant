---
title: 為資產計算擴充性設定本機開發環境
description: 開發資產計算工具（即Node.js JavaScript應用程式）需要與傳統AEM開發不同的特定開發工具，從Node.js和各種npm模組到Docker Desktop和Microsoft Visual Studio程式碼。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# 設定本機開發環境

Adobe Asset Compute專案無法與AEM SDK提供的本機AEM執行階段整合，而且是使用其專屬的工具鏈來開發，這與AEM應用程式根據AEM Maven專案原型所需的工具鏈不同。

若要擴充Asset Compute microservices，必須在本機開發人員機器上安裝下列工具。

## 簡略的設定指示

以下是設定說明的基礎。 以下各節將詳述這些開發工具的詳細資訊。

1. [安裝Docker Desktop](https://www.docker.com/products/docker-desktop) ，並提取所需的Docker映像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [安裝Visual Studio代碼](https://code.visualstudio.com/download)
1. [安裝Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 從命令行安裝所需的npm模組和Adobe I/O CLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有關簡略安裝說明的詳細資訊，請閱讀以下各節。

## 安裝Visual Studio代碼{#vscode}

[Microsoft Visual Studio代碼](https://code.visualstudio.com/download) ，用於開發和調試資產計算工作器。 雖然可 [以使用其他與](../../local-development-environment/development-tools.md#set-up-the-development-ide) JavaScript相容的IDE [，來開發工作器，但只有Visual Studio代碼可與Asset](../test-debug/debug.md) Compute工作器進行整合。

_需要有Visual Studio Code 1.48.x+才能 [wskdebug](#wskdebug) 運作。_

本教學課程假設使用Visual Studio代碼，因為它為擴展資產計算提供了最佳的開發人員體驗。

## 安裝Docker Desktop{#docker}

下載並安裝最新、穩定的 [Docker Desktop](https://www.docker.com/products/docker-desktop)，因為在本機測試和調試Asset [Compute項目時需要這](../test-debug/test.md) 樣做 [](../test-debug/debug.md) 。

安裝Docker Desktop後，啟動它並從命令行安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦的開發人員應確定他們使用Linux容器來處理上述影像。 Docker for Windows文檔中介紹了切換到Linux [容器的步驟](https://docs.docker.com/docker-for-windows/)。

## 安裝Node.js（和npm）{#node-js}

資產計算工 [作者是基於Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）來開發和構建。

+ [以與傳統AEM開發相同的方式安裝Node.js（和npm）](../../local-development-environment/development-tools.md#node-js) 。

## 安裝Adobe I/O CLI{#aio}

[安裝Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli)，或 ____ aio是命令列(CLI)npm模組，可協助使用Adobe I/O技術並與之互動，並可用來產生和本機開發自訂的資產計算工作者。

```
$ npm install -g @adobe/aio-cli
```

## 安裝Adobe I/O CLI Asset Compute插件{#aio-asset-compute}

[Adobe I/O CLI Asset Compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝 [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，以方便資產計算工作者的本機除錯。

_需要有Visual Studio Code 1.48.x+才能 [wskdebug](#wskdebug) 運作。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝ngrok{#ngrok}

下載並安裝 [ngrok](https://www.npmjs.com/package/ngrok) npm模組，此模組可公開存取您的本機開發機器，以方便資產計算工作者的本機除錯。

```
$ npm install -g ngrok --unsafe-perm=true
```
