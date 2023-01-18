---
title: AMS Dispatcher基本檔案配置
description: 了解基本的Apache和Dispatcher檔案配置。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---


# 基本檔案配置

[目錄](./overview.md)

[&lt; — 上一個：什麼是「The Dispatcher」](./what-is-the-dispatcher.md)

本檔案說明AMS標準設定檔案集，以及此設定標準背後的思路

## 預設的Enterprise Linux資料夾結構

在AMS中，基本安裝使用Enterprise Linux作為基本作業系統。 安裝Apache Webserver時，會設定預設的安裝檔案集。 以下是通過安裝由yum儲存庫提供的基本RPM來安裝的預設檔案

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

在進行安裝設計/結構跟蹤並執行時，我們將獲得以下好處：

- 更輕鬆支援可預測的佈局
- 自動熟悉過去曾使用過Enterprise Linux HTTPD的任何人
- 允許作業系統完全支援的修補週期，無需任何衝突或手動調整
- 避免SELinux違反錯誤標籤的檔案內容

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
Adobe Managed Services伺服器影像通常具有小型作業系統根磁碟。  我們將資料放在一個單獨的卷中，該卷通常裝入「/mnt」中。然後，我們使用該卷，而不是以下預設目錄的預設值

`DocumentRoot`
- 預設:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 預設: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

請記住，新舊目錄將映射回原始裝載點，以消除混淆。
使用單獨的卷並不重要，但值得注意
</div>

## AMS附加元件

AMS將添加到Apache Web Server的基本安裝中。

### 檔案根

AMS預設文檔根：
- 作者:
   - `/mnt/var/www/author/`
- 發佈:
   - `/mnt/var/www/html/`
- 全包和運行狀況檢查維護
   - `/mnt/var/www/default/`

### 暫存和啟用的VirtualHost目錄

以下目錄允許您生成配置檔案，該配置檔案具有可以處理檔案的暫存區域，並且僅在檔案準備就緒時啟用。
- `/etc/httpd/conf.d/available_vhosts/`
   - 此資料夾托管所有名為 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 當您準備好使用 `.vhost` 檔案，您在 `available_vhosts` 資料夾使用相對路徑將它們連結到 `enabled_vhosts` 目錄

### 其他 `conf.d` 目錄

在Apache配置中，還有其他常見的部分，我們建立了子目錄，以允許以乾淨的方式分隔這些檔案，而不是將所有檔案放在一個目錄中

#### 重寫目錄

此目錄可包含 `_rewrite.rules` 您建立的檔案，包含與Apache Web伺服器交互的典型RewriteRulesyntax [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 模組

- `/etc/httpd/conf.d/rewrites/`

#### 白名單目錄

此目錄可包含 `_whitelist.rules` 您建立的檔案包含您的 `IP Allow` 或 `Require IP`與Apache Web伺服器互動的語法 [存取控制](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 變數目錄

此目錄可包含 `.vars` 您建立的檔案包含可在配置檔案中使用的變數

- `/etc/httpd/conf.d/variables/`

### Dispatcher模組特定配置目錄

Apache Web Server非常可擴展，當模組包含許多配置檔案時，最好在安裝基礎目錄下建立自己的配置目錄，而不是將預設目錄雜亂。

我們遵循最佳做法，並建立自己的

#### 模組配置檔案目錄

- `/etc/httpd/conf.dispatcher.d/`

#### 暫存和啟用的伺服器陣列

以下目錄允許您生成配置檔案，該配置檔案具有可以處理檔案的暫存區域，並且僅在檔案準備就緒時啟用。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此資料夾會托管所有 `/myfarm {` 檔案名為 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 準備好使用場檔案時，可用場資料夾內有symlink，它們使用相對路徑連結到enabledfarms目錄

### 其他 `conf.dispatcher.d` 目錄

有其他片段是Dispatcher伺服器陣列檔案設定的子區段，我們建立了子目錄，以允許以清潔的方式分隔這些檔案，而不是將所有檔案放在一個目錄中

#### 快取目錄

此目錄包含 `_cache.any`, `_invalidate.any` 您所建立的檔案，其中包含您要模組如何處理來自AEM的快取元素及失效規則語法的規則。  本節的更多詳細資訊如下 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 客戶端標題目錄

此目錄可包含 `_clientheaders.any` 您建立的檔案，其中包含當請求傳入時，您要傳遞至AEM的用戶端標題清單。  本節的更多詳細資訊包括 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 篩選器目錄

此目錄可包含 `_filters.any` 您建立的檔案，包含您要封鎖或允許流量透過Dispatcher到達AEM的所有篩選規則

- `/etc/httpd/conf.dispatcher.d/filters/`

#### 呈現目錄

此目錄可包含 `_renders.any` 您建立的檔案，其中包含與每個後端伺服器的連線詳細資訊，而Dispatcher會使用來自

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目錄

此目錄可包含 `_vhosts.any` 您建立的檔案，包含與特定伺服器場匹配的域名和路徑清單

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完整的資料夾結構

AMS已使用自訂的副檔名來建構每個檔案，以避免命名空間問題/衝突和任何混淆。

以下是來自AMS預設部署的標準檔案集的範例：

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

## 保持理想

Enterprise Linux具有Apache Webserver包(httpd)的修補週期。

更改的預設檔案安裝較少，原因是，如果通過RPM / Yum命令應用了任何修補的安全修正或配置改進，則不會對更改的檔案頂部應用修正。

而是會建立 `.rpmnew` 檔案。  這表示您會遺漏一些您可能想要的變更，並在設定資料夾中建立更多垃圾。

即，在更新安裝期間，RPM將查看 `httpd.conf` 如果在 `unaltered` 會 *replace* 檔案上會有重要更新。  若 `httpd.conf` was `altered` 然後 *不會取代* 檔案，而是會建立名為 `httpd.conf.rpmnew` 而許多需要的修正會位於不適用於服務啟動的檔案中。

Enterprise Linux已正確設定，以更好地處理此使用案例。  它們為您提供可延伸或覆寫它們為您設定的預設值的區域。  在httpd的基礎安裝內，您會找到該檔案 `/etc/httpd/conf/httpd.conf`，其語法如下：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

其理念是，Apache希望您在將新檔案新增至 `/etc/httpd/conf.d/` 和 `/etc/httpd/conf.modules.d/` 副檔名為的目錄 `.conf`

將Dispatcher模組新增至Apache時，您會建立模組，這是最理想的範例 `.so` 檔案 ` /etc/httpd/modules/` 然後將檔案新增至 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 包含要載入模組的內容 `.so` 檔案

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
我們未修改Apache提供的任何現有檔案。  而是把我們的目錄添加到他們打算去的目錄中。
</div><br/>

現在，我們會在檔案中取用模組 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 初始化模組並載入初始模組特定配置檔案

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

您會再次注意到，我們已新增檔案和模組，但未變更任何原始檔案。  這為我們提供了所需的功能，並保護我們不丟失所需的修補程式修正，同時使軟體包的每次升級都保持最高級別的相容性。

[Next ->配置檔案的說明](./explanation-config-files.md)