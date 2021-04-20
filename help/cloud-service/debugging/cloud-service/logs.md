---
title: 記錄檔
description: 日誌是Cloud Service中除錯應AEM用程AEM式的前沿，但取決於在部署的應用程式中登入的AEM充分程度。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '995'
ht-degree: 2%

---


# 使用記AEM錄檔除錯為Cloud Service

日誌是Cloud Service中除錯應AEM用程AEM式的前沿，但取決於在部署的應用程式中登入的AEM充分程度。

給定環境服務（作者、發佈／發佈調度器）的所有日誌活動AEM都會合併到單個日誌檔案中，即使該服務中的不同pod生成了日誌語句也是如此。

Pod Id會提供在每個log陳述式中，並允許篩選或整合log陳述式。 Pod Id的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 範例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自訂記錄檔

由AEM於Cloud Services不支援自訂記錄檔，但支援自訂記錄檔。

對於要以Cloud Service形式AEM(通過[Cloud Manager](#cloud-manager)或[Adobe I/OCLI](#aio))提供的Java日誌，必須在`error.log`中寫入自定義日誌語句。 寫入自定義命名日誌（如`example.log`）的日誌將不能作為Cloud ServiceAEM訪問。

## AEM Author和Publish服務記錄檔

AEM Author和Publish服務都提供執行階AEM段伺服器記錄：

+ `aemerror` 是Java錯誤記錄(可在 `/crx-quickstart/error.log` AEMSDK本機快速入門中找到)。以下是[針對每種環境類型的自定義日誌程式建議的日誌級別](#log-levels):
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemaccess` 列出對服務的HTTP請AEM求和詳細資訊
+ `aemrequest` 列出對服務發出的HTTP請AEM求及其對應的HTTP響應

## AEM Publish Dispatcher記錄檔

只有AEM Publish Dispatcher才提供Apache網頁伺服器和Dispatcher記錄檔，因為這些方面僅存在於AEM Publish層，而不存在於AEM Author層。

+ `httpdaccess` 列出向服務的Apache Web AEM server/Dispatcher發出的HTTP請求。
+ `httperror`  列出來自Apache Web伺服器的日誌消息，以及調試支援的Apache模組（如）的幫助 `mod_rewrite`。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemdispatcher` 列出來自Dispatcher模組的日誌消息，包括從快取消息中過濾和服務。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe雲管理器允許通過環境的「下載日誌」操作按天下載日誌。

![Cloud Manager —— 下載記錄檔](./assets/logs/download-logs.png)

這些記錄檔可透過任何記錄檔分析工具進行下載和檢查。

## Adobe I/OCLI with Cloud Manager plugin{#aio}

AdobeCloud Manager支援通AEM過[Adobe I/OCLI](https://github.com/adobe/aio-cli)作為Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)的[ Cloud Manager插件訪問Cloud Service日誌。

首先，[使用Cloud Manager plugin](../../local-development-environment/development-tools.md#aio-cli)設定Adobe I/O。

確保已識別相關的程式Id和環境Id，並使用[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)列出用於[tail](#aio-cli-tail-logs)或[download](#aio-cli-download-logs)日誌的日誌選項。

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

### 跟蹤日誌{#aio-cli-tail-logs}

Adobe I/OCLI使用[tail-logsAEM](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令從Cloud Service即時跟蹤日誌。 跟蹤對於在作為Cloud Service環境執行操作時監視即時日誌活動AEM非常有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具（例如`grep`）可與`tail-logs`搭配使用，以幫助隔離感興趣的日誌語句，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...僅顯示從`com.example.MySlingModel`生成的日誌語句，或在其中包含該字串。

### 下載日誌{#aio-cli-download-logs}

Adobe I/OCLI使用[download-logsAEM](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)命令從Cloud Service下載日誌。 這提供與從Cloud Manager Web UI下載日誌相同的最終結果，其差異是`download-logs`命令會根據請求的日誌數天合併日誌。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 瞭解記錄檔

以Cloud ServiceAEM身分登入時，會有多個pod將log陳述式寫入其中。 由於多AEM個執行個體會寫入相同的記錄檔，因此在除錯時，請務必瞭解如何分析並降低雜訊。 要解釋，將使用以下`aemerror`日誌代碼段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id（日期和時間之後的資料點），記錄檔可以由Pod或服務中的例項進行整AEM理，讓您更容易追蹤和瞭解程式碼執行。

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

Adobe對每個Cloud Service環境的日誌AEM級別的一般指導是：

+ 本機開AEM發(SDK):`DEBUG`
+ 開發: `DEBUG`
+ 分段: `WARN`
+ 生產: `ERROR`

為每種環境類型設定最合適的日誌級別是作為AEMCloud Service，日誌級別將在代碼中維護

+ OSGi配置中維護了Java日誌配置
+ Dispatcher項目中的Apache Web伺服器和Dispatcher日誌級別

...因此，需要進行部署才能改變。

### 用於設定Java日誌級別的環境特定變數

為每個環境設定靜態眾所周知的Java日誌級別的替代方法是使用AEM[環境特定變數](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)作為Cloud Service的參數化日誌級別，允許通過[Adobe I/OCLI和Cloud Manager插件](#aio-cli)動態更改這些值。

這需要更新日誌OSGi配置以使用環境特定的變數佔位符。 [記錄](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 層級的預設值應依據Adobe建 [議設定](#log-levels)。例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

這種方法有其缺點，必須加以考慮：

+ [允許的環境變數數量有限](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，建立用於管理日誌級別的變數將使用一個。
+ 環境變數只能通過[Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)或[ Cloud Manager HTTP API ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)以寫程式方式管理。
+ 環境變數的變更必須由支援的工具手動重設。 忘記將高流量環境（如「生產」）重設為較少的詳細記錄層級，可能會淹沒記錄並影響AEM效能。

_環境特定變數不適用於Apache Web伺服器或Dispatcher日誌配置，因為這些配置不是通過OSGi配置進行配置的。_