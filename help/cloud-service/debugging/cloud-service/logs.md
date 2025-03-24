---
title: 記錄
description: 記錄檔是在AEM as a Cloud Service中偵錯AEM應用程式的首選工具，但部署的AEM應用程式必須有充足的登入次數。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 1%

---

# 使用記錄檔對AEM as a Cloud Service除錯

記錄檔是在AEM as a Cloud Service中偵錯AEM應用程式的首選工具，但部署的AEM應用程式必須有充足的登入次數。

指定環境的AEM服務(作者、發佈/發佈Dispatcher)的所有記錄活動都會合併為單一記錄檔案，即使該服務中的不同pod會產生記錄陳述式亦然。

Pod ID會顯示在每個記錄陳述式中，並可篩選或排序記錄陳述式。 Pod ID的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 範例：`cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自訂記錄檔

AEM as a Cloud Services不支援自訂記錄檔，但支援自訂記錄。

若要讓Java記錄檔可在AEM as a Cloud Service中使用(透過[Cloud Manager](#cloud-manager)或[Adobe I/O CLI](#aio))，自訂記錄檔陳述式必須寫入`error.log`。 無法從AEM as a Cloud Service存取寫入自訂具名記錄（例如`example.log`）的記錄。

可以使用應用程式的`org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`檔案中的Sling LogManager OSGi設定屬性，將記錄檔寫入`error.log`。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM作者和發佈服務記錄檔

AEM製作和發佈服務都提供AEM執行階段伺服器記錄檔：

+ `aemerror`是Java錯誤記錄檔(可在AEM SDK本機Quickstart的`/crx-quickstart/logs/error.log`找到)。 以下是每個環境型別自訂記錄器的[建議記錄層級](#log-levels)：
   + 開發： `DEBUG`
   + 階段： `WARN`
   + 生產：`ERROR`
+ `aemaccess`列出對AEM服務的HTTP要求與詳細資料
+ `aemrequest`列出向AEM服務提出的HTTP要求及其對應的HTTP回應

## AEM發佈Dispatcher記錄檔

只有AEM Publish Dispatcher可提供Apache Web Server和Dispatcher記錄檔，因為這些方面僅存在於AEM發佈層級，而非AEM作者層級。

+ `httpdaccess`列出向AEM服務的Apache Web Server/Dispatcher發出的HTTP要求。
+ `httperror`列出來自Apache網頁伺服器的記錄訊息，並協助偵錯支援的Apache模組，例如`mod_rewrite`。
   + 開發： `DEBUG`
   + 階段： `WARN`
   + 生產：`ERROR`
+ `aemdispatcher`列出來自Dispatcher模組的記錄訊息，包括從快取訊息中篩選及服務。
   + 開發： `DEBUG`
   + 階段： `WARN`
   + 生產：`ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager可讓您透過環境的下載記錄動作，以天為單位下載記錄檔。

![Cloud Manager — 下載記錄檔](./assets/logs/download-logs.png)

您可以透過任何記錄分析工具下載及檢查這些記錄。

## Adobe I/O CLI搭配Cloud Manager增效模組{#aio}

Adobe Cloud Manager支援透過Adobe I/O CLI的[Adobe I/O CLI](https://github.com/adobe/aio-cli)及[Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager)存取AEM as a Cloud Service記錄檔。

首先，[使用Cloud Manager外掛程式](../../local-development-environment/development-tools.md#aio-cli)設定Adobe I/O。

請確定已識別相關的程式識別碼和環境識別碼，並使用[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)列出用於[tail](#aio-cli-tail-logs)或[下載](#aio-cli-download-logs)記錄的記錄選項。

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### 追蹤記錄{#aio-cli-tail-logs}

Adobe I/O CLI提供使用[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令即時從AEM as a Cloud Service追蹤記錄的功能。 在AEM as a Cloud Service環境中執行動作時，追蹤對於觀看即時記錄活動很有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令列工具（例如`grep`）可與`tail-logs`搭配使用，以協助隔離感興趣的記錄陳述式，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...只會顯示從`com.example.MySlingModel`產生或包含該字串的記錄陳述式。

### 正在下載記錄檔{#aio-cli-download-logs}

Adobe I/O CLI提供使用[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))命令從AEM as a Cloud Service下載記錄檔的功能。 這會提供與從Cloud Manager網頁UI下載記錄檔相同的最終結果，差異在於`download-logs`命令會根據要求的記錄檔天數，整合各天的記錄檔。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 瞭解記錄

AEM as a Cloud Service中的記錄有多個Pod，可將記錄陳述式寫入其中。 由於多個AEM執行個體會寫入相同的記錄檔，因此請務必瞭解如何在偵錯時分析和減少雜訊。 為了說明，使用了下列`aemerror`記錄檔片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod ID （日期和時間之後的資料點）時，記錄可以由Pod或服務內的AEM執行個體來整理，更易於追蹤和瞭解程式碼執行。

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 建議的記錄層級{#log-levels}

Adobe對於每個AEM as a Cloud Service環境的記錄層級的一般指引如下：

+ 本機開發(AEM SDK)： `DEBUG`
+ 開發： `DEBUG`
+ 階段： `WARN`
+ 生產：`ERROR`

針對每種環境型別設定最適當的記錄層級是AEM as a Cloud Service，記錄層級會保留在程式碼中

+ OSGi設定中會維護Java記錄設定
+ Dispatcher專案中的Apache網頁伺服器和Dispatcher記錄層級

...因此需要部署才能變更。

### 用於設定Java記錄層級的環境特定變數

為每個環境設定靜態的已知Java記錄層級的替代方法，是使用AEM做為Cloud Service的[環境特定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)來引數化記錄層級，以允許透過[Adobe I/O CLI搭配Cloud Manager外掛程式](#aio-cli)動態變更值。

這需要更新記錄OSGi設定，以使用環境特定的變數預留位置。 [記錄層級的預設值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values)應根據[Adobe建議](#log-levels)設定。 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

此方法有其必須考量的缺點：

+ [允許有限數量的環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，建立變數來管理記錄層級將使用一個。
+ 可透過[Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)、[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)和[Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)以程式設計方式管理環境變數。
+ 環境變數的變更必須由支援的工具手動重設。 若忘記將高流量環境（例如生產）重設為較不詳細的記錄層級，可能會讓記錄泛濫，並影響AEM的效能。

_環境特定變數無法用於Apache Web Server或Dispatcher記錄設定，因為這些設定未透過OSGi設定。_
