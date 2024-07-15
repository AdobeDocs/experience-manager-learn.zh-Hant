---
title: 如何設定快速開發環境
description: 瞭解如何設定AEM as a Cloud Service的快速開發環境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: f714adaa9bb637c0c7b17837c1d4b9f2be737c5c
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# 如何設定快速開發環境

瞭解如何在AEM as a Cloud Service中設定&#x200B;**快速開發環境(RDE)。**

本影片說明：

- 使用Cloud Manager將RDE新增至您的程式
- 使用Adobe IMS的RDE登入流程，與任何其他AEM as a Cloud Service環境有何相似之處
- [Adobe I/O Runtime可擴充CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)的設定，也稱為`aio CLI`
- 使用非互動模式來設定AEM RDE和Cloud Manager `aio CLI`外掛程式。 如需互動模式，請參閱[安裝指示](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 必備條件

下列專案應在本機安裝：

- [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
- [npm 8+](https://docs.npmjs.com/)

## 本機設定

若要從您的本機電腦將[WKND Sites專案的](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)程式碼和內容部署到RDE，請完成下列步驟。

### Adobe I/O Runtime可擴充CLI

從命令列執行下列命令，安裝Adobe I/O Runtime可擴充CLI （也稱為`aio CLI`）。

```shell
$ npm install -g @adobe/aio-cli
```

### 安裝及設定aio CLI外掛程式

aio CLI必須已安裝外掛程式，並使用組織、方案和RDE環境ID進行設定，才能與您的RDE互動。 安裝程式可透過aio CLI使用較簡單的互動模式或非互動模式來執行。

>[!BEGINTABS]

>[!TAB 互動模式]

使用`aio cli`的`plugins:install`命令安裝及設定AEM RDE外掛程式。

1. 使用`aio cli`的`plugins:install`命令安裝aio CLI的AEM RDE外掛程式。

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   AEM RDE外掛程式可讓開發人員從本機電腦部署程式碼和內容。

2. 執行下列命令以登入Adobe I/O Runtime可擴充CLI來取得存取Token。 請務必登入與Cloud Manager相同的Adobe組織。

   ```shell
   $ aio login
   ```

3. 執行以下命令，以使用互動模式設定RDE。

   ```shell
   $ aio aem:rde:setup
   ```

4. CLI會提示您輸入組織ID、方案ID和環境ID。

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - 如果您只使用單一RDE，而且想要將您的RDE組態全域儲存在本機電腦上，請選擇&#x200B;__否__。

   - 如果您正在使用多個RDE，或想要將您的RDE組態儲存在目前資料夾的`.aio`檔案中，請為每個專案選擇&#x200B;__是__。

5. 從可用選項清單中選取組織ID、方案ID和RDE環境ID。

6. 執行下列指令，確認已設定正確的組織、程式和環境。

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB 非互動模式]

使用`aio cli`的`plugins:install`命令安裝及設定Cloud Manager和AEM RDE外掛程式。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Cloud Manager外掛程式，可讓開發人員從命令列與Cloud Manager互動。

AEM RDE外掛程式可讓開發人員從本機電腦部署程式碼和內容。

必須設定aio CLI外掛程式才能與您的RDE互動。

1. 首先，使用Cloud Manager複製組織、方案和環境ID的值。

   - 組織ID：從&#x200B;**設定檔圖片>帳戶資訊（內部） >強制回應視窗>目前組織ID**&#x200B;複製值

   ![組織ID](./assets/Org-ID.png)

   - 程式ID：從&#x200B;**程式概述>環境> {ProgramName}-rde >瀏覽器URI >介於`program/`和`/environment`**&#x200B;之間的數字複製值

   ![程式和環境ID](./assets/Program-Environment-Id.png)

   - 環境ID：從&#x200B;**程式總覽>環境> {ProgramName}-rde >瀏覽器URI >`environment/`**&#x200B;之後的數字複製值

   ![程式和環境ID](./assets/Program-Environment-Id.png)

1. 使用`aio cli`的`config:set`命令執行下列命令以設定這些值。

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. 執行下列命令，驗證目前的設定值。

   ```shell
   $ aio config:list
   ```

1. 切換或檢閱您目前登入的組織：

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## 驗證RDE存取權

執行下列命令，驗證AEM RDE外掛程式的安裝和組態。

```shell
$ aio aem:rde:status
```

RDE狀態資訊的顯示方式如環境狀態、_您的AEM專案_&#x200B;套件組合清單以及製作和發佈服務上的設定。

## 下一步

瞭解[如何使用](./how-to-use.md) RDE來部署您最喜愛的整合式開發環境(IDE)中的程式碼和內容，以加快開發週期。


## 其他資源

[在程式檔案中啟用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

[Adobe I/O Runtime可擴充CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)的設定，也稱為`aio CLI`

[aio CLI使用方式和命令](https://github.com/adobe/aio-cli#usage)

用於與AEM快速開發環境互動的[Adobe I/O Runtime CLI外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager aio CLI外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager)
