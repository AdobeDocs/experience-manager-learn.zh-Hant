---
title: 設定AEMas a Cloud Service開發的Dispatcher工具
description: AEM SDK的Dispatcher工具可讓您在本機輕鬆安裝、執行和疑難排解Adobe Experience Manager(AEM)專案，以利本機開發。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: eb31c5fb79e01e1c363fc153355e8d92d1a54021
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 9%

---

# 設定本機Dispatcher工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本機 Dispatcher 工具"
>abstract="Dispatcher 是整個 Experience Manager 架構的組成部分，應該是本機開發設定的一部分。AEM as a Cloud Service SDK 包括建議的 Dispatcher 工具版本，該版本有助於在本機設定、驗證和模擬 Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="雲端中的 Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載 AEM as a Cloud Service SDK"

Adobe Experience Manager(AEM)的Dispatcher是Apache HTTP Web伺服器模組，可在CDN和AEM Publish層級之間提供安全性和效能層。 Dispatcher 是整個 Experience Manager 架構的組成部分，應該是本機開發設定的一部分。

AEM as a Cloud Service SDK 包括建議的 Dispatcher 工具版本，該版本有助於在本機設定、驗證和模擬 Dispatcher。Dispatcher工具由下列部分組成：

+ Apache HTTP Web伺服器和Dispatcher組態檔的基準集，位於 `.../dispatcher-sdk-x.x.x/src`
+ 配置驗證器CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validate`
+ 配置生成CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 不可修改的配置檔案，覆蓋位於 `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ 使用Dispatcher模組執行Apache HTTP Web伺服器的Docker影像

請注意 `~` 用作用戶目錄的簡稱。 在Windows中，這等同於 `%HOMEPATH%`.

>[!NOTE]
>
> 本頁的影片已記錄在macOS上。 Windows使用者可以跟著，但使用每個視訊隨附的同等Dispatcher工具Windows命令。

## 必備條件

1. Windows用戶必須使用Windows 10 Professional（或支援Docker的版本）
1. 安裝 [Experience Manager發佈快速入門Jar](./aem-runtime.md) 在本地開發機上。

+ （可選）安裝最新 [AEM參考網站](https://github.com/adobe/aem-guides-wknd/releases) 在本機AEM Publish服務上。 本教學課程會使用此網站來視覺化運作中的Dispatcher。

1. 安裝並啟動最新版本的 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下載Dispatcher工具(附於AEM SDK)

AEMas a Cloud ServiceSDK(或AEM SDK)包含Dispatcher工具，用於在本機執行具有Dispatcher模組的Apache HTTP Web伺服器，以進行開發，以及相容的QuickStart Jar。

如果AEMas a Cloud ServiceSDK已下載至 [設定本機AEM執行階段](./aem-runtime.md)，則不需要重新下載。

1. 登入 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) 與Adobe ID
   + 您的Adobe組織 __必須__ 布建AEMas a Cloud Service以下載AEMas a Cloud Service SDK
1. 按一下最新 __AEM SDK__ 結果行下載

## 從AEM SDK壓縮Dispatcher工具

>[!TIP]
>
> Windows使用者在包含本機Dispatcher工具之資料夾的路徑中，不能有任何空格或特殊字元。 如果路徑中有空格，則 `docker_run.cmd` 失敗。

Dispatcher工具的版本與AEM SDK的版本不同。 請確定Dispatcher工具的版本是透過符合AEMas a Cloud Service版本的AEM SDK版本提供。

1. 將下載的郵遞區號解壓縮 `aem-sdk-xxx.zip` 檔案
1. 將Dispatcher工具解壓縮至 `~/aem-sdk/dispatcher`

+ 窗口：解壓縮 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` into `C:\Users\<My User>\aem-sdk\dispatcher` （視需要建立遺失資料夾）
+ macOS Linux®:執行隨附的shell指令碼 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 解壓縮Dispatcher工具
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

下面發出的所有命令都假定當前工作目錄包含展開的Dispatcher工具內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*此影片說明用途使用macOS。 可以使用等效的Windows/Linux命令來獲得類似的結果。*

## 了解Dispatcher設定檔

>[!TIP]
> Experience Manager從 [AEM專案Maven原型](https://github.com/adobe/aem-project-archetype) 會預先填入這組Dispatcher設定檔，因此不需要從Dispatcher工具src資料夾複製。

Dispatcher工具提供一組Apache HTTP Web伺服器和Dispatcher設定檔，可定義所有環境的行為，包括本機開發。

這些檔案會複製到Experience ManagerMaven專案中，並複製到 `dispatcher/src` ，如果Experience ManagerMaven專案中尚未存在。

未封裝的Dispatcher工具中提供設定檔案的完整說明，如下所示 `dispatcher-sdk-x.x.x/docs/Config.html`.

## 驗證配置

（可選）Dispatcher和Apache Web伺服器設定(透過 `httpd -t`)可透過 `validate` 指令碼(不要與 `validator` 執行檔)。 此 `validate` 指令碼提供了一種方便的運行方式 [三階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) 的 `validator`.

+ 使用狀況:
   + Windows: `bin\validate src`
   + macOS Linux®: `./bin/validate.sh ./src`

## 在本機執行Dispatcher

AEM Dispatcher是使用Docker對 `src` Dispatcher與Apache Web伺服器組態檔。

+ 使用狀況:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

此 `<aem-publish-host>` 可設為 `host.docker.internal`,Docker在解析至主機IP的容器中提供特殊的DNS名稱。 若 `host.docker.internal` 無法解析，請參閱 [疑難排解](#troubleshooting-host-docker-internal) 一節。

例如，若要使用Dispatcher工具提供的預設組態檔來啟動Dispatcher Docker容器：

啟動Dispatcher Docker容器，提供Dispatcher設定src資料夾的路徑：

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

在連接埠4503上本機執行的AEMas a Cloud ServiceSDK發佈服務，可透過Dispatcher取得，網址為 `http://localhost:8080`.

