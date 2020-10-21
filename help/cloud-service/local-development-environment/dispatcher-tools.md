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
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# 設定本地Dispatcher工具

Adobe Experience Manager(AEM)的Dispatcher是Apache HTTP Web伺服器模組，可在CDN和AEM Publish層之間提供安全性與效能層。 Dispatcher是整體Experience Manager架構的一部分，應是本端開發設定的一部分。

AEM a Cloud Service SDK包含建議的Dispatcher Tools版本，可協助在本機設定、驗證和模擬Dispatcher。 Dispatcher Tools由以下組成：

+ Apache HTTP Web伺服器和Dispatcher配置檔案的基準集，位於 `.../dispatcher-sdk-x.x.x/src`
+ 配置驗證器CLI工具，位 `.../dispatcher-sdk-x.x.x/bin/validate` 於(Dispatcher SDK 2.0.29+)
+ 配置生成CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 使用Dispatcher模組運行Apache HTTP Web伺服器的Docker映像

請注意， `~` 這是用戶目錄的速記。 在Windows中，這相當於 `%HOMEPATH%`。

>[!NOTE]
>
> 本頁中的影片是在macOS上錄制的。 Windows用戶可以跟進，但使用與每個視頻一起提供的等效Dispatcher Tools Windows命令。

## 必備條件

1. Windows使用者必須使用Windows 10 Professional
1. 在本 [機開發機器上安裝Experience Manager Publish Quickstart Jar](./aem-runtime.md) 。
   + （可選）在本機 [AEM Publish服務上安裝最新](https://github.com/adobe/aem-guides-wknd/releases) AEM參考網站。 本教程使用此網站來直觀顯示工作的Dispatcher。
1. 在本機開發機器上安裝並啟動 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)的最新版本。

## 下載Dispatcher Tools（做為AEM SDK的一部分）

AEM(Cloud Service SDK)或AEM SDK包含Dispatcher Tools，可用來在本機執行Apache HTTP Web伺服器並搭配Dispatcher模組進行開發，以及相容的QuickStart Jar。

如果AEM已下載為Cloud Service SDK，以 [設定本機AEM執行階段](./aem-runtime.md)，則不需要重新下載。

1. 使用您 [的Adobe ID登入experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderstestModied&amp;orderby.st.sort.st.st.st.st.st.st&amp;st.sted&amp;orde.sted&amp;ord&amp;orde.s.sted&amp;ord.se.s.st.st.s.se.st.se.se.s=desc&amp;d.se.se.st.s&amp;dsc&amp;des&amp;des&amp;desc&amp;s&amp;desc&amp;s&amp;d.sy=list&amp;p.offset=0&amp;p.limit=1)
   + 您的Adobe組 __織必須__ 布建AEM做為雲端服務，才能將AEM下載為雲端服務SDK
1. 按一下最新 __的AEM SDK__ 結果列以下載
   + 請確定下載說明中已注明AEM SDK的Dispatcher Tools v2.0.29+

## 從AEM SDK zip解壓縮Dispatcher Tools

>[!TIP]
>
> Windows用戶在包含本地調度器工具的資料夾的路徑中不能有空格或特殊字元。 如果路徑中存在空格，則 `docker_run.cmd` 將失敗。

Dispatcher Tools的版本與AEM SDK的版本不同。 請確定Dispatcher Tools版本是透過與AEM相符的AEM SDK版本提供，做為雲端服務版本。

1. 解壓縮下載的檔 `aem-sdk-xxx.zip` 案
1. 將Dispatcher Tools解壓縮到 `~/aem-sdk/dispatcher`
   + Windows:解壓縮 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 至( `C:\Users\<My User>\aem-sdk\dispatcher` 視需要建立遺失的檔案夾)
   + macOS / Linux:執行隨附的shell指令碼以解 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 壓縮Dispatcher Tools
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

請注意，下面發出的所有命令假定當前工作目錄包含正在擴展的Dispatcher Tools內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。 等效的Windows/Linux命令可用於獲得類似的結果*

## 瞭解Dispatcher配置檔案

>[!TIP]
> 從 [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) （Aexperience Manager專案）建立的專案會預先填入此組Dispatcher組態檔案，因此不需要從Dispatcher Tools src檔案夾複製。

Dispatcher Tools提供一組Apache HTTP Web伺服器和Dispatcher配置檔案，它們定義了所有環境的行為，包括本地開發。

如果Experience Manager Maven專案中不存在這些檔案， `dispatcher/src` 則這些檔案會複製到Experience Manager Maven專案中。

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*此視訊使用macOS做為說明用途。 等效的Windows/Linux命令可用於獲得類似的結果*

在解壓縮的Dispatcher Tools中，可獲得配置檔案的完整說明，如 `dispatcher-sdk-x.x.x/docs/Config.html`。

## 驗證配置

或者，Dispatcher和Apache Web伺服器配置(通過 `httpd -t`)可以使用指令碼進行驗證( `validate` 不要與執行檔 `validator` 混淆)。

