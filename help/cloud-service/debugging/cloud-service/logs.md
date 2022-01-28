---
title: 記錄檔
description: 日誌在as a Cloud Service中充當調試應AEM用程AEM序的前線，但取決於在已部署的應用程式中進行充分的記AEM錄。
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
source-git-commit: 2685f2553349d6f0b48e03f2ed24dcea7ad9ac70
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 1%

---

# 使用日AEM志調試as a Cloud Service

日誌在as a Cloud Service中充當調試應AEM用程AEM序的前線，但取決於在已部署的應用程式中進行充分的記AEM錄。

給定環境服務(作者、發佈/發佈調度程式AEM)的所有日誌活動都合併到單個日誌檔案中，即使該服務中的不同資料夾生成日誌語句也是如此。

Pod Id在每條log語句中提供，允許過濾或整理log語句。 Pod ID的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 範例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自定義日誌檔案

由AEM於Cloud Services不支援自定義日誌檔案，但它支援自定義日誌。

使Java日誌在as a Cloud Service中AEM可用(通過 [雲管理器](#cloud-manager) 或 [Adobe I/OCLI](#aio))，必須編寫自定義日誌語句 `error.log`。 寫入自定義命名日誌的日誌，如 `example.log`，無法從AEMas a Cloud Service訪問。

日誌可寫入 `error.log` 在應用程式的 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 的子菜單。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM作者和發佈服務日誌

AEM Author和Publish服務都提供運行時服AEM務器日誌：

+ `aemerror` 是Java錯誤日誌(位於 `/crx-quickstart/logs/error.log` )AEM的正文。 以下是 [建議的日誌級別](#log-levels) 對於按環境類型的自定義記錄器：
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemaccess` 列出對服務的HTTP請AEM求及詳細資訊
+ `aemrequest` 列出對服務的HTTP請AEM求及其相應的HTTP響應

## AEM發佈調度程式日誌

只有AEM Publish Dispatcher提供Apache Web伺服器和Dispatcher日誌，因為這些方面僅存在於AEM Publish層中，而不存在於AEM Author層中。

+ `httpdaccess` 列出對服務的AEMApache Web伺服器/Dispatcher發出的HTTP請求。
+ `httperror`  列出來自Apache Web伺服器的日誌消息，並幫助調試支援的Apache模組，如 `mod_rewrite`。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemdispatcher` 列出Dispatcher模組中的日誌消息，包括從快取消息中篩選和提供服務。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe雲管理器允許通過環境的「下載日誌」操作按天下載日誌。

![雲管理器 — 下載日誌](./assets/logs/download-logs.png)

這些日誌可以通過任何日誌分析工具下載和檢查。

## Adobe I/OCLI和Cloud Manager插件{#aio}

Adobe雲管理器支援通AEM過以下方式訪問as a Cloud Service日誌 [Adobe I/OCLI](https://github.com/adobe/aio-cli) 和 [用於Adobe I/OCLI的Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

首先， [使用Cloud Manager插件設定Adobe I/O](../../local-development-environment/development-tools.md#aio-cli)。

確保已識別相關的程式ID和環境ID，並使用 [清單可用日誌選項](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) 列出用於 [尾](#aio-cli-tail-logs) 或 [下載](#aio-cli-download-logs) 日誌。

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

Adobe I/OCLI使用CLI從as a Cloud Service即時跟蹤AEM日誌 [尾日誌](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 的子菜單。 跟蹤在as a Cloud Service環境上執行操作時對即時日誌活動AEM非常有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具，如 `grep` 可與 `tail-logs` 幫助隔離感興趣的日誌語句，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...僅顯示從 `com.example.MySlingModel` 或者包含字串。

### 正在下載日誌{#aio-cli-download-logs}

Adobe I/OCLI提供了從as a Cloud Service下載日誌AEM的功能 [下載日誌](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))命令。 這與從Cloud Manager Web UI下載日誌的結果相同，其區別是 `download-logs` 命令根據請求的日誌天數整合日誌。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 瞭解日誌

as a Cloud Service中AEM的日誌有多個pod將日誌語句寫入其中。 因為多AEM個實例寫入同一日誌檔案，所以瞭解在調試時如何分析和降低噪音非常重要。 要解釋，以下 `aemerror` 將使用日誌段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id，即在日期和時間之後的資料點，日誌可以由Pod或服務中的實例AEM進行整理，從而更容易跟蹤和理解代碼執行。

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

Adobe對每個as a Cloud Service環境的日誌級AEM別的一般指導是：

+ 本地開發(AEMSDK): `DEBUG`
+ 開發: `DEBUG`
+ 分段: `WARN`
+ 生產: `ERROR`

為每種環境類型設定最合適的日誌級別時AEMas a Cloud Service，日誌級別在代碼中保持

+ 在OSGi配置中維護Java日誌配置
+ Apache Web伺服器和Dispatcher項目中的Dispatcher日誌級別

...因此，需要部署來改變。

### 用於設定Java日誌級別的環境特定變數

為每個環境設定靜態的已知Java日誌級別的替代方法是AEM用作Cloud Service [環境特定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) 參數化日誌級別，允許通過 [Adobe I/OCLI和Cloud Manager插件](#aio-cli)。

這需要更新記錄OSGi配置以使用環境特定變數佔位符。 [預設值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 對於日誌級別，應設定為 [Adobe建議](#log-levels)。 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

這一方法有其弊端，必須加以考慮：

+ [允許有限數量的環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，並建立用於管理日誌級別的變數將使用一個。
+ 只能通過寫程式方式管理環境變數 [Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 或 [雲管理器HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)。
+ 必須使用支援的工具手動重置對環境變數的更改。 忘記將高流量環境（如生產）重置為較少冗餘的日誌級別可能會淹沒日誌並影響AEM效能。

_特定於環境的變數不適用於Apache Web伺服器或Dispatcher日誌配置，因為這些變數未通過OSGi配置進行配置。_
