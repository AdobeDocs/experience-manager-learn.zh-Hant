---
title: AMS Dispatcher只讀或不可變檔案
description: 瞭解為什麼某些檔案是只讀的或不可編輯的，以及如何進行所需的功能更改
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---

# AMS中的只讀或不可變檔案

[目錄](./overview.md)

[&lt; — 上一個：常見日誌](./common-logs.md)

## 說明

本文檔將介紹哪些檔案已鎖定且不會更改，以及如何正確設定所需的配置設定。

當AMS設定系統時，它們會推出使所有功能都能夠正常工作和安全的基準配置。  這些是AMS希望確保作為功能和安全基準的內容。  要完成此操作，某些檔案被標籤為只讀和不可變，以避免更改它們。

佈局不會阻止您更改其行為並覆蓋您需要的任何更改。  您不會更改這些檔案，而是會覆蓋取代原始檔案的自己檔案。

這還允許您確信，當AMS用最新的修復和安全增強程式修補調度程式時，它們不會更改您的檔案。  然後，您可以繼續從這些改進中獲益，並只採用您想要的更改。
![展示一個保齡球道，球滾過球道。  球有一個箭頭，上面寫著你。  本發明公開了一種水溝阻擋器，該水溝阻擋器被抬起，並且其上具有不可改變的文字檔案。](assets/immutable-files/bowling-file-immutability.png "保齡檔案不可變性")
如上圖所示，不可變檔案不會阻止您玩遊戲。  他們只是阻止你傷害你的表演，讓你留在車道上。  這種方法允許我們有幾個非常關鍵的特徵：

- 在其自己的安全空間中處理自定義
- 自定義更改的覆蓋鏡像中的覆蓋方法AEM
- 可以在不更改自定義項的情況下對AMS配置進行修補
- 可以同時測試基本安裝與自定義配置，以幫助確定問題是否是由自定義或其他什麼檔案引起的？


下面是與Dispatcher一起部署的典型檔案清單：

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

要確定哪些檔案不可變，可以在Dispatcher上運行以下命令以查看：

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

以下是檔案不可變的示例響應：

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

## 如何進行更改

### 變數

變數允許您在不更改配置檔案本身的情況下進行功能更改。  通過調整變數的值可以調整配置的某些元素。  一個示例可以從檔案中突出顯示 `/etc/httpd/conf.d/dispatcher_vhost.conf` 如下所示：

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

請參見DispatcherLogLevel指令的變數 `DISP_LOG_LEVEL` 而不是你在那裡看到的正常值。  在該代碼部分上方，您還會看到變數檔案的include語句。  變數檔案 `/etc/httpd/conf.d/variables/ams_default.vars` 是我們下一個想看的地方。  以下是該變數檔案的內容：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

如上所示， `DISP_LOG_LEVEL` 變數 `info`。  我們可以調整此值以跟蹤或調試，或調整您選擇的數字值/級別。  現在，控制日誌級別的所有位置都將自動調整。

### 覆蓋方法

請瞭解頂層包含檔案，因為這些檔案將是您進行任何自定義的起始位置。  首先，我們有一個簡單的示例，我們希望添加一個新的域名，並指向此Dispatcher。  我們將使用的域示例是we-retail.adobe.com。  首先，我們將將現有配置檔案複製到新配置檔案中，在此檔案中可以添加更改：

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

我們複製了現有的aem_publish.vhost檔案，因為它已經具備了我們使項目正常運行所需的內容，而且我們不想重新創造一個已經很強的開始。  現在，我們編輯新的weretail.vhost檔案並進行所需的更改。

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

現在我們已更新 `ServerName` 和 `ServerAlias` 匹配新域名，以及更新其他breadcrumb標頭。  現在，讓我們啟用新檔案，以便apache知道使用新檔案：

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

現在，Apache webserver知道域是它應該為其提供流量的域，但我們仍需要通知Dispatcher模組它有一個新的域名來維護。  我們首先建立 `*_vhost.any` 檔案 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 在該檔案中，我們將輸入要遵守的域名：

```
"we-retail.adobe.com"
```

現在，我們需要建立一個新的場檔案，該檔案將使用我們的新vhost條目檔案，我們將從將一個強啟動檔案複製到我們自己的新啟動檔案開始。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

顯示需要對此場檔案進行的更改

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

現在，我們已更新了伺服器場名稱，並將其用於 `/virtualhosts` 的子目錄中。  我們需要啟用此新場檔案，以便它可以在運行配置中使用它：

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

現在，我們只需重新載入Web伺服器服務並使用新域！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請注意，我們只更改了更改所需的部分，並利用了基線配置檔案附帶的現有包括和代碼。  我們只需要勾勒出我們需要改變的要素。  使事情更輕鬆，並且允許我們維護較少的代碼
</div>

[下一步 — > Dispatcher運行狀況檢查](./health-check.md)
