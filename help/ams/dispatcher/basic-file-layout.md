---
title: AMS Dispatcher基本檔案配置
description: 瞭解基本Apache和Dispatcher檔案配置。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 373
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 0%

---

# 基本檔案配置

[目錄](./overview.md)

[&lt; — 上一頁：「Dispatcher」是什麼](./what-is-the-dispatcher.md)

本檔案說明AMS標準組態檔案集以及此組態標準背後的想法

## 預設Enterprise Linux檔案夾結構

在AMS中，基礎安裝使用Enterprise Linux作為基礎作業系統。 安裝Apache Webserver時，會設定預設的安裝檔案。 以下是透過安裝yum儲存區域提供的基本RPM而安裝的預設檔案

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

遵循並遵循安裝設計/結構時，我們將獲得下列優點：

- 更輕鬆支援可預測的版面
- 任何曾經使用Enterprise Linux HTTPD安裝的人都會自動熟悉
- 允許修補作業系統完全支援的週期，不會發生任何衝突或手動調整
- 避免標籤錯誤的檔案前後關聯違反SELinux

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
AdobeManaged Services伺服器影像通常有小型作業系統根磁碟機。  我們將資料放入另一個磁碟區，該磁碟區通常掛載在'/mnt'中。然後我們會使用該磁碟區，而不是下列預設目錄的預設值

`DocumentRoot`
- 預設：`/var/www/html`
- AMS：`/mnt/var/www/html`

`Log Directory`
- 預設： `/var/log/httpd`
- AMS： `/mnt/var/log/httpd`

請記住，舊目錄和新目錄會對應回原始掛載點，以避免混淆。
使用單獨的磁碟區並不重要，但值得注意
</div>

## AMS附加元件

AMS將新增到Apache Web Server的基本安裝上。

### 檔案根目錄

AMS預設檔案根：
- 作者：
   - `/mnt/var/www/author/`
- 發佈：
   - `/mnt/var/www/html/`
- 全面收集與健康情況檢查維護
   - `/mnt/var/www/default/`

### 暫存與啟用的VirtualHost目錄

下列目錄可讓您建置具有臨時區域的組態檔，您可以在檔案上工作，並且只能在準備就緒時啟用。
- `/etc/httpd/conf.d/available_vhosts/`
   - 此資料夾會主控您所有名為的VirtualHost /檔案 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 當您準備好使用時 `.vhost` 檔案，您將 `available_vhosts` 資料夾使用相對路徑將其符號連結 `enabled_vhosts` 目錄

### 其他 `conf.d` 目錄

Apache設定中會有其他常見的片段，而我們建立了子目錄，允許以簡潔的方式分隔這些檔案，而不會將所有檔案集中在一個目錄中

#### 重寫目錄

此目錄可包含所有 `_rewrite.rules` 您建立的檔案包含與Apache Web伺服器互動的典型RewriteRulesyntax [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 模組

- `/etc/httpd/conf.d/rewrites/`

#### 白名單目錄

此目錄可包含所有 `_whitelist.rules` 您建立的檔案，其中包含您的一般 `IP Allow` 或 `Require IP`與Apache Web伺服器互動的語法 [存取控制](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 變數目錄

此目錄可包含所有 `.vars` 您建立的檔案包含可在設定檔案中使用的變數

- `/etc/httpd/conf.d/variables/`

### Dispatcher模組專屬設定目錄

Apache Web Server極具擴充性，當模組有許多組態檔時，最佳實務是在安裝基礎目錄下建立您自己的組態目錄，而不是將預設目錄搞得一團糟。

我們遵循最佳實務，並建立自己的

#### 模組組態檔目錄

- `/etc/httpd/conf.dispatcher.d/`

#### 暫存與啟用的陣列

下列目錄可讓您建置具有臨時區域的組態檔，您可以在檔案上工作，並且只能在準備就緒時啟用。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此資料夾會主控您所有的 `/myfarm {` 已呼叫的檔案 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 當您準備好使用伺服器陣列檔案時，您可以在available_farms資料夾內將其連結，並使用相對路徑連至enabled_farms目錄

### 其他 `conf.dispatcher.d` 目錄

有一些其他片段是Dispatcher伺服器陣列檔案設定的子區段，我們建立了子目錄以允許簡潔的方式分隔這些檔案，而不是將所有檔案放在同一個目錄中

#### 快取目錄

此目錄包含所有 `_cache.any`， `_invalidate.any` 您建立的檔案包含您想要模組如何處理來自AEM的快取元素以及失效規則語法的規則。  如需此章節的詳細資訊，請參閱此處 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 使用者端標頭目錄

此目錄可包含所有 `_clientheaders.any` 您建立的檔案包含您希望在請求傳入時傳遞給AEM的使用者端標題清單。  本節的詳細資訊如下 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 篩選器目錄

此目錄可包含所有 `_filters.any` 您建立的檔案包含要封鎖或允許透過Dispatcher的流量到達AEM的所有篩選規則

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Renders目錄

此目錄可包含所有 `_renders.any` 您建立的檔案，其中包含與Dispatcher將使用其內容的每個後端伺服器的連線詳細資料

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目錄

此目錄可包含所有 `_vhosts.any` 您建立的檔案包含網域名稱和路徑的清單，以符合特定伺服器陣列與特定後端伺服器的需求

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完成資料夾結構

AMS已將每個檔案結構化為自訂副檔名，並旨在避免名稱空間問題/衝突及任何混淆。

以下是來自AMS預設部署的標準檔案集範例：

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

Enterprise Linux具有Apache Webserver套裝程式(httpd)的修補週期。

您變更的預設檔案越少安裝越好，原因在於，如果有任何修補的安全性修正或設定改進是透過RPM / Yum命令套用的，則不會在變更的檔案上方套用修正。

而是會建立 `.rpmnew` 檔案。  這表示您會遺漏某些您可能想要的變更，並在設定資料夾中建立更多記憶體。

即更新安裝期間的RPM將會檢視 `httpd.conf` 如果它位於 `unaltered` 表示它將 *replace* 檔案會讓您取得重要更新。  如果 `httpd.conf` 為 `altered` 然後它 *不會取代* 檔案，而是會建立一個名為的參考檔案 `httpd.conf.rpmnew` 而且該檔案中有許多需要的修正，不適用於服務啟動。

Enterprise Linux已正確設定，以便以更好的方式處理此使用案例。  它們為您提供可以延伸或覆寫其為您設定的預設值的區域。  在httpd的基本安裝內，您會找到檔案 `/etc/httpd/conf/httpd.conf`，而且其語法如下：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

我們的想法是，Apache希望您在將新檔案新增到時擴充模組和設定 `/etc/httpd/conf.d/` 和 `/etc/httpd/conf.modules.d/` 副檔名為 `.conf`

將Dispatcher模組新增到Apache時，您會建立一個模組，這是完美的範例 `.so` 中的檔案 ` /etc/httpd/modules/` 然後在中新增檔案，將其納入 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 包含載入模組的內容 `.so` 檔案

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
我們並未修改Apache提供的任何現有檔案。  相反地，只是將我們的目錄新增到他們原本要前往的目錄中。
</div><br/>

現在我們在檔案中使用模組 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 會初始化模組並載入初始模組專屬設定檔

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

再次強調，您會注意到我們已新增檔案和模組，但未變更任何原始檔案。  這可提供我們所需的功能，並保護我們避免遺失所需的修補程式修正，以及與套件每次升級保持最高相容性等級。

[下一個 — >組態檔說明](./explanation-config-files.md)
