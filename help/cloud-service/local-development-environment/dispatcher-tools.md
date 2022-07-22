---
title: 設定Dispatcher工具AEM以進行as a Cloud Service開發
description: AEMSDK的Dispatcher Tools通過使Dispatcher易於在本地安裝、運AEM行和排除故障，為Adobe Experience Manager(Dispatcher)項目的本地開發提供了方便。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 2%

---

# 設定本地Dispatcher工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本地調度程式工具"
>abstract="Dispatcher是整個Experience Manager體系結構的一個組成部分，應是本地開發設定的一部分。 as a Cloud ServiceAEM的SDK包括推薦的Dispatcher Tools版本，它便於在本地配置、驗證和模擬Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="雲端中的 Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載AEMas a Cloud ServiceSDK"

Adobe Experience Manager(AEM)的Dispatcher是Apache HTTP Web伺服器模組，在CDN和AEM發佈層之間提供安全和效能層。 Dispatcher是整個Experience Manager體系結構的一個組成部分，應是本地開發設定的一部分。

as a Cloud ServiceAEM的SDK包括推薦的Dispatcher Tools版本，它便於在本地配置、驗證和模擬Dispatcher。 Dispatcher Tools包括：

+ 位於的Apache HTTP Web伺服器和Dispatcher配置檔案的基線集 `.../dispatcher-sdk-x.x.x/src`
+ 配置驗證程式CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validate`
+ 配置層代CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 使用Dispatcher模組運行Apache HTTP Web伺服器的Docker映像

請注意 `~` 用作用戶目錄的簡寫。 在Windows中，這相當於 `%HOMEPATH%`。

>[!NOTE]
>
> 本頁的視頻在macOS錄制。 Windows用戶可以隨後操作，但使用與每個視頻相同的Dispatcher Tools Windows命令。

## 必備條件

1. Windows用戶必須使用Windows 10 Professional（或支援Docker的版本）
1. 安裝 [Experience Manager發佈快速啟動Jar](./aem-runtime.md) 在本地開發機上。
   + （可選）安裝最新 [AEM參考網站](https://github.com/adobe/aem-guides-wknd/releases) 本地AEM發佈服務。 本教程使用此網站來直觀顯示正在工作的Dispatcher。
1. 安裝並啟動最新版本的 [多克](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下載Dispatcher Tools(作為SDK的一AEM部分)

as a Cloud ServiceAEMSDK(或AEMSDK)包含Dispatcher Tools，該Dispatcher Tools用於在本地運行Apache HTTP Web伺服器以進行開發，以及相容的QuickStart Jar。

如果AEMas a Cloud ServiceSDK已下載到 [設定本地運AEM行時](./aem-runtime.md)，不需要重新下載。

1. 登錄到 [experience.adobe.com/#/下載](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;orderby=%40jcr%3Acont%2Fjcr%3AlastModified&amp;orderby.sort.st&amp;rded&amp;lay=list&amp;p.offset=0&amp;p.limit=1) 你的Adobe ID
   + 您的Adobe組織 __必須__ 已設定AEMas a Cloud Service以下載AEMas a Cloud ServiceSDK
1. 按一下最新 __SDKAEM__ 要下載的結果行

## 從SDK zip提取DispatcherAEM Tools

>[!TIP]
>
> Windows用戶在包含本地Dispatcher工具的資料夾的路徑中不能有任何空格或特殊字元。 如果路徑中存在空格，則 `docker_run.cmd` 會失敗。

Dispatcher Tools的版本與SDK的版本AEM不同。 確保通過與as a Cloud Service版本匹配的AEMSDK版本提供Dispatcher ToolsAEM版本。

1. 解壓縮下載的 `aem-sdk-xxx.zip` 檔案
1. 將Dispatcher工具解包到 `~/aem-sdk/dispatcher`
   + 窗口：解壓縮 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 入 `C:\Users\<My User>\aem-sdk\dispatcher` （根據需要建立缺少的資料夾）
   + macOS/Linux:執行附帶的Shell指令碼 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 解壓縮Dispatcher Tools
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

請注意，下面發出的所有命令都假定當前工作目錄包含正在擴展的Dispatcher Tools內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*這段視頻用macOS做說明。 等效的Windows/Linux命令可用於獲得類似的結果*

## 瞭解Dispatcher配置檔案

>[!TIP]
> Experience Manager從 [馬文AEM原型計畫](https://github.com/adobe/aem-project-archetype) 已預填充這組Dispatcher配置檔案，因此無需從Dispatcher Tools src資料夾進行複製。

Dispatcher Tools提供一組Apache HTTP Web伺服器和Dispatcher配置檔案，這些配置檔案定義了所有環境的行為，包括本地開發。

這些檔案將複製到Experience ManagerMaven項目中 `dispatcher/src` 資料夾。

在解壓縮的Dispatcher Tools中，可獲得配置檔案的完整說明，如 `dispatcher-sdk-x.x.x/docs/Config.html`。

## 驗證配置

（可選）Dispatcher和Apache Web伺服器配置(通過 `httpd -t`)可使用 `validate` 指令碼(不要與 `validator` 可執行)。 的 `validate` 指令碼提供了一種方便的運行方法 [3個階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) 的 `validator`。

+ 使用狀況:
   + Windows: `bin\validate src`
   + macOS/Linux: `./bin/validate.sh ./src`

## 在本地運行Dispatcher

使用AEMDocker對 `src` Dispatcher和Apache Web伺服器配置檔案。

+ 使用狀況:
   + 窗口： `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