若要針對Experience Manager專案的Dispatcher設定執行Dispatcher工具，請指向您專案的 `dispatcher/src` 檔案夾。

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher工具記錄檔

在本機開發期間，Dispatcher記錄檔有助於了解HTTP要求是否遭到封鎖及其原因。 您可以預先設定 `docker_run` 搭配環境參數。

Dispatcher工具記錄檔會在 `docker_run` 執行中。

用於偵錯Dispatcher的實用參數包括：

+ `DISP_LOG_LEVEL=Debug` 將Dispatcher模組記錄設為「除錯」層級
   + 預設值為: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 將Apache HTTP Web伺服器重寫模組日誌設定為Debug級別
   + 預設值為: `Warn`
+ `DISP_RUN_MODE` 設定Dispatcher環境的「執行模式」，並載入相應的執行模式Dispatcher設定檔案。
   + 預設為 `dev`
+ 有效值： `dev`, `stage`，或 `prod`

一或多個參數可傳遞至 `docker_run`

+ Windows:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### 記錄檔存取

您可以在Docker容器中直接存取Apache Web伺服器和AEM Dispatcher記錄檔：

+ [存取Docker容器中的記錄](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [將Docker日誌複製到本地檔案系統](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 更新Dispatcher工具的時機{#dispatcher-tools-version}

Dispatcher工具版本的遞增頻率比Experience Manager低，因此，Dispatcher工具在本機開發環境中需要的更新較少。

建議的Dispatcher工具版本是與AEMas a Cloud ServiceSDK搭配的，與Experience Manageras a Cloud Service版本相符。 AEM as a Cloud Service版本可透過 [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager >環境__，根據 __AEM版本__ 標籤

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

*請注意，Dispatcher工具版本與Experience Manager版本不符。*

## 如何更新Apache和Dispatcher設定的基準集

AEM Apache和Dispatcher設定的基準集會定期增強，並隨as a Cloud ServiceSDK版本一併發行。 最佳實務是將基線設定增強功能併入您的AEM專案，並避免 [本機驗證](#validate-configurations) 和Cloud Manager管道故障。 使用 `update_maven.sh` 指令碼 `.../dispatcher-sdk-x.x.x/bin` 檔案夾。

>[!VIDEO](https://video.tv.adobe.com/v/3416744/?quality=12&learn=on)

*此影片說明用途使用macOS。 可以使用等效的Windows/Linux命令來獲得類似的結果。*


假設您過去是使用 [AEM專案原型](https://github.com/adobe/aem-project-archetype)，則基準Apache和Dispatcher設定為最新。 使用這些基線配置時，將重複使用並複製檔案(如 `*.vhost`, `*.conf`, `*.farm` 和 `*.any` 從 `dispatcher/src/conf.d` 和 `dispatcher/src/conf.dispatcher.d` 資料夾。 您的本機Dispatcher驗證和Cloud Manager管道運作正常。

同時，基線Apache和Dispatcher設定也因多項原因而增強，例如新功能、安全性修正和最佳化。 這些工具會在AEMas a Cloud Service版本中透過較新版本的Dispatcher工具發行。

現在，根據最新Dispatcher工具版本驗證專案專用的Dispatcher設定時，開始失敗。 若要解決此問題，必須使用下列步驟來更新基線設定：

+ 驗證最新Dispatcher工具版本的驗證失敗

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ 使用 `update_maven.sh` 指令碼

   ```shell
   $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Updating dispatcher configuration at folder 
   running in 'extract' mode
   running in 'extract' mode
   reading immutable file list from /etc/httpd/immutable.files.txt
   preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
   ...
   immutable files extraction COMPLETE
   fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
   Cloud manager validator 2.0.53
   ```

+ 驗證更新的不可變檔案，如 `dispatcher_vhost.conf`, `default.vhost`，和 `default.farm` 並視需要對衍生自這些檔案的自訂檔案進行相關變更。

+ 重新驗證設定，應通過

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ 在本地驗證更改後，提交更新的配置檔案

## 疑難排解

### docker_run結果「等到host.docker.internal可用」消息{#troubleshooting-host-docker-internal}

此 `host.docker.internal` 是提供給Docker包含的主機名，它解析到主機。 根據docs.docker.com([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> 建議從Docker 18.03開始連線至特殊的DNS名稱host.docker.internal，其解析至主機使用的內部IP位址

當 `bin/docker_run src host.docker.internal:4503 8080` 結果訊息 __等待主機.docker.internal可用__，然後：

1. 確保安裝的Docker版本為18.03或更高版本
2. 您可能已設定本機電腦，以防止 `host.docker.internal` 名稱。 請改用本機IP。
   + Windows:
   + 在命令提示符下，執行 `ipconfig`，並記錄主機的 __IPv4地址__ 主機。
   + 然後，執行 `docker_run` 使用此IP地址：
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®:
   + 從終端機執行 `ifconfig` 並記錄主機 __ine__ IP位址，通常為 __en0__ 裝置。
   + 然後執行 `docker_run` 使用主機IP地址：
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 範例錯誤

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [下載AEM參考網站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience ManagerDispatcher檔案](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
