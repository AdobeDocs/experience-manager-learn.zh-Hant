---
title: 記錄檔
description: 記錄檔是AEMas a Cloud Service中除錯AEM應用程式的最前線，但部署的AEM應用程式必須具備充足登入率。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: eb669d1e2493d9b4a973314ab1323764920ba220
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 1%

---

# 使用記錄檔除錯AEMas a Cloud Service

記錄檔是AEMas a Cloud Service中除錯AEM應用程式的最前線，但部署的AEM應用程式必須具備充足登入率。

指定環境的AEM服務（製作、發佈/發佈Dispatcher）的所有記錄活動都會整合為單一記錄檔，即使該服務中的不同Pod會產生記錄陳述式亦然。

Pod Id會提供在每個log陳述式中，並允許篩選或整理log陳述式。 Pod Id的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 範例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自訂記錄檔

AEM as aCloud Services不支援自訂記錄檔，但支援自訂記錄。

讓Java記錄檔可在AEMas a Cloud Service中使用(透過 [Cloud Manager](#cloud-manager) 或 [Adobe I/OCLI](#aio))，則必須編寫自訂記錄陳述式 `error.log`. 寫入自訂命名記錄的記錄，例如 `example.log`，無法從AEM as a Cloud Service存取。

記錄檔可寫入 `error.log` 在應用程式的 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 檔案。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM製作和發佈服務記錄檔

AEM製作和發佈服務都提供AEM執行階段伺服器記錄：

+ `aemerror` 是Java錯誤日誌(在 `/crx-quickstart/logs/error.log` (在AEM SDK本機快速入門)。 以下是 [建議的記錄層級](#log-levels) 針對每種環境類型的自訂記錄器：
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemaccess` 列出對AEM服務的HTTP要求及詳細資訊
+ `aemrequest` 列出對AEM服務提出的HTTP要求及其對應的HTTP回應

## AEM Publish Dispatcher記錄檔

只有AEM Publish Dispatcher會提供Apache Web伺服器和Dispatcher記錄檔，因為這些方面僅存在於AEM Publish層級中，而不存在於AEM Author層級中。

+ `httpdaccess` 列出對AEM服務的Apache Web伺服器/Dispatcher提出的HTTP要求。
+ `httperror`  列出來自Apache Web伺服器的日誌消息，並幫助調試支援的Apache模組，如 `mod_rewrite`.
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemdispatcher` 列出來自Dispatcher模組的記錄訊息，包括從快取訊息篩選及提供服務。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`

## Cloud Manager{#cloud-manager}

AdobeCloud Manager允許透過環境的「下載記錄」動作，以日為單位下載記錄。

![Cloud Manager — 下載記錄檔](./assets/logs/download-logs.png)

這些記錄檔可透過任何記錄檔分析工具進行下載和檢查。

## Adobe I/OCLI與Cloud Manager外掛程式{#aio}

AdobeCloud Manager支援透過存取AEMas a Cloud Service記錄 [Adobe I/OCLI](https://github.com/adobe/aio-cli) 和 [適用於Adobe I/OCLI的Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager).

首先， [使用Cloud Manager外掛程式設定Adobe I/O](../../local-development-environment/development-tools.md#aio-cli).

確認已識別相關方案ID和環境ID，並使用 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) 列出用於 [尾部](#aio-cli-tail-logs) 或 [下載](#aio-cli-download-logs) 記錄檔。

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

### 追蹤日誌{#aio-cli-tail-logs}

Adobe I/OCLI可讓您使用，從AEMas a Cloud Service即時追蹤記錄 [尾部日誌](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 命令。 追蹤在AEMas a Cloud Service環境上執行動作時，對於觀看即時記錄活動很實用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令列工具，例如 `grep` 可與 `tail-logs` 幫助隔離感興趣的日誌陳述式，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...僅顯示從中產生的記錄陳述式 `com.example.MySlingModel` 或者包含字串。

### 正在下載日誌{#aio-cli-download-logs}

Adobe I/OCLI提供從AEMas a Cloud Service下載記錄的功能，使用 [下載 — 記錄](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))命令。 這可提供與從Cloud Manager網頁UI下載記錄檔相同的結束結果，其差異為 `download-logs` 命令會根據要求的日誌天數，跨天整合日誌。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 了解記錄

AEMas a Cloud Service中的記錄檔有多個Pod將記錄陳述式寫入其中。 由於多個AEM例項會寫入相同的記錄檔，因此在除錯時了解分析和降低雜訊十分重要。 要解釋，以下 `aemerror` 將使用記錄片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id（日期和時間之後的資料點），即可透過Pod或服務內的AEM例項來整理記錄檔，讓追蹤和了解程式碼執行的程式更簡單。

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 建議的日誌級別{#log-levels}

Adobe對每個AEMas a Cloud Service環境記錄層級的一般指引為：

+ 本機開發(AEM SDK): `DEBUG`
+ 開發: `DEBUG`
+ 分段: `WARN`
+ 生產: `ERROR`

為每個環境類型設定最適當的記錄層級是使用AEMas a Cloud Service，記錄層級會保留在程式碼中

+ Java日誌配置在OSGi配置中保持不變
+ Dispatcher專案中的Apache Web伺服器和Dispatcher記錄層級

...因此，需要改變部署。

### 設定Java日誌級別的環境特定變數

若要為每個環境設定靜態的已知Java記錄層級，另一種方法是使用AEM做為Cloud Service [環境特定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) 以參數化記錄層級，允許透過 [Adobe I/OCLI與Cloud Manager外掛程式](#aio-cli).

這需要更新記錄OSGi設定，才能使用環境特定的變數預留位置。 [預設值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 對於記錄層級，應設為「根據」 [Adobe建議](#log-levels). 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

這種做法有其不利之處，必須加以考慮：

+ [允許的環境變數數量有限](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，以及建立變數以管理記錄層級將使用。
+ 環境變數只能透過程式設計方式管理 [Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 或 [Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ 環境變數的變更必須由支援的工具手動重設。 忘記將高流量環境（例如生產）重設為較少的詳細記錄層級，可能會淹沒記錄並影響AEM效能。

_Apache Web伺服器或Dispatcher記錄設定無法使用環境特定變數，因為這些變數並非透過OSGi設定設定。_
