---
title: 將AEM的Dispatcher Tools設定為雲端服務開發
description: AEM SDK的Dispatcher Tools可讓本機安裝、執行和疑難排解Dispatcher，以協助本機開發Adobe Experience Manager(AEM)專案。
sub-product: 基礎
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 1%

---


# 設定本地Dispatcher工具

Adobe Experience Manager(AEM)的Dispatcher是Apache HTTP Web伺服器模組，可在CDN和AEM Publish層之間提供安全性與效能層。 Dispatcher是整體Experience Manager架構的一部分，應是本端開發設定的一部分。

AEM a Cloud Service SDK包含建議的Dispatcher Tools版本，可協助在本機設定、驗證和模擬Dispatcher。 Dispatcher Tools由以下組成：

+ 位於`.../dispatcher-sdk-x.x.x/src`的Apache HTTP Web伺服器和Dispatcher配置檔案的基準集
+ 配置驗證器CLI工具，位於`.../dispatcher-sdk-x.x.x/bin/validate`(Dispatcher SDK 2.0.29+)
+ 配置生成CLI工具，位於`.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位於`.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 使用Dispatcher模組運行Apache HTTP Web伺服器的Docker映像

請注意，`~`是用作用戶目錄的速記。 在Windows中，此值相當於`%HOMEPATH%`。

>[!NOTE]
>
> 本頁中的影片是在macOS上錄制的。 Windows用戶可以跟進，但使用與每個視頻一起提供的等效Dispatcher Tools Windows命令。

## 必備條件

