---
title: 使用和了解AEM Dispatcher設定中的變數
description: 了解如何在Apache和Dispatcher模組設定檔案中使用變數，將變數帶入下一個層級。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# 使用和了解變數

[目錄](./overview.md)

[&lt; — 上一個：了解快取](./understanding-cache.md)

本檔案將說明如何在Apache Web伺服器和Dispatcher模組組態檔中運用變數的功能。

## 變數

Apache支援變數，且自Dispather模組4.1.9版起，也支援這些變數！

我們可以利用這些功能來執行一大堆有用的工作，例如：

- 請確定環境特定的任何項目不會內嵌在設定中，而是擷取，以確保開發中的設定檔案可在產品中以相同的功能輸出運作。
- 切換AMS提供的不可變檔案的功能並更改日誌級別，不允許您更改。
- 根據變數(如 `RUNMODE` 和 `ENV_TYPE`
- 符合 `DocumentRoot`&#39;s和 `VirtualHost` Apache配置和模組配置之間的DNS名稱。

## 使用基線變數

由於AMS基線檔案是唯讀且不可修改的，因此有些功能可以切換為開啟，也可以編輯其使用的變數來進行設定。

### 基線變數

AMS預設變數會在檔案中宣告 `/etc/httpd/conf.d/variables/ootb.vars`.  此檔案不可編輯，但存在，以確保變數沒有空值。  先包含後包含，而非包含 `/etc/httpd/conf.d/variables/ams_default.vars`.  您可以編輯該檔案以變更這些變數的值，或甚至在您自己的檔案中加入相同的變數名稱和值！

以下是檔案內容的範例 `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 範例1 — 強制SSL

以上所示的變數 `AUHOR_FORCE_SSL`，或 `PUBLISH_FORCE_SSL` 可設為1，以啟用重新寫入規則，強制使用者在透過http要求登入時重新導向至https

以下是可讓此切換運作的設定檔案語法：

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

如您所見，重寫規則包含用來重新導向使用者瀏覽器的程式碼，但設為1的變數則是允許使用或不使用檔案的內容

### 範例2 — 記錄層級

變數 `DISP_LOG_LEVEL` 可用於設定您要擁有的用於執行設定中實際使用的記錄層級。

以下是ams基線組態檔中存在的語法範例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果您需要提高Dispatcher記錄層級，請只更新 `ams_default.vars` 變數 `DISP_LOG_LEVEL` 達到你想要的水準。

範例值可以是整數或字：

| 記錄層級 | 整數值 | 字值 |
| --- | --- | --- |
| Trace | 4 | trace |
| 偵錯 | 3 | 偵錯 |
| 資訊 | 2 | 資訊 |
| 警告 | 1 | 警告 |
| 錯誤 | 0 | 錯誤 |

### 範例3 — 白名單

變數 `AUTHOR_WHITELIST_ENABLED` 和 `PUBLISH_WHITELIST_ENABLED` 可設為1，以參與包含規則的重寫規則，以允許或不允許根據IP位址的一般使用者流量。  開啟此功能需要結合建立白名單規則檔案，以便納入。

以下是變數如何啟用包含白名單檔案和白名單檔案範例的一些語法範例

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

如您所見 `sample_whitelist.rules` 強制IP限制，但切換變數可讓變數包含在 `sample.vhost`

## 變數的放置位置

### Web伺服器啟動參數

AMS會將伺服器/拓撲特定變數放入檔案內Apache程式的啟動引數中 `/etc/sysconfig/httpd`

此檔案已預先定義變數，如下所示：

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

這些不是您可以變更的項目，但在設定檔中很適合運用

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

由於此檔案僅在服務啟動時才包含。  需要重新啟動服務才能取得變更。  這表示重新載入是不夠的，但需要重新啟動
</div>

### 變數檔案(`.vars`)

程式碼提供的自訂變數應存留於 `.vars` 目錄中的檔案 `/etc/httpd/conf.d/variables/`

這些檔案可以有您想要的任何自訂變數，下列範例檔案中也提供一些語法範例

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

建立您自己的變數時，檔案會根據變數的內容命名，並遵循手冊中提供的命名標準 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  在上例中，您可以看到變數檔案包含不同的DNS項目，作為要在設定檔案中使用的變數。

## 使用變數

現在您已在變數檔案中定義變數，您將想知道如何在其他設定檔案中正確使用變數。

我們舉個例子 `.vars` 檔案，以說明正確的使用案例。

我們想要全域納入所有要建立檔案的環境變數 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我們知道httpd服務啟動時，會提取AMS設定的變數(位於 `/etc/sysconfig/httpd` 且變數集為 `ENV_TYPE` 和 `RUNMODE`

當此全局 `.conf` 檔案被提取後，會提早提取，因為檔案的包含順序 `conf.d` 檔案名稱中的英數字元載入順序平均值為000，可確保在載入目錄中的其他檔案之前載入。

include陳述式也在檔案名稱中使用變數。  這可以根據 `ENV_TYPE` 和 `RUNMODE` 變數。

若 `ENV_TYPE` 值為 `dev` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

若 `ENV_TYPE` 值為 `stage` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

若 `RUNMODE` 值為 `preview` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

當該檔案納入時，我們便可使用儲存在其中的變數名稱。

在 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 檔案我們可以交換只適用於開發的一般語法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

使用較新語法，使用變數的功能來用於開發、測試和生產：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 檔案我們可以交換只適用於開發的一般語法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

使用較新語法，使用變數的功能來用於開發、測試和生產：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

這些變數可大量重複使用，無須針對每個環境部署不同的檔案，即可個別化執行設定。  您使用變數來範本化設定檔案，並根據變數包含檔案。

## 檢視變數值

使用變數時，有時必須進行搜尋，才能查看設定檔中的值。  在伺服器上執行下列命令，即可檢視已解析的變數：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

變數在您編譯的Apache設定中的外觀：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

變數在您編譯的Dispatcher設定中的外觀：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

從命令的輸出中，您會看到設定檔案中變數與已編譯輸出的差異。

組態範例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

現在運行命令以查看已編譯的輸出

編譯的Apache設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

編譯的Dispatcher設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一步 — >排清](./disp-flushing.md)