的 `<aem-publish-host>` 可以設定為 `host.docker.internal`, Docker在解析為主機IP的容器中提供了特殊的DNS名稱。 如果他 `host.docker.internal` 不解析，請參閱 [故障排除](#troubleshooting-host-docker-internal) 的下界。

例如，使用Dispatcher Tools提供的預設配置檔案啟動Dispatcher Docker容器：

啟動Dispatcher Docker容器，提供Dispatcher配置src資料夾的路徑：

+ 窗口： `bin\docker_run src host.docker.internal:4503 8080`
+ macOS/Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

在AEM埠4503上本地運行的as a Cloud ServiceSDK發佈服務將通過Dispatcher提供，地址為 `http://localhost:8080`。

要針對Experience Manager項目的Dispatcher配置運行Dispatcher Tools，請指向項目 `dispatcher/src` 的子菜單。

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher Tools日誌

在本地開發過程中，Dispatcher日誌有助於瞭解HTTP請求是否被阻止以及阻止的原因。 可以通過預先執行 `docker_run` 環境參數。

Dispatcher Tools日誌在 `docker_run` 。

用於調試Dispatcher的有用參數包括：

+ `DISP_LOG_LEVEL=Debug` 將Dispatcher模組日誌記錄設定為「調試」級別
   + 預設值為: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 將Apache HTTP Web伺服器重寫模組日誌記錄設定為調試級別
   + 預設值為: `Warn`
+ `DISP_RUN_MODE` 設定Dispatcher環境的「運行模式」，並載入相應的運行模式Dispatcher配置檔案。
   + 預設為 `dev`
+ 有效值： `dev`。 `stage`或 `prod`

可以將一個或多個參數傳遞到 `docker_run`

+ 窗口：

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### 日誌檔案訪問

可以在Docker容AEM器中直接訪問Apache Web伺服器和Dispatcher日誌：

+ [訪問Docker容器中的日誌](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [將Docker日誌複製到本地檔案系統](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何時更新Dispatcher工具{#dispatcher-tools-version}

Dispatcher Tools版本的增量比Experience Manager的增量更少，因此Dispatcher Tools在本地開發環境中需要的更新更少。

建議的Dispatcher Tools版本是與與Experience Manageras a Cloud Service版AEM本匹配的as a Cloud ServiceSDK捆綁的版本。 可以通AEM過 [雲管理器](https://my.cloudmanager.adobe.com/)。

+ __雲管理器>環境__，按指定的環境 __發AEM行__ 標籤

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

_請注意，Dispatcher Tools版本本身與Experience Manager版本不匹配。_

## 疑難排解

### docker_run導致「等到host.docker.internal可用」消息{#troubleshooting-host-docker-internal}

`host.docker.internal` 是提供給Docker的主機名，它包含解析到主機的主機名。 每個docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)。 [窗口](https://docs.docker.com/docker-for-windows/networking/)):

> 從Docker 18.03開始，我們的建議是連接到特殊的DNS名稱host.docker.internal，該名稱解析為主機使用的內部IP地址

如果，當 `bin/docker_run src host.docker.internal:4503 8080` 結果顯示消息 __等待主機.docker.internal可用__，則：

1. 確保已安裝的Docker版本為18.03或更高版本
2. 您可能設定了阻止註冊/解析的本地電腦 `host.docker.internal` 名稱。 改用本地IP。
   + 窗口：
      + 從命令提示符執行 `ipconfig`，並記錄主機的 __IPv4地址__ 主機。
      + 然後，執行 `docker_run` 使用此IP地址：
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS/Linux:
      + 從終端執行 `ifconfig` 並記錄主機 __內__ IP地址，通常 __en0__ 設備。
      + 然後執行 `docker_run` 使用主機IP地址：
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 示例錯誤

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run在Windows上啟動失敗{#troubleshooting-windows-compatible}

正在運行 `docker_run` 在Windows上可能導致以下錯誤，導致Dispatcher無法啟動。 這是Windows上Dispatcher報告的問題，將在以後的版本中解決。

#### 示例錯誤

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run src host.docker.internal:4503 8080

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

+ [下載AEMSDK](https://experience.adobe.com/#/downloads)
+ [Adobe雲管理器](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [下載AEM參考網站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager調度程式文檔](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
