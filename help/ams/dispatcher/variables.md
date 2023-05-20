---
title: 使用和瞭解Dispatcher配置AEM中的變數
description: 瞭解如何在Apache和Dispatcher模組配置檔案中使用變數將其帶到下一級別。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# 使用和瞭解變數

[目錄](./overview.md)

[&lt; — 上一個：瞭解快取](./understanding-cache.md)

本文檔將介紹如何在Apache Web伺服器和Dispatcher模組配置檔案中利用變數的功能。

## 變數

Apache支援變數，自Dispather模組4.1.9版以來，它也支援這些變數！

我們可以利用這些工具做很多有用的事情，比如：

- 確保所有特定於環境的內容不在配置中內聯，而是被解壓，以確保從開發中的配置檔案在具有相同功能輸出的產品中工作。
- 切換不可變檔案AMS提供的功能和更改日誌級別，但不允許您更改。
- 根據變數(如 `RUNMODE` 和 `ENV_TYPE`
- 匹配 `DocumentRoot``s和 `VirtualHost` Apache配置和模組配置之間的DNS名稱。

## 使用基線變數

由於AMS基線檔案是只讀的和不可變的，因此可以切換和開啟這些檔案，並通過編輯它們使用的變數來配置這些功能。

### 基線變數

AMS預設變數在檔案中聲明 `/etc/httpd/conf.d/variables/ootb.vars`。  此檔案不可編輯，但存在以確保變數不具有空值。  先包括後包括，而不包括 `/etc/httpd/conf.d/variables/ams_default.vars`。  您可以編輯該檔案以更改這些變數的值，甚至可以在您自己的檔案中包含相同的變數名稱和值！

下面是檔案內容的示例 `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 示例1 — 強制SSL

上面顯示的變數 `AUHOR_FORCE_SSL`或 `PUBLISH_FORCE_SSL` 可以設定為1以啟用重寫規則，這些規則在http請求進入時強制最終用戶重定向到https

以下是允許此切換工作的配置檔案語法：

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

正如您所看到的，重寫規則包括哪些代碼可以重定向最終用戶瀏覽器，但設定為1的變數允許使用或不使用檔案

### 示例2 — 日誌記錄級別

變數 `DISP_LOG_LEVEL` 可用於設定實際在運行配置中使用的日誌級別的所需內容。

以下是ams基線配置檔案中存在的語法示例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果需要提高Dispatcher日誌記錄級別，只需更新 `ams_default.vars` 變數 `DISP_LOG_LEVEL` 到你想要的水準。

示例值可以是整數或字：

| 記錄層級 | 整數值 | Word值 |
| --- | --- | --- |
| 跟蹤 | 4 | 痕跡 |
| 偵錯 | 3 | 偵錯 |
| 資訊 | 2 | 資訊 |
| 警告 | 1 | 警告 |
| 錯誤 | 0 | 錯誤 |

### 示例3 — 白名單

變數 `AUTHOR_WHITELIST_ENABLED` 和 `PUBLISH_WHITELIST_ENABLED` 可以設定為1以參與重寫規則，這些規則包括允許或禁止基於IP地址的最終用戶通信的規則。  切換此功能需要與建立白名單規則檔案相結合，以便其包含。

下面是一些語法示例，說明變數如何啟用包含白名單檔案和白名單檔案示例

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

如您所見 `sample_whitelist.rules` 強制實施IP限制，但切換變數允許其包含在 `sample.vhost`

## 將變數放在何處

### Web伺服器啟動參數

AMS將將伺服器/拓撲特定變數放在檔案內的Apache進程的啟動參數中 `/etc/sysconfig/httpd`

此檔案具有預定義的變數，如下所示：

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

這些不是您可以更改的，但是在配置檔案中很有用

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

由於此檔案僅在服務啟動時包含。  需要重新啟動服務以提取更改。  表示重新載入是不夠的，而是需要重新啟動
</div>

### 變數檔案(`.vars`)

代碼提供的自定義變數應位於 `.vars` 目錄中的檔案 `/etc/httpd/conf.d/variables/`

這些檔案可以包含您想要的任何自定義變數，在以下示例檔案中可以看到一些語法示例

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

建立自己的變數檔案時，根據變數檔案的內容命名它們，並遵循手冊中提供的命名標準 [這裡](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention)。  在上例中，您可以看到變數檔案將不同的DNS條目作為要在配置檔案中使用的變數進行承載。

## 使用變數

既然已在變數檔案中定義了變數，您就想知道如何在其他配置檔案中正確使用這些變數。

我們舉個例子 `.vars` 檔案，說明正確的使用案例。

我們希望全局包括所有基於環境的變數，我們將建立該檔案 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我們知道，httpd服務啟動時，它會將AMS在 `/etc/sysconfig/httpd` 並且具有 `ENV_TYPE` 和 `RUNMODE`

當此全局 `.conf` 檔案被拉入，因為檔案的包含順序 `conf.d` 是檔案名中的字母數字載入順序均值為000，將確保在目錄中的其他檔案之前載入。

include語句還在檔案名中使用變數。  這可以根據 `ENV_TYPE` 和 `RUNMODE` 變數。

如果 `ENV_TYPE` 值 `dev` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

如果 `ENV_TYPE` 值 `stage` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

如果 `RUNMODE` 值 `preview` 則使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

當該檔案被包括時，它將允許我們使用儲存在其中的變數名稱。

在 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 檔案我們可以替換僅適用於dev的常規語法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

使用使用變數功能用於dev、stage和prod的較新語法：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 檔案我們可以替換僅適用於dev的常規語法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

使用使用變數功能用於dev、stage和prod的較新語法：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

這些變數在個性化運行設定時具有大量的重複使用量，而不必為每個環境部署不同的檔案。  實際上，您可以使用變數來模板化配置檔案，並包括基於變數的檔案。

## 查看變數值

有時，在使用變數時，我們必須進行搜索，以查看配置檔案中的值。  通過在伺服器上運行以下命令，可以查看已解析的變數：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

變數在編譯的Apache配置中的查找方式：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

變數在編譯的Dispatcher配置中的查找方式：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

從命令的輸出中，您將看到配置檔案中變數與已編譯輸出之間的差異。

配置示例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

現在運行命令查看已編譯的輸出

已編譯的Apache配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

已編譯的Dispatcher配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一個 — >法拉盛](./disp-flushing.md)
