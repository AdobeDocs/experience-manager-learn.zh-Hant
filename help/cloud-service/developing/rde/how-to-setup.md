---
title: 如何設定快速開發環境
description: 瞭解如何設定AEMas a Cloud Service的快速開發環境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 512
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 1%

---

# 如何設定快速開發環境

瞭解 **如何設定** AEMas a Cloud Service中的快速開發環境(RDE)。

本影片說明：

- 使用Cloud Manager將RDE新增到您的計畫
- 使用Adobe IMS的RDE登入流程，與任何其他AEMas a Cloud Service環境有何相似之處
- 設定 [Adobe I/O Runtime可擴充CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`
- AEM RDE和Cloud Manager的設定和設定 `aio CLI` 外掛程式

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 必備條件

下列專案應在本機安裝：

- [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
- [npm 8+](https://docs.npmjs.com/)

## 本機設定

若要部署 [WKND網站專案的](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 將程式碼和內容從本機電腦移至RDE上，請完成下列步驟。

### Adobe I/O Runtime可擴充CLI

安裝Adobe I/O Runtime可擴充CLI (也稱為 `aio CLI` 從命令列執行下列命令。

```shell
$ npm install -g @adobe/aio-cli
```

### AEM外掛程式

使用安裝Cloud Manager和AEM RDE外掛程式 `aio cli`的 `plugins:install` 命令。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager外掛程式，可讓開發人員從命令列與Cloud Manager互動。

AEM RDE外掛程式可讓開發人員從本機電腦部署程式碼和內容。

此外，若要更新外掛程式，請使用 `aio plugins:update` 命令。

## 設定AEM外掛程式

AEM外掛程式必須設定為與您的RDE互動。 首先，使用Cloud Manager UI複製組織、計畫和環境ID的值。

1. 組織ID：複製值來源 **個人資料圖片>帳戶資訊（內部） >模型視窗>目前組織ID**

   ![組織ID](./assets/Org-ID.png)

1. 方案ID：複製值，從 **計畫總覽>環境> {ProgramName}-rde >瀏覽器URI >數字介於 `program/` 和`/environment`**

1. 環境ID：複製值 **計畫總覽>環境> {ProgramName}-rde >瀏覽器URI >之後的數字`environment/`**

   ![程式和環境ID](./assets/Program-Environment-Id.png)

1. 然後，透過使用 `aio cli`的 `config:set` 命令執行下列命令來設定這些值。

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

您可以透過執行以下命令來驗證目前的設定值。

```shell
$ aio config:list
```

此外，若要切換或知道您目前登入的組織，您可以使用以下指令。

```shell
$ aio where
```

## 驗證RDE存取權

執行下列命令，驗證AEM RDE外掛程式的安裝和組態。

```shell
$ aio aem:rde:status
```

RDE狀態資訊會像環境狀態一樣顯示， _您的AEM專案_ 製作和發佈服務的套件組合和設定。

## 下一步

瞭解 [使用方式](./how-to-use.md) 從您喜愛的整合式開發環境(IDE)部署程式碼和內容的RDE，以加快開發週期。


## 其他資源

[在程式檔案中啟用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

設定 [Adobe I/O Runtime可擴充CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也稱為 `aio CLI`

[AIO CLI使用方式和命令](https://github.com/adobe/aio-cli#usage)

[與AEM快速開發環境互動的Adobe I/O Runtime CLI外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager)
