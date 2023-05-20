---
title: AMS Dispatcher基本檔案佈局
description: 瞭解基本的Apache和Dispatcher檔案佈局。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---

# 基本檔案佈局

[目錄](./overview.md)

[&lt; — 上一個：什麼是「調度員」](./what-is-the-dispatcher.md)

本文檔介紹AMS標準配置檔案集以及此配置標準背後的思路

## 預設的Enterprise Linux資料夾結構

在AMS中，基本安裝使用Enterprise Linux作為基本作業系統。 安裝Apache Webserver時，它具有預設安裝檔案集。 以下是通過安裝yum儲存庫提供的基本RPM來安裝的預設檔案

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

在遵循並遵守安裝設計/結構時，我們將獲得以下好處：

- 更易於支援可預測的佈局
- 過去在Enterprise Linux HTTPD安裝上工作過的人自動熟悉
- 允許作業系統完全支援的修補週期，而無需任何衝突或手動調整
- 避免SELinux違反錯誤標籤的檔案上下文

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b>
Adobe Managed Services伺服器映像通常具有小型作業系統根驅動器。  我們將資料放在一個單獨的卷中，該卷通常裝入「/mnt」中。然後，我們使用該卷，而不是以下預設目錄的預設值

`DocumentRoot`
- 預設:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 預設: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

請記住，舊目錄和新目錄將映射回原裝載點，以消除混淆。
使用單獨的卷並不重要，但值得注意
</div>

## AMS附加項

AMS添加到Apache Web Server的基本安裝中。

### 文檔根

AMS預設文檔根：
- 作者:
   - `/mnt/var/www/author/`
- 發佈:
   - `/mnt/var/www/html/`
- Catch-All和Health Check維護
   - `/mnt/var/www/default/`

### 暫存和啟用的VirtualHost目錄

以下目錄允許您生成具有臨時區域的配置檔案，只有在準備好時才啟用這些臨時區域。
- `/etc/httpd/conf.d/available_vhosts/`
   - 此資料夾承載您調用的所有VirtualHost/檔案 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 當您準備使用 `.vhost` 檔案，你在裡面 `available_vhosts` 資料夾使用相對路徑將它們連結到 `enabled_vhosts` 目錄

### 其他 `conf.d` 目錄

Apache配置中有其他常見部分，我們建立了子目錄，以允許以乾淨的方式分隔這些檔案，而不是將所有檔案都放在一個目錄中

#### 重寫目錄

此目錄可以包含 `_rewrite.rules` 您建立的檔案包含與Apache Web伺服器接觸的典型RewriteRulesyntax [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 模組

- `/etc/httpd/conf.d/rewrites/`

#### 白名單目錄

此目錄可以包含 `_whitelist.rules` 您建立的檔案包含您的 `IP Allow` 或 `Require IP`與Apache Web伺服器接觸的語法 [訪問控制](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 變數目錄

此目錄可以包含 `.vars` 您建立的檔案包含您在配置檔案中可以使用的變數

- `/etc/httpd/conf.d/variables/`

### Dispatcher模組特定的配置目錄

Apache Web Server具有非常高的擴展性，當一個模組有大量配置檔案時，最好在安裝基目錄下建立您自己的配置目錄，而不是亂放預設目錄。

我們遵循最佳實踐，創造自己的

#### 模組配置檔案目錄

- `/etc/httpd/conf.dispatcher.d/`

#### 暫存和啟用的場

以下目錄允許您生成具有臨時區域的配置檔案，只有在準備好時才啟用這些臨時區域。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此資料夾承載您的所有 `/myfarm {` 調用的檔案 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 準備好使用場檔案後，在available_farms資料夾內使用相對路徑將它們連結到enabled_farms目錄

### 其他 `conf.dispatcher.d` 目錄

還有其他部分是Dispatcher場檔案配置的子部分，我們建立了子目錄，以允許以乾淨的方式分離這些檔案，而不是將所有檔案都放在一個目錄中

#### 快取目錄

此目錄包含 `_cache.any`。 `_invalidate.any` 您建立的檔案包含您希望模組如何處理來自快取元素的規則以及AEM無效規則語法。  此部分的詳細資訊在此處 [這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 客戶端標頭目錄

此目錄可以包含 `_clientheaders.any` 您建立的檔案包含在請求傳入時要傳AEM遞到的客戶端標頭清單。  本節的更多詳細資訊包括 [這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 篩選器目錄

此目錄可以包含 `_filters.any` 您建立的檔案包含所有要阻止或允許通過Dispatcher的流量到達的篩選器規AEM則

- `/etc/httpd/conf.dispatcher.d/filters/`

#### 呈現目錄

此目錄可以包含 `_renders.any` 您建立的檔案包含與調度程式將從中使用內容的每個後端伺服器的連接詳細資訊

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目錄

此目錄可以包含 `_vhosts.any` 您建立的檔案包含與特定伺服器場與特定後端伺服器匹配的域名和路徑清單

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完成資料夾結構

AMS已使用自定義檔案副檔名對每個檔案進行結構化，以避免命名空間問題/衝突和任何混淆。

以下是AMS預設部署的標準檔案集示例：

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## 保持理想狀態

Enterprise Linux具有Apache Webserver包(httpd)的修補週期。

您更改的預設檔案安裝較少，原因是，如果通過RPM / Yum命令應用了任何修補的安全修復或配置改進，則不會在更改檔案頂部應用修復。

而是建立 `.rpmnew` 檔案。  這意味著您會錯過一些可能希望的更改，並在配置資料夾中建立更多垃圾。

即，在更新安裝過程中， RPM將查看 `httpd.conf` 如果在 `unaltered` 說 *替換* 檔案，你會得到重要的更新。  如果 `httpd.conf` 是 `altered` 然後 *不替換* 檔案，而是建立名為 `httpd.conf.rpmnew` 而許多所需的修復程式將位於該檔案中，該檔案不適用於服務啟動。

已正確設定Enterprise Linux以更好地處理此使用情形。  它們為您提供可擴展或覆蓋它們為您設定的預設值的區域。  在httpd的基本安裝中，您會找到該檔案 `/etc/httpd/conf/httpd.conf`，其中包含的語法如下：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

其思想是，Apache希望您在向Apache中添加新檔案時擴展模組和配置 `/etc/httpd/conf.d/` 和 `/etc/httpd/conf.modules.d/` 檔案副檔名為 `.conf`

作為將Dispatcher模組添加到Apache時的完美示例，您將建立一個模組 `.so` 檔案 ` /etc/httpd/modules/` 然後通過在中添加檔案來包括 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 包含要載入模組的內容 `.so` 檔案

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
我們未修改Apache提供的任何現有檔案。  而是把我們添加到他們本該去的目錄中。
</div><br/>

現在，我們在檔案中使用我們的模組 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 初始化模組並載入初始模組特定的配置檔案

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

您會再次注意到，我們添加了檔案和模組，但未更改任何原始檔案。  這為我們提供了所需的功能，並保護我們不會丟失所需的修補程式修復程式，同時保持與軟體包每次升級的最高級別相容性。

[下一步 — >配置檔案說明](./explanation-config-files.md)