1. Windows使用者必須使用Windows 10 Professional
1. 在本機開發機器上安裝[Experience Manager發佈快速入門Jar](./aem-runtime.md)。
   + （可選）在本機AEM Publish服務上安裝最新[AEM參考網站](https://github.com/adobe/aem-guides-wknd/releases)。 本教程使用此網站來直觀顯示工作的Dispatcher。
1. 在本機開發機器上安裝並啟動最新版[Docker](https://www.docker.com/)(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下載Dispatcher Tools（做為AEM SDK的一部分）

AEM(Cloud Service SDK)或AEM SDK包含Dispatcher Tools，可用來在本機執行Apache HTTP Web伺服器並搭配Dispatcher模組進行開發，以及相容的QuickStart Jar。

如果AEM已下載為Cloud Service SDK，並已下載至[設定本機AEM執行階段](./aem-runtime.md)，則不需要重新下載。

1. 使用您的Adobe ID登入[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderstestModied&amp;orderby.st.sort.st.st.st.st.st.st&amp;st.sted&amp;orde.sted&amp;ord&amp;orde.s.sted&amp;ord.se.s.st.st.s.se.st.se.se.s=desc&amp;d.se.se.st.s&amp;dsc&amp;des&amp;des&amp;desc&amp;s&amp;desc&amp;s&amp;d.sy=list&amp;p.offset=0&amp;p.limit=1)
   + 您的Adobe組織&#x200B;__必須__&#x200B;已布建AEM做為雲端服務，才能將AEM下載為雲端服務SDK
1. 按一下最新&#x200B;__AEM SDK__&#x200B;結果列以下載
   + 請確定下載說明中已注明AEM SDK的Dispatcher Tools v2.0.29+

## 從AEM SDK zip解壓縮Dispatcher Tools

>[!TIP]
>
> Windows用戶在包含本地調度器工具的資料夾的路徑中不能有空格或特殊字元。 如果路徑中存在空格，則`docker_run.cmd`將失敗。

Dispatcher Tools的版本與AEM SDK的版本不同。 請確定Dispatcher Tools版本是透過與AEM相符的AEM SDK版本提供，做為雲端服務版本。

1. 解壓縮下載的`aem-sdk-xxx.zip`檔案
1. 將Dispatcher Tools解壓縮到`~/aem-sdk/dispatcher`
   + Windows:將`aem-sdk-dispatcher-tools-x.x.x-windows.zip`解壓縮至`C:\Users\<My User>\aem-sdk\dispatcher`（視需要建立遺失的資料夾）
   + macOS / Linux:執行隨附的shell指令碼`aem-sdk-dispatcher-tools-x.x.x-unix.sh`以解壓縮Dispatcher工具
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

請注意，下面發出的所有命令假定當前工作目錄包含正在擴展的Dispatcher Tools內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。可使用等效的Windows/Linux命令實現類似結果*

## 瞭解Dispatcher配置檔案

>[!TIP]
> 從[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)建立的Experience Manager專案會預先填入此組Dispatcher組態檔案，因此不需要從Dispatcher Tools src資料夾複製。

Dispatcher Tools提供一組Apache HTTP Web伺服器和Dispatcher配置檔案，它們定義了所有環境的行為，包括本地開發。

如果Experience Manager Maven專案中不存在這些檔案，這些檔案將複製到Experience Manager Maven專案至`dispatcher/src`檔案夾。

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。可使用等效的Windows/Linux命令實現類似結果*

未打包的Dispatcher Tools中提供了配置檔案的完整說明，如`dispatcher-sdk-x.x.x/docs/Config.html`。

## 驗證配置

或者，Dispatcher和Apache Web伺服器配置（通過`httpd -t`）可以使用`validate`指令碼進行驗證（不要與`validator`執行檔混淆）。

+ 使用狀況:
   + Windows: `bin\validate src`
   + macOS / Linux:`./bin/validate.sh ./src`

## 在本地運行Dispatcher

要在本地運行Dispatcher，必須使用Dispatcher Tools的`validator` CLI工具生成Dispatcher配置檔案。

+ 使用狀況:
   + Windows:`bin\validator full -d out src`
   + macOS / Linux:`./bin/validator full -d ./out ./src`

此命令將配置傳輸到與Docker容器的Apache HTTP Web伺服器相容的檔案集中。

生成後，將使用經傳輸的配置在Docker容器中本地運行Dispatcher。 請務必確保最新配置已通過`validate` __和__&#x200B;輸出驗證器的`-d`選項進行驗證。

+ 使用狀況:
   + Windows:`bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux:`./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host`可設為`host.docker.internal`,Docker在容器中提供特殊的DNS名稱，可解析為主機的IP。 如果`host.docker.internal`未解析，請參閱下面的[疑難排解](#troubleshooting-host-docker-internal)一節。

例如，使用Dispatcher Tools提供的預設配置檔案啟動Dispatcher Docker容器：

1. 每次配置更改時，都從頭開始生成`deployment-folder`（按慣例命名為`out`）:

   + Windows:`del /Q out && bin\validator full -d out src`
   + macOS / Linux:`rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （重新）啟動Dispatcher Docker容器，提供部署資料夾的路徑：

   + Windows:`bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux:`./bin/docker_run.sh ./out host.docker.internal:4503 8080`

AEM作為雲端服務SDK的Publish Service，在埠4503上本機執行，可透過位於`http://localhost:8080`的Dispatcher取得。

要對Experience Manager項目的Dispatcher配置運行Dispatcher Tools，只需使用項目的`dispatcher/src`資料夾生成`deployment-folder`。

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。可使用等效的Windows/Linux命令實現類似結果*

## Dispatcher Tools日誌

在本機開發期間，Dispatcher記錄檔可協助您瞭解HTTP請求是否遭到封鎖，以及為何遭到封鎖。 通過使用環境參數預裝`docker_run`的執行，可以設定日誌級別。

運行`docker_run`時，Dispatcher Tools日誌會發出到標準輸出。

用於調試Dispatcher的有用參數包括：

+ `DISP_LOG_LEVEL=Debug` 將Dispatcher模組日誌設定為「調試」級別
   + 預設值為: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 將Apache HTTP Web伺服器重寫模組日誌設定為調試級別
   + 預設值為: `Warn`
+ `DISP_RUN_MODE` 設定Dispatcher環境的「運行模式」，載入相應的運行模式Dispatcher配置檔案。
   + 預設為 `dev`
+ 有效值：`dev`、`stage`或`prod`

一或多個參數，可傳遞至`docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。可使用等效的Windows/Linux命令實現類似結果*

### 記錄檔存取

Apache Web伺服器和AEM Dispatcher記錄檔可直接在Docker容器中存取：

+ [訪問Docker容器中的日誌](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [將Docker日誌複製到本地檔案系統](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何時更新Dispatcher Tools{#dispatcher-tools-version}

Dispatcher Tools版本的增加頻率比Experience Manager低，因此Dispatcher Tools在本地開發環境中所需的更新更少。

建議的Dispatcher Tools版本是與AEM搭售的Cloud Service SDK，與Experience Manager搭配為雲端服務版本。 AEM的雲端服務版本可透過[Cloud Manager](https://my.cloudmanager.adobe.com/)找到。

+ __「雲端管理員>環境__」，依據 __AEM Releaselabel指定的環__ 境

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

_請注意，Dispatcher Tools版本本身與Experience Manager版本不符。_

## 疑難排解

### docker_run導致「等到host.docker.internal可用」消息{#troubleshooting-host-docker-internal}

`host.docker.internal` 是提供給Docker的主機名，其中包含的主機名解析到主機。每個docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)、[Windows](https://docs.docker.com/docker-for-windows/networking/)):

> 從Docker 18.03開始，我們的建議是連接到特殊的DNS名稱host.docker.internal，該名稱解析到主機使用的內部IP地址

如果`bin/docker_run out host.docker.internal:4503 8080`導致消息&#x200B;__Waiting until host.docker.internal可用__&#x200B;時，則：

1. 確保安裝的Docker版本為18.03或更高版本
2. 您可能已設定本地電腦，使`host.docker.internal`名稱的註冊／解析度無法使用。 請改用您的本機IP。
   + Windows:
      + 在命令提示符下，執行`ipconfig` ，並記錄主機的&#x200B;__IPv4地址__。
      + 然後，使用此IP地址執行`docker_run` :
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + 在終端中，執行`ifconfig`並記錄主機&#x200B;__inet__ IP地址，通常為&#x200B;__en0__&#x200B;設備。
      + 然後使用主機IP地址執行`docker_run` :
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### 範例錯誤

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run導致&#39;**錯誤：找不到部署資料夾&#39;

當執行`docker_run.cmd`時，會顯示一個讀取&#x200B;__**錯誤的錯誤：找不到部署資料夾：__。 這通常是因為路徑中有空格。 如果可能，請刪除資料夾中的空格，或將`aem-sdk`資料夾移動到不包含空格的路徑。

例如，Windows用戶資料夾通常為`<First name> <Last name>` ，中間有空格。 在下面的示例中，資料夾`...\My User\...`包含一個空格，該空格會中斷本地Dispatcher Tools的`docker_run`執行。 如果空格位於Windows使用者資料夾中，請勿嘗試重新命名此資料夾，因為它會中斷Windows，而是將`aem-sdk`資料夾移至您的使用者有權完全修改的新位置。 請注意，假設`aem-sdk`資料夾位於使用者的首頁目錄中的指示，必須調整至新位置。

#### 範例錯誤

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run無法在Windows{#troubleshooting-windows-compatible}上啟動

在Windows上運行`docker_run`可能會導致以下錯誤，導致Dispatcher無法啟動。 Dispatcher on Windows已回報此問題，未來版本將會修正此問題。

#### 範例錯誤

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [下載AEM參考網站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-dispatcher/using/dispatcher.html)
