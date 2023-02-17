---
title: 如何建立快速發展環境
description: 了解如何為AEM as a Cloud Service設定快速開發環境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 65d54f0137786c7e8ac9ac962c424dd20bf5f3dd
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 2%

---


# 如何建立快速發展環境

學習 **如何設定** AEMas a Cloud Service的快速開發環境(RDE)。

此影片顯示：

- 使用Cloud Manager將RDE新增至您的程式
- 使用Adobe IMS的RDE登入流程，如何與任何其他AEMas a Cloud Service環境相似
- 設定 [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`
- AEM RDE和Cloud Manager的設定與設定 `aio CLI` 外掛程式

>[!VIDEO](https://video.tv.adobe.com/v/3415490/?quality=12&learn=on)

## 必備條件

應在本機安裝下列項目：

- [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
- [npm 8+](https://docs.npmjs.com/)

## 本機設定

部署 [WKND Sites專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 從本機電腦將程式碼和內容載入RDE，請完成下列步驟。

### Adobe I/O Runtime Extensible CLI

安裝Adobe I/O Runtime Extensible CLI(也稱為 `aio CLI` 從命令列執行下列命令。

```shell
$ npm install -g @adobe/aio-cli
```

### AEM外掛程式

請使用 `aio cli`&#39;s `plugins:install` 命令。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager外掛程式可讓開發人員從命令列與Cloud Manager互動。

AEM RDE外掛程式可讓開發人員從本機電腦部署程式碼和內容。

此外，若要更新外掛程式，請使用 `aio plugins:update` 命令。

## 設定AEM外掛程式

必須設定AEM外掛程式以與RDE互動。 首先，使用Cloud Manager UI複製組織、方案和環境ID的值。

1. 組織ID:複製值自 **個人資料圖片>帳戶資訊（內部）>強制回應視窗>目前組織ID**

   ![組織 ID](./assets/Org-ID.png)

1. 程式ID:複製值自 **程式概述>環境> {ProgramName}-rde >瀏覽器URI >之間的數字 `program/` 和`/environment`**

1. 環境ID:複製值自 **程式概述>環境> {ProgramName}-rde >瀏覽器URI >後面的數字`environment/`**

   ![方案與環境ID](./assets/Program-Environment-Id.png)

1. 然後，透過使用 `aio cli`&#39;s `config:set` 命令通過運行以下命令來設定這些值。

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

您可以執行下列命令以驗證目前的設定值。

```shell
$ aio config:list
```

此外，若要切換或了解您目前登入的組織，可使用以下命令。

```shell
$ aio where
```

## 驗證RDE訪問

請執行下列命令，以確認AEM RDE外掛程式的安裝和設定。

```shell
$ aio aem:rde:status
```

RDE狀態資訊的顯示方式與環境狀態、 _您的AEM專案_ 製作和發佈服務上的套件和設定。

## 下一步

學習 [如何使用](./how-to-use.md) 從您喜愛的整合開發環境(IDE)部署代碼和內容，以縮短開發週期。


## 其他資源

[在計畫文檔中啟用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

設定 [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`

[AIO CLI使用與命令](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI外掛程式，用於與AEM Rapid Development Environments互動](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI增效模組](https://github.com/adobe/aio-cli-plugin-cloudmanager)
