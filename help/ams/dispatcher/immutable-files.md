---
title: AMS Dispatcher唯讀或不可修改的檔案
description: 了解某些檔案為何是唯讀或不可編輯，以及如何進行您想要的功能變更
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---


# AMS中的只讀或不可變檔案

[目錄](./overview.md)

[&lt; — 上一個：常見記錄檔](./common-logs.md)

## 說明

本檔案將說明哪些檔案已鎖定且不會變更，以及如何正確設定所需的組態設定。

當AMS調配系統時，會推出基線配置，使所有功能都可正常運作且安全。  這些是AMS想要確保作為功能與安全性基準的項目。  要完成此操作，某些檔案被標籤為只讀和不可變，以避免您更改它們。

版面不會防止您變更其行為並覆寫您需要的任何變更。  您不會變更這些檔案，而是會覆蓋您自己的檔案，取代原始檔案。

這也可讓您放心，當AMS以最新修正和安全性增強功能修補Dispatcher時，不會變更您的檔案。  接著，您可以繼續從這些改善中獲益，並只採用您想要的變更。
![展示一個保齡球道，球滾著。  球上有一個箭頭，上面寫著。  水溝阻擋器被抬起，上面有不可修改的字。](assets/immutable-files/bowling-file-immutability.png "bowling-file-micutibility")
如上圖所示，不可修改的檔案不會阻止您玩遊戲。  他們只是阻止你傷害你的表演，讓你留在巷子裡。  此方法允許我們使用幾個非常關鍵的功能：

- 自訂會在自己的安全空間中處理
- 自訂變更的覆蓋，鏡射AEM中覆蓋方法的覆蓋
- 修補AMS配置無需更改自定義項即可完成
- 測試基本安裝和自定義配置可同時完成，以幫助識別問題是否是由自定義或其他什麼檔案引起的？


以下是與Dispatcher一起部署的典型檔案清單：

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

若要判斷哪些檔案不可修改，您可以在Dispatcher上執行下列命令以查看：

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

以下是不可修改檔案的範例回應：

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## 如何進行變更

### 變數

變數可讓您進行功能變更，而無須變更設定檔案本身。  設定的某些元素可透過調整變數的值來調整。  可從檔案中反白顯示的範例 `/etc/httpd/conf.d/dispatcher_vhost.conf` 如下所示：

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

請參閱DispatcherLogLevel指令如何具有 `DISP_LOG_LEVEL` 而不是你在那裡看到的正常值。  在程式碼的該節上方，您也會看到變數檔案的include陳述式。  變數檔案 `/etc/httpd/conf.d/variables/ams_default.vars` 是我們下一個想看的地方。  以下是變數檔案的內容：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

如上所示， `DISP_LOG_LEVEL` 變數 `info`.  我們可以調整以追蹤或除錯，或您選擇的數字值/層級。  現在，所有控制日誌級別的設備都會自動調整。

### 覆蓋方法

請了解頂層包含檔案，因為這些將是您進行任何自訂的起點。  首先，我們有一個案例，想要新增要指向此Dispatcher的新網域名稱。  我們將使用的網域範例為we-retail.adobe.com。  首先，我們會將現有的設定檔案複製到新檔案，以便新增變更：

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

我們複製了現有的aem_publish.vhost檔案，因為它已具備讓項目正常運作所需的功能，且我們不想重新發明已強大的開始。  現在，我們編輯新的weretail.vhost檔案，並進行所需的變更。

變更前:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

變更後:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

現在，我們已更新 `ServerName` 和 `ServerAlias` 以符合新網域名稱，以及更新其他階層連結標題。  現在啟用新檔案，讓apache知道可以使用新檔案：

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

現在，Apache webserver知道網域是它應該產生流量的網域，但我們仍需通知Dispatcher模組，它有新的網域名稱需遵守。  首先，我們將建立 `*_vhost.any` 檔案 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 在檔案里，我們要放入我們要遵守的域名：

```
"we-retail.adobe.com"
```

現在，我們需要建立一個新的伺服器陣列檔案，該檔案將使用新的vhost條目檔案，我們將從將一個強啟動檔案複製到自己的新啟動檔案開始。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

讓我們顯示對此伺服器陣列檔案所需的變更

變更前:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

變更後:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

現在，我們已更新伺服器陣列名稱，並將其用於 `/virtualhosts` 伺服器陣列設定的區段。  我們需要啟用此新伺服器陣列檔案，以便它能在執行中的設定中使用：

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

現在，我們只是重新載入Web伺服器服務並使用新域！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請注意，我們只變更了變更所需的片段，並運用基線組態檔隨附的現有包含和程式碼。  我們只需要勾勒出我們需要改變的要素。  讓事情變得簡單多了，而且讓我們能夠維護更少的代碼
</div>

[Next -> Dispatcher健康狀況檢查](./health-check.md)