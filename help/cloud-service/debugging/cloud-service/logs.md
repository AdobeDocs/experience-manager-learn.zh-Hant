---
title: 記錄檔
description: 記錄檔是AEM中除錯AEM應用程式的前沿，但需視部署的AEM應用程式中的適當登入而定。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
translation-type: tm+mt
source-git-commit: 7fd232d6821f91c342dd04fcdd04b9b505cb7250
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 1%

---


# 使用記錄將AEM除錯為雲端服務

記錄檔是AEM中除錯AEM應用程式的前沿，但需視部署的AEM應用程式中的適當登入而定。

特定環境的AEM服務（作者、發佈／發佈調度程式）的所有記錄檔活動都會整合在單一記錄檔中，即使該服務中的不同pod會產生記錄檔陳述式亦然。

Pod Id會提供在每個log陳述式中，並允許篩選或整合log陳述式。 Pod Id的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 範例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自訂記錄檔

AEM做為Cloud Services不支援自訂記錄檔，但支援自訂記錄。

若要在AEM中以雲端服務形式提供Java記錄檔（透過[Cloud Manager](#cloud-manager)或[Adobe I/O CLI](#aio)），自訂記錄檔必須寫入`error.log`。 寫入自訂命名記錄檔（例如`example.log`）的記錄檔將無法透過AEM以雲端服務方式存取。

## AEM Author和Publish服務記錄檔

AEM Author和Publish服務都提供AEM執行階段伺服器記錄：

+ `aemerror` 是Java錯誤記錄檔(可在AEM SDK本 `/crx-quickstart/error.log` 機快速入門中找到)。以下是[針對每種環境類型的自定義日誌程式建議的日誌級別](#log-levels):
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemaccess` 列出AEM服務的HTTP要求及詳細資訊
+ `aemrequest` 列出對AEM服務提出的HTTP要求及其對應的HTTP回應

## AEM Publish Dispatcher記錄檔

只有AEM Publish Dispatcher才提供Apache網頁伺服器和Dispatcher記錄檔，因為這些方面僅存在於AEM Publish層，而不存在於AEM Author層。

+ `httpdaccess` 列出對AEM服務的Apache Web Server/Dispatcher發出的HTTP請求。
+ `httperror`  列出來自Apache Web伺服器的日誌消息，以及調試支援的Apache模組（如）的幫助 `mod_rewrite`。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`
+ `aemdispatcher` 列出來自Dispatcher模組的日誌消息，包括從快取消息中過濾和服務。
   + 開發: `DEBUG`
   + 分段: `WARN`
   + 生產: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager允許透過環境的「下載記錄檔」動作，按日下載記錄檔。

![Cloud Manager —— 下載記錄檔](./assets/logs/download-logs.png)

這些記錄檔可透過任何記錄檔分析工具進行下載和檢查。

## Adobe I/O CLI with Cloud Manager plugin{#aio}

Adobe Cloud Manager支援透過[Adobe I/O CLI](https://github.com/adobe/aio-cli)以Adobe I/O CLI[ Cloud Manager外掛程式，以雲端服務記錄檔形式存取AEM。](https://github.com/adobe/aio-cli-plugin-cloudmanager)

首先，[使用Cloud Manager外掛程式設定Adobe I/O。](../../local-development-environment/development-tools.md#aio-cli)

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

Adobe I/O CLI提供使用[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令從AEM即時追蹤雲端服務記錄的功能。 當AEM的「雲端服務」環境執行動作時，追蹤對於監視即時記錄活動非常有用。

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

Adobe I/O CLI提供使用[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)命令從AEM下載雲端服務記錄檔的功能。 這提供與從Cloud Manager Web UI下載日誌相同的最終結果，其差異是`download-logs`命令會根據請求的日誌數天合併日誌。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 瞭解記錄檔

以雲端服務身分登入AEM的Pod有多個將log陳述式寫入其中。 由於多個AEM例項會寫入相同的記錄檔，因此在除錯時，請務必瞭解如何分析並降低雜訊。 要解釋，將使用以下`aemerror`日誌代碼段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id（日期和時間之後的資料點），您就可以透過Pod或服務中的AEM例項來對記錄檔進行整理，讓追蹤和瞭解程式碼執行變得更輕鬆。

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

Adobe針對每個AEM做為雲端服務環境之記錄層級的一般指引如下：

+ 本機開發(AEM SDK):`DEBUG`
+ 開發: `DEBUG`
+ 分段: `WARN`
+ 生產: `ERROR`

為每種環境類型設定最適合的記錄層級是將AEM當做雲端服務，記錄層級會維護在程式碼中

+ OSGi配置中維護了Java日誌配置
+ Dispatcher項目中的Apache Web伺服器和Dispatcher日誌級別

...因此，需要進行部署才能改變。

### 用於設定Java日誌級別的環境特定變數

為每個環境設定靜態眾所周知的Java記錄檔層級的替代方法，是使用AEM做為雲端服務的[環境特定變數](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)來參數化記錄檔層級，讓您透過[Adobe I/O CLI及Cloud Manager](#aio-cli)動態變更值。

這要求更新日誌OSGi配置以使用環境特定的變數佔位符。 [記錄](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 層級的預設值應依據 [Adobe建議而設定](#log-levels)。例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

這種方法有其缺點，必須加以考慮：

+ [允許的環境變數數量有限](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，建立用於管理日誌級別的變數將使用一個。
+ 環境變數只能透過[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)或[Cloud Manager HTTP API](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)以程式方式管理。
+ 環境變數的變更必須由支援的工具手動重設。 忘記將高流量環境（例如「生產」）重設為較少的詳細記錄層級，可能會淹沒記錄檔並影響AEM的效能。

_環境特定變數不適用於Apache Web伺服器或Dispatcher日誌配置，因為這些配置不是通過OSGi配置進行配置的。_