---
title: 設定Dispatcher工具以進行AEM as a Cloud Service開發
description: AEM SDK的Dispatcher工具可讓您在本機輕鬆安裝、執行及疑難排解Adobe Experience Manager (AEM)專案，協助本機Dispatcher開發。
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 4%

---

# 設定本機 Dispatcher 工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本機 Dispatcher 工具"
>abstract="Dispatcher 是整個 Experience Manager 架構的組成部分，應該是本機開發設定的一部分。AEM as a Cloud Service SDK 包括建議的 Dispatcher 工具版本，該版本可協助在本機設定、驗證和模擬 Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=zh-Hant" text="雲端中的 Dispatcher"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=zh-Hant" text="下載 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM)的Dispatcher是Apache HTTP Web伺服器模組，可在CDN和AEM發佈層級之間提供安全性與效能層。 Dispatcher是整體Experience Manager架構不可或缺的一部分，並應成為本機開發設定的一部分。

AEM as a Cloud Service SDK包含建議的Dispatcher工具版本，可協助設定驗證並在本機模擬Dispatcher。 Dispatcher Tools由以下部分組成：

+ 位於`.../dispatcher-sdk-x.x.x/src`的Apache HTTP Web Server和Dispatcher組態檔基準組
+ 位於`.../dispatcher-sdk-x.x.x/bin/validate`的組態驗證器CLI工具
+ 位於`.../dispatcher-sdk-x.x.x/bin/validator`的組態產生CLI工具
+ 位於`.../dispatcher-sdk-x.x.x/bin/docker_run`的組態部署CLI工具
+ 不可變的組態檔覆寫CLI工具，位於`.../dispatcher-sdk-x.x.x/bin/update_maven`
+ 使用Dispatcher模組執行Apache HTTP Web伺服器的Docker影像

請注意，`~`是用作使用者目錄的速記。 在Windows中，這相當於`%HOMEPATH%`。

>[!NOTE]
>
> 此頁面中的影片是錄製在macOS上。 Windows使用者可以跟著進行，但會使用每個視訊提供的對等Dispatcher Tools Windows命令。

## 先決條件

1. Windows使用者必須使用Windows 10專業版（或支援Docker的版本）
1. 在本機開發電腦上安裝[Experience Manager發佈快速入門Jar](./aem-runtime.md)。

