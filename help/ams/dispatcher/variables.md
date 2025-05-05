---
title: 在您的AEM Dispatcher設定中使用和瞭解變數
description: 瞭解如何在Apache和Dispatcher模組設定檔案中使用變數，以將它們提升到新的境界。
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# 使用和瞭解變數

[目錄](./overview.md)

[&lt; — 上一步：瞭解快取](./understanding-cache.md)

本檔案將說明如何在Apache Web Server和Dispatcher模組設定檔案中運用變數的強大功能。

## 變數

Apache支援變數，而且由於Dispather模組版本4.1.9，因此也支援變數！

我們可以善用這些工具，做許多有用的事，例如：

- 請確定任何特定於環境的內容並非內嵌在設定中，而是會解壓縮的，以確保來自開發環境的設定檔案可在生產環境中使用相同的功能輸出。
- 切換功能並變更AMS提供且不允許您變更的不可變檔案的記錄層級。
- 根據`RUNMODE`和`ENV_TYPE`等變數變更要使用的包含
- 在Apache設定和模組設定之間比對`DocumentRoot`和`VirtualHost`的DNS名稱。

## 使用基線變數

由於AMS基準檔案是唯讀且不可變的，因此有些功能可以切換或開啟，也可以透過編輯它們使用的變數來設定。

### 基線變數

已在檔案`/etc/httpd/conf.d/variables/ootb.vars`中宣告AMS預設變數。  此檔案無法編輯，但存在是為了確保變數沒有null值。  先包含後包含，而非包含`/etc/httpd/conf.d/variables/ams_default.vars`。  您可以編輯該檔案來更改這些變數的值，甚至可以在您自己的檔案中加入相同的變數名稱和值！

以下是檔案`/etc/httpd/conf.d/variables/ams_default.vars`內容的範例：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 範例1 — 強制SSL

`AUHOR_FORCE_SSL`或`PUBLISH_FORCE_SSL`上顯示的變數可設為1，以啟用重寫規則，強制一般使用者在收到http要求時重新導向至https

以下是允許此切換運作的設定檔案語法：

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

如您所見，重寫規則包括的程式碼會重新導向一般使用者瀏覽器，但設定為1的變數會允許使用或不使用檔案

### 範例2 — 記錄層級

變數`DISP_LOG_LEVEL`可用來設定您想讓實際用於執行組態中的記錄層級使用的變數。

以下是ams基準組態檔案中的語法範例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果您需要增加Dispatcher記錄層級，只需將`ams_default.vars`變數`DISP_LOG_LEVEL`更新為您想要的層級。

範例值可以是整數或單字：

| 記錄層級 | 整數值 | Word值 |
| --- | --- | --- |
| 追蹤 | 4 | trace |
| 偵錯 | 3 | 偵錯 |
| 資訊 | 2 | 資訊 |
| 警告 | 1 | 警告 |
| 錯誤 | 0 | 錯誤 |

### 範例3 — 白名單

變數`AUTHOR_WHITELIST_ENABLED`和`PUBLISH_WHITELIST_ENABLED`可設為1以參與重寫規則，這些規則包括允許或禁止根據IP位址的一般使用者流量的規則。  若要在上切換此功能，需要結合建立白名單規則檔案並加以納入。

以下是一些語法範例，說明變數如何啟用包含白名單檔案和白名單檔案的範例

`sample.vhost`：

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`：

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

如您所見，`sample_whitelist.rules`強制執行IP限制，但切換變數可將其納入`sample.vhost`

## 變數的放置位置

### 網頁伺服器啟動引數

AMS會將伺服器/拓撲特定變數放在`/etc/sysconfig/httpd`檔案內的Apache處理序啟動引數中

此檔案有預先定義的變數，如下所示：

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

您無法變更這些設定，但可在設定檔案中妥善運用

>[!NOTE]
>
>因為只有在服務啟動時才會包含這個檔案。  必須重新啟動服務才能取得變更。  這表示重新載入是不夠的，而是需要重新啟動

### 變數檔案(`.vars`)

您的程式碼提供的自訂變數應存在於目錄`/etc/httpd/conf.d/variables/`內的`.vars`檔案中

這些檔案可以有您想要的任何自訂變數，以下範例檔案中可以看到一些語法範例

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`：

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`：

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`：

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

建立您自己的變數時，檔案會根據變數的內容來命名，並遵循手冊[此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html?lang=zh-Hant#naming-convention)提供的命名標準。  在上述範例中，您可以看到變數檔案裝載不同的DNS專案，做為設定檔案中使用的變數。

## 使用變數

現在您已定義變數檔案中的變數，您將想知道如何在其他設定檔案中正確使用變數。

我們將使用以上範例`.vars`檔案來說明正確的使用案例。

我們要將所有基於環境的變數納入全域，我們將建立檔案`/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我們知道，當httpd服務啟動時，它會提取AMS在`/etc/sysconfig/httpd`中設定的變數，並具有變數集`ENV_TYPE`和`RUNMODE`

當這個全域`.conf`檔案被提取時，將會提早提取，因為`conf.d`中的檔案包含順序是字母數字載入順序，檔案名稱中的平均值000將確保它先於目錄中的其他檔案載入。

include陳述式也在檔案名稱中使用變數。  這可以根據`ENV_TYPE`和`RUNMODE`變數中的值來變更它將實際載入的檔案。

如果`ENV_TYPE`值為`dev`，則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

如果`ENV_TYPE`值為`stage`，則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

如果`RUNMODE`值為`preview`，則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

包含該檔案後，我們將可使用儲存在中的變數名稱。

在`/etc/httpd/conf.d/available_vhosts/weretail.vhost`檔案中，我們可以置換僅適用於dev的一般語法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

對於使用變數強大功能以用於dev、stage和prod的較新語法：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在`/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any`檔案中，我們可以置換僅適用於dev的一般語法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

對於使用變數強大功能以用於dev、stage和prod的較新語法：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

這些變數重複使用相當多，以便個人化執行中的設定，而不需要為每個環境部署不同的檔案。  您基本上是使用變數將設定檔案範本化，並包含以變數為基礎的檔案。

## 檢視變數值

有時候在使用變數時，我們必須搜尋以檢視設定檔案中的值。  您可以在伺服器上執行下列命令，以檢視已解析的變數：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

變數在編譯的Apache設定中的外觀如何：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

變數在經過編譯的Dispatcher設定中的外觀如何：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

從命令輸出中，您會看到組態檔案中的變數與編譯輸出的差異。

設定範例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`：

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

現在執行命令以檢視編譯後的輸出

編譯的Apache設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

已編譯的Dispatcher設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一個 — >排清](./disp-flushing.md)
