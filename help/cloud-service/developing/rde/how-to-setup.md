---
title: 如何營造快速發展環境
description: 瞭解如何為as a Cloud Service設定快速開發環境AEM。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 2%

---

# 如何營造快速發展環境

學習 **如何設定** 快速開發環境(RDE)在AEMas a Cloud Service。

該視頻顯示：

- 使用雲管理器向程式添加RDE
- 使用Adobe IMS的RDE登錄流，與其它任何as a Cloud Service環境AEM相似
- 設定 [Adobe I/O Runtime可擴展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`
- RDE和雲管理AEM器的設定和配置 `aio CLI` 插件

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 必備條件

應在本地安裝以下內容：

- [節點.js](https://nodejs.org/en/) （LTS — 長期支援）
- [npm 8+](https://docs.npmjs.com/)

## 本地設定

部署 [WKND站點項目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 將代碼和內容從本地電腦寫入RDE，請完成以下步驟。

### Adobe I/O Runtime可擴展CLI

安裝Adobe I/O Runtime可擴展CLI，也稱為 `aio CLI` 從命令行運行以下命令。

```shell
$ npm install -g @adobe/aio-cli
```

### AEM插件

使用以下命令安AEM裝Cloud Manager和RDE插件 `aio cli``s `plugins:install` 的子菜單。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager插件允許開發人員從命令行與Cloud Manager交互。

RDE插AEM件允許開發人員從本地電腦部署代碼和內容。

此外，要更新插件，請使用 `aio plugins:update` 的子菜單。

## 配置插AEM件

必須AEM將插件配置為與RDE交互。 首先，使用雲管理器UI複製組織、程式和環境ID的值。

1. 組織ID:複製值 **配置檔案圖片>帳戶資訊（內部）>模式窗口>當前組織標識**

   ![組織 ID](./assets/Org-ID.png)

1. 程式ID:複製值 **程式概述>環境> {ProgramName}-rde >瀏覽器URI >介於 `program/` 和`/environment`**

1. 環境ID:複製值 **程式概述>環境> {ProgramName}-rde >瀏覽器URI >後面的數字`environment/`**

   ![程式和環境ID](./assets/Program-Environment-Id.png)

1. 然後，使用 `aio cli``s `config:set` 命令通過運行以下命令來設定這些值。

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

通過運行以下命令，可以驗證當前配置值。

```shell
$ aio config:list
```

此外，要切換或瞭解您當前登錄的組織，可以使用以下命令。

```shell
$ aio where
```

## 驗證RDE訪問

運行以AEM下命令驗證RDE插件的安裝和配置。

```shell
$ aio aem:rde:status
```

RDE狀態資訊顯示為環境狀態， _您的項AEM目_ 作者和發佈服務上的捆綁和配置。

## 下一步

學習 [如何使用](./how-to-use.md) 部署RDE，以從您最喜愛的整合開發環境(IDE)部署代碼和內容，以加快開發週期。


## 其他資源

[在程式文檔中啟用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

設定 [Adobe I/O Runtime可擴展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`

[AIO CLI用法和命令](https://github.com/adobe/aio-cli#usage)

[Adobe I/O RuntimeCLI插件，用於與快速開發AEM環境交互](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)
