---
title: 設定本機開發環境以擴充Asset compute
description: 開發Asset compute工作程式（即Node.js JavaScript應用程式）需要與傳統開發不同的特定開發工具，從Node.js和各種npm模組到Docker Desktop和Microsoft Visual Studio程式碼。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: 53c20b9774c15b04a1c78c7c0c7b61a60996bf60
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---


# 設定本機開發環境

AdobeAsset compute專案無法與AEMAEM SDK提供的本機執行階段整合，而且是使用其專屬的工具鏈來開發，與以Maven專案原型為基礎的應用程式所AEM需的工具AEM鏈不同。

若要擴充Asset compute微服務，必須在本機開發人員機器上安裝下列工具。

## 簡略的設定指示

以下是設定說明的基礎。 以下各節將詳述這些開發工具的詳細資訊。

1. [安裝Docker ](https://www.docker.com/products/docker-desktop) Desktop並提取所需的Docker映像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安裝Visual Studio代碼](https://code.visualstudio.com/download)
1. [安裝Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 從命令行安裝所需的npm模組和Adobe I/OCLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有關簡略安裝說明的詳細資訊，請閱讀以下各節。

## 安裝Visual Studio代碼{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) Code用於開發和調試Asset compute工作程式。雖然可使用其他與[JavaScript相容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)來開發工作器，但只有Visual Studio代碼可與[debug](../test-debug/debug.md)Asset compute工作器整合。

_Visual Studio Code 1.48.x+是進行 [](#wskdebug) wskdebutgo工作的必要項。_

本教學課程假設使用Visual Studio程式碼，因為它提供最佳的開發人員體驗以擴展Asset compute。

## 安裝Docker Desktop{#docker}

下載並安裝最新、穩定的[Docker Desktop](https://www.docker.com/products/docker-desktop)，因為在本機下載[test](../test-debug/test.md)和[debug](../test-debug/debug.md)Asset compute項目時需要此選項。

安裝Docker Desktop後，啟動它並從命令行安裝以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows電腦的開發人員應確定他們使用Linux容器來處理上述影像。 [Docker for Windows文檔](https://docs.docker.com/docker-for-windows/)中介紹了切換到Linux容器的步驟。

## 安裝Node.js（和npm）{#node-js}

asset compute工作者基於[Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）來開發和構建。

+ [以與傳統開發相同的方](../../local-development-environment/development-tools.md#node-js) 式安裝Node.js（和npm）AEM。

## 安裝Adobe I/OCLI{#aio}

[安裝Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或 ____ aiois命令行(CLI)npm模組，該模組便於使用Adobe I/O技術並與其交互，並用於生成自定義Asset compute工作程式和在本地開發自定義操作程式。

```
$ npm install -g @adobe/aio-cli
```

## 安裝Adobe I/OCLIAsset compute插件{#aio-asset-compute}

[Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安裝wskdebug{#wskdebug}

下載並安裝[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，以便對Asset compute工作者進行本地調試。

_Visual Studio Code 1.48.x+是進行 [](#wskdebug) wskdebutgo工作的必要項。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安裝ngrok{#ngrok}

下載並安裝[ngrok](https://www.npmjs.com/package/ngrok) npm模組，該模組提供對本地開發機器的公共訪問，以便於Asset compute工作程式的本地調試。

```
$ npm install -g ngrok --unsafe-perm=true
```