+ 可選擇在本機AEM發佈服務上安裝最新的[AEM參考網站](https://github.com/adobe/aem-guides-wknd/releases)。 本教學課程會使用此網站以視覺效果呈現運作中的Dispatcher。

1. 在本機開發電腦上安裝並啟動最新版本的[Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下載Dispatcher工具(做為AEM SDK的一部分)

AEM as a Cloud Service SDK (又稱AEM SDK)包含用於在本機執行Apache HTTP Web伺服器(包含Dispatcher模組以進行開發)的Dispatcher工具，以及相容的QuickStart Jar。

如果AEM as a Cloud Service SDK已下載至[設定本機AEM執行階段](./aem-runtime.md)，則不需要重新下載。

1. 使用您的Adobe ID登入[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
   + 您的Adobe組織&#x200B;__必須__&#x200B;已布建給AEM as a Cloud Service，才能下載AEM as a Cloud Service SDK
1. 按一下要下載的最新&#x200B;__AEM SDK__&#x200B;結果列

## 從AEM SDK zip解壓縮Dispatcher工具

>[!TIP]
>
> Windows使用者在包含「本機Dispatcher工具」的資料夾路徑中不能有任何空格或特殊字元。 如果路徑中存在空格，`docker_run.cmd`會失敗。

Dispatcher工具的版本與AEM SDK的版本不同。 確保透過與Dispatcher版本相符的AEM SDK版本來提供AEM as a Cloud Service工具的版本。

1. 解壓縮下載的`aem-sdk-xxx.zip`檔案
1. 將Dispatcher工具解壓縮至`~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

將`aem-sdk-dispatcher-tools-x.x.x-windows.zip`解壓縮至`C:\Users\<My User>\aem-sdk\dispatcher` （視需要建立缺少的資料夾）。

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

以下發出的所有命令都假設目前的工作目錄包含展開的Dispatcher工具內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*此影片使用macOS作說明用途。 可使用同等的Windows/Linux命令來取得類似的結果。*

## 瞭解Dispatcher設定檔

>[!TIP]
> 從[Experience Manager專案Maven Archetype](https://github.com/adobe/aem-project-archetype)建立的AEM專案會預先填入這組Dispatcher設定檔案，因此不需要從Dispatcher Tools src資料夾進行複製。

Dispatcher工具提供一組Apache HTTP Web伺服器和Dispatcher設定檔，可定義所有環境（包括本機開發）的行為。

如果這些檔案不存在於Experience Manager Maven專案中，這些檔案將會複製到Experience Manager Maven專案中的`dispatcher/src`資料夾。

在解壓縮的Dispatcher工具中，組態檔的完整說明為`dispatcher-sdk-x.x.x/docs/Config.html`。

## 驗證設定

可選擇使用`validate`指令碼驗證Dispatcher和Apache Web伺服器設定（透過`httpd -t`） （請勿與`validator`可執行檔混淆）。 `validate`指令碼提供執行`validator`的[三個階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=zh-Hant)的便利方式。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## 在本機執行Dispatcher

AEM Dispatcher是使用Docker針對`src` Dispatcher和Apache Web伺服器組態檔在本機執行。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload`可執行檔優先於`docker_run`，因為它會在組態檔變更時重新載入組態檔，而不需手動終止和重新啟動`docker_run`。 或者，可以使用`docker_run`，但是當組態檔變更時，它需要手動終止和重新啟動`docker_run`。

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload`可執行檔優先於`docker_run`，因為它會在組態檔變更時重新載入組態檔，而不需手動終止和重新啟動`docker_run`。 或者，可以使用`docker_run`，但是當組態檔變更時，它需要手動終止和重新啟動`docker_run`。

>[!ENDTABS]

`<aem-publish-host>`可設為`host.docker.internal`，這是Docker在解析為主機電腦IP的容器中提供的特殊DNS名稱。 如果`host.docker.internal`無法解析，請參閱下列[疑難排解](#troubleshooting-host-docker-internal)一節。

例如，若要使用Dispatcher工具提供的預設設定檔案啟動Dispatcher Docker容器：

啟動Dispatcher Docker容器，提供Dispatcher設定src資料夾的路徑：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

透過Dispatcher在`http://localhost:8080`提供於連線埠4503本機執行的AEM as a Cloud Service SDK的發佈服務。

若要對Experience Manager專案的Dispatcher設定執行Dispatcher工具，請指向您專案的`dispatcher/src`資料夾。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher工具記錄檔

在本機開發期間，Dispatcher記錄有助於瞭解HTTP請求是否遭到封鎖以及為何遭到封鎖。 記錄層級可以透過以環境引數為執行`docker_run`加上前置字元來設定。

執行`docker_run`時，會將Dispatcher工具記錄檔發出至標準。

用於偵錯Dispatcher的實用引數包括：

+ `DISP_LOG_LEVEL=Debug`將Dispatcher模組記錄設定為偵錯層級
   + 預設值為： `Warn`
+ `REWRITE_LOG_LEVEL=Debug`將Apache HTTP Web伺服器重寫模組記錄設定為Debug層級
   + 預設值為： `Warn`
+ `DISP_RUN_MODE`會設定Dispatcher環境的「執行模式」，載入對應的執行模式Dispatcher設定檔。
   + 預設為`dev`
+ 有效值： `dev`、`stage`或`prod`

一或多個引數可以傳遞至`docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### 記錄檔存取

Apache Web Server和AEM Dispatcher記錄檔可直接在Docker容器中存取：

+ [存取Docker容器中的日誌](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [將Docker日誌複製到本機檔案系統](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何時更新Dispatcher工具{#dispatcher-tools-version}

Dispatcher工具版本的增加頻率低於Experience Manager，因此Dispatcher工具在本機開發環境中所需的更新較少。

建議的Dispatcher Tools版本是，此版本與Experience Manager as a Cloud Service版本相符的AEM as a Cloud Service SDK搭配。 可透過[Cloud Manager](https://my.cloudmanager.adobe.com/)找到AEM as a Cloud Service的版本。

+ __Cloud Manager >環境__，依由&#x200B;__AEM版本__&#x200B;標籤指定的環境而定

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

*請注意，Dispatcher Tools版本與Experience Manager版本不符。*

## 如何更新Apache和Dispatcher設定的基準組

Apache和Dispatcher設定的基準集已定期增強，並隨AEM as a Cloud Service SDK版本一起發行。 最佳實務是將基準設定增強功能合併到您的AEM專案中，並避免[本機驗證](#validate-configurations)和Cloud Manager管道失敗。 使用`.../dispatcher-sdk-x.x.x/bin`資料夾中的`update_maven.sh`指令碼更新它們。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*此影片使用macOS作說明用途。 可使用同等的Windows/Linux命令來取得類似的結果。*


假設您過去曾使用[AEM專案原型](https://github.com/adobe/aem-project-archetype)建立AEM專案，基準Apache和Dispatcher設定為最新。 使用這些基準組態，您的專案特定組態是透過重複使用和從`dispatcher/src/conf.d`和`dispatcher/src/conf.dispatcher.d`資料夾複製`*.vhost`、`*.conf`、`*.farm`和`*.any`等檔案所建立的。 您的本機Dispatcher驗證和Cloud Manager管道運作正常。

同時，由於新功能、安全性修正和最佳化等多種原因，基準Apache和Dispatcher設定已獲得增強。 做為AEM as a Cloud Service發行版本的一部分，透過較新版本的Dispatcher Tools發行。

現在，針對最新的Dispatcher工具版本驗證您專案專屬的Dispatcher設定時，這些設定會開始失敗。 若要解決此問題，必須使用下列步驟更新基準線設定：

+ 確認針對最新Dispatcher Tools版本的驗證失敗

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ 使用`update_maven.sh`指令碼更新不可變檔案

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

+ 驗證已更新的不可變檔案（例如`dispatcher_vhost.conf`、`default.vhost`和`default.farm`），並視需要在自訂檔案中進行衍生自這些檔案的相關變更。

+ 重新驗證設定，應該會通過

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

+ 在本地驗證變更後，提交更新的設定檔案

## 疑難排解

### docker_run導致「等到host.docker.internal可用」訊息{#troubleshooting-host-docker-internal}

`host.docker.internal`是提供給Docker包含的主機名稱，可解析成主機。 根據docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/)， [Windows](https://docs.docker.com/desktop/networking/))：

> 從Docker 18.03開始，建議連線到特殊的DNS名稱host.docker.internal，這會解析為主機使用的內部IP位址

當`bin/docker_run src host.docker.internal:4503 8080`導致訊息&#x200B;__等候host.docker.internal可用時__，則：

1. 確保已安裝的Docker版本為18.03或更高版本
1. 您可能設定了本機電腦，無法註冊/解析`host.docker.internal`名稱。 請改用您的本機IP。

>[!BEGINTABS]

>[!TAB macOS]

+ 從終端機，執行`ifconfig`並記錄主機&#x200B;__inet__ IP位址，通常是&#x200B;__en0__&#x200B;裝置。
+ 然後使用主機IP位址執行`docker_run`： `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ 在命令提示字元中，執行`ipconfig`並記錄主機的&#x200B;__IPv4位址__。
+ 然後，使用此IP位址執行`docker_run`： `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ 從終端機，執行`ifconfig`並記錄主機&#x200B;__inet__ IP位址，通常是&#x200B;__en0__&#x200B;裝置。
+ 然後使用主機IP位址執行`docker_run`： `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### 錯誤範例

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [下載AEM參考網站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher檔案](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