+ 使用狀況:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate ./src`

## 在本地運行Dispatcher

要在本地運行Dispatcher，必須使用Dispatcher Tools的 `validator` CLI工具生成Dispatcher配置檔案。

+ 使用狀況:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

此命令將配置傳輸到與Docker容器的Apache HTTP Web伺服器相容的檔案集中。

生成後，將使用經傳輸的配置在Docker容器中本地運行Dispatcher。 請務必確保最新配置已通過驗證器選 `validate` 項的 __驗證__ ，並使用驗證器 `-d` 選項輸出。

+ 使用狀況:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

可以 `aem-publish-host` 設定為，Docker在容 `host.docker.internal`器中提供一個特殊的DNS名稱，該名稱解析到主機的IP。 如果他 `host.docker.internal` 未解決問題，請參閱下 [面的疑難排解](#troubleshooting-host-docker-internal) 。

例如，使用Dispatcher Tools提供的預設配置檔案啟動Dispatcher Docker容器：

1. 每次配 `deployment-folder`置更改 `out` 時，從頭開始生成按慣例命名的：

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （重新）啟動Dispatcher Docker容器，提供部署資料夾的路徑：

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

AEM as a Cloud Service SDK&#39;s Publish Service，在埠4503本機上執行，將可透過Dispatcher at取得 `http://localhost:8080`。

要對Experience Manager項目的Dispatcher配置運行Dispatcher Tools，只需使用項 `deployment-folder` 目的資料夾生成 `dispatcher/src` 即可。

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

*此視訊使用macOS做為說明用途。 等效的Windows/Linux命令可用於獲得類似的結果*

## Dispatcher Tools日誌

在本機開發期間，Dispatcher記錄檔可協助您瞭解HTTP請求是否遭到封鎖，以及為何遭到封鎖。 可通過預先執行環境參數來設定 `docker_run` 日誌級別。

Dispatcher Tools日誌在運行時發出到標準 `docker_run` 輸出。

用於調試Dispatcher的有用參數包括：

+ `DISP_LOG_LEVEL=Debug` 將Dispatcher模組日誌設定為「調試」級別
   + 預設值為: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 將Apache HTTP Web伺服器重寫模組日誌設定為調試級別
   + 預設值為: `Warn`
+ `DISP_RUN_MODE` 設定Dispatcher環境的「運行模式」，載入相應的運行模式Dispatcher配置檔案。
   + 預設為 `dev`
+ 有效值： `dev`、 `stage`或 `prod`

一或多個參數，可傳遞至 `docker_run`

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

*此視訊使用macOS做為說明用途。 等效的Windows/Linux命令可用於獲得類似的結果*

## 何時更新Dispatcher Tools{#dispatcher-tools-version}

Dispatcher Tools版本的增加頻率比Experience Manager低，因此Dispatcher Tools在本地開發環境中所需的更新更少。

建議的Dispatcher Tools版本是與AEM搭售的Cloud Service SDK，與Experience Manager搭配為雲端服務版本。 AEM的雲端服務版本可透過 [Cloud Manager找到](https://my.cloudmanager.adobe.com/)。

+ __「Cloud Manager >環境__」，依 __AEM Release標籤所指定的環境__

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

_請注意，Dispatcher Tools版本本身與Experience Manager版本不符。_

## 疑難排解

### docker_run導致「等待host.docker.internal可用」消息{#troubleshooting-host-docker-internal}

`host.docker.internal` 是提供給Docker的主機名，其中包含的主機名解析到主機。 Per docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)、 [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> 從Docker 18.03開始，我們的建議是連接到特殊的DNS名稱host.docker.internal，該名稱解析到主機使用的內部IP地址

如果，當出 `bin/docker_run out host.docker.internal:4503 8080` 現「Waiting untilt __host.docker.internal is available__（等待ost.docker.internal可用）」消息時，則：

1. 確保安裝的Docker版本為18.03或更高版本
2. 您可能已設定本地電腦，阻止對名稱進行註冊／解 `host.docker.internal` 析。 請改用您的本機IP。
   + Windows:
      + 在命令提示符下，執 `ipconfig`行並記錄主機 __的IPv4地址__ 。
      + 然後，使 `docker_run` 用此IP地址執行：
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + 在終端機中，執 `ifconfig` 行並記錄主 __機inet__ IP地址，通常 __是en0__ 設備。
      + 然後使 `docker_run` 用主機IP地址執行：
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

當執行 `docker_run.cmd`時，會顯示讀取** __錯誤的錯誤：找不到部署資料夾：__。 這通常是因為路徑中有空格。 如果可能，請刪除資料夾中的空格，或將 `aem-sdk` 資料夾移動到不包含空格的路徑。

例如，Windows使用者資料夾通常 `<First name> <Last name>`是，中間有空格。 在下面的示例中，該文 `...\My User\...` 件夾包含一個空格，該空格會中斷本地Dispatcher Tools的執 `docker_run` 行。 如果空格位於Windows使用者資料夾中，請勿嘗試重新命名此資料夾，因為它會中斷Windows，而是將資料夾移至您的使用者有權完全修改的新位置。 `aem-sdk` 請注意，假定該文 `aem-sdk` 件夾位於用戶的主目錄中的說明，需要調整到新位置。

#### 範例錯誤

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run無法在Windows上啟動{#troubleshooting-windows-compatible}

在Windows `docker_run` 上運行可能會導致以下錯誤，導致Dispatcher無法啟動。 Dispatcher on Windows已回報此問題，未來版本將會修正此問題。

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
