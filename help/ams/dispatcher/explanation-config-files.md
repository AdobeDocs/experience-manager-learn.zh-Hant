---
title: Dispatcher組態檔說明
description: 了解組態檔、命名慣例等。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# 組態檔說明

[目錄](./overview.md)

[&lt; — 上一個：基本檔案配置](./basic-file-layout.md)

本檔案將劃分並說明部署在Adobe Managed Services中布建之標準內建Dispatcher伺服器中的每個設定檔。 其使用、命名慣例等……

## 命名慣例

使用 `Include` 或 `IncludeOptional` 語句。  以消除衝突和混淆的名稱正確命名，有助於 <b>ton</b>. 使用的名稱將描述檔案的應用範圍，使生活更輕鬆。 如果一切都已命名 `.conf` 這真讓人困惑。 我們希望避免檔案和副檔名名稱不佳。  以下是典型AMS設定Dispatcher中使用的不同自訂副檔名和命名慣例的清單。

## 包含在「conf.d/」中的檔案

| 檔案 | 檔案目標 | 說明 |
| ---- | ---------------- | ----------- |
| 檔案名`.conf` | `/etc/httpd/conf.d/` | 預設的Enterprise Linux安裝使用此檔案副檔名和包含資料夾作為覆蓋httpd.conf中聲明的設定的位置，並允許您在Apache中添加全局級別的其他功能。 |
| 檔案名`.vhost` | 已轉移： `/etc/httpd/conf.d/available_vhosts/`<br>活動： `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> .vhost檔案不會複製到enabled_vhosts資料夾中，但使用symlink到available_vhosts/\*.vhost檔案的相對路徑</div></u><br><br> | \*.vhost（虛擬主機）檔案為 `<VirtualHosts>`  以比對主機名稱，並允許Apache以不同規則處理每個網域流量的項目。 從 `.vhost` 檔案，其他檔案，如 `rewrites`, `whitelisting`, `etc` 將會包含。 |
| 檔案名`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 檔案儲存 `mod_rewrite` 明確包含和使用的規則 `vhost` 檔案 |
| 檔案名`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 檔案從內部包含 `*.vhost` 檔案。 它包含IP規則運算式或允許拒絕規則，以允許IP列入白名單。 如果您試圖根據IP地址限制查看虛擬主機，將生成其中一個檔案，並將其從 `*.vhost` 檔案 |

## 包含在「conf.modules.d/」中的檔案

| 檔案 | 檔案目標 | 說明 |
| --- | --- | --- |
| 檔案名`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache模組會從 `*.any` 檔案。 預設的父包含檔案為 `conf.dispatcher.d/dispatcher.any` |
| 檔案名`_farm.any` | 已轉移： `/etc/httpd/conf.dispatcher.d/available_farms/`<br>活動： `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> 這些伺服器陣列檔案不會複製到 `enabled_farms` 資料夾，但使用 `symlinks` 到 `available_farms/*_farm.any` 檔案 </div> <br/>`*_farm.any` 檔案包含在內 `conf.dispatcher.d/dispatcher.any` 檔案。 這些父伺服器陣列檔案的存在用於控制每種呈現或網站類型的模組行為。 檔案是在 `available_farms` 目錄，並使用 `symlink` 進入 `enabled_farms` 目錄。  <br/>它會從 `dispatcher.any` 檔案。<br/><b>基線</b> 伺服器陣列檔案的開頭 `000_` 以確保先載入。<br><b>自訂</b> 伺服器陣列檔案應在之後載入，方法是在 `100_` 來確保正確的包含行為。 |
| 檔案名`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 每個伺服器陣列都有一組規則，可變更應篩選掉的流量，而不是轉譯者。 |
| 檔案名`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 這些檔案是要由blob比對來比對的主機名稱或uri路徑清單，用以判斷要用來提供該請求的轉譯器 |
| 檔案名`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 這些檔案會指定要快取的項目以及不要快取的項目 |
| 檔案名`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 檔案包含在內 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們指定哪些IP位址可傳送排清和失效請求。 |
| 檔案名`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 檔案包含在內 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們指定應將哪些用戶端標題傳遞至每個轉譯器。 |
| 檔案名`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 檔案包含在內 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們會為每個轉譯器指定IP、連接埠和逾時設定。 正確的轉譯器可以是livecycle伺服器或任何AEM系統，Dispatcher可在其中擷取/代理請求 |

## 避免的問題

遵循命名慣例時，您可以避免一些相當容易的錯誤，而這些錯誤可能會造成災難性的結果。  我們將舉幾個例子。

### 問題示例

例如，ExampleCo的網站範例是Dispatcher設定的開發人員建立了兩個設定檔案。

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

若 `vhost` 檔案意外放入 `rewrites` 資料夾和 `rewrites file` 被放入 `vhosts` 檔案夾。  檔案名稱可正確部署，但Apache會擲回 *錯誤* 問題也不會立即顯現。

<b>這通常會變成問題</b>

若 `two files` 下載至 `same` 位置 `overwrite themselves` 或者讓部署過程變得難以區分。

<b>副檔名相同，而且容易自動納入</b>

副檔名相同，且使用Apache將自動包含的副檔名 `auto include` any `.conf` 其中許多檔案是預設資料夾。

<b>這通常會變成問題</b>

如果副檔名為的vhost檔案 `.conf` 被放入 `/etc/httpd/conf.d/` 資料夾會嘗試將其載入至Apache上的記憶體中，但若重寫規則檔案的副檔名為 `.conf` 被放入 `/etc/httpd/conf.d/` 資料夾，則會自動加入並全域套用，造成混淆或不適當的結果。

## 解析度

根據檔案的功能命名檔案，並安全地離開自動包含規則命名空間。

如果是虛擬主機檔案名，則使用 `.vhost` 作為擴充功能。

如果是重寫規則檔案，請將其命名為「站點」`_rewrite.rules` 作為尾碼和擴充功能。 此命名慣例會清楚說明其用於哪個網站，且是一組重寫規則。

如果是IP白名單規則檔案，請將其命名為說明`_whitelist.rules` 作為尾碼和擴充功能。 此命名慣例會提供一些用途的說明，而且是一組IP比對規則。

如果檔案移至不屬於的自動包含目錄，使用這些命名慣例將可避免問題。

例如，將名為的檔案放入 `.rules`, `.any`，或 `.vhost` 在的「自動包含」資料夾中 `/etc/httpd/conf.d/` 不會有任何影響。

如果部署變更請求顯示「請將examplecorewrite.rules部署至生產Dispatcher」，則部署變更的人員已經知道他們不會新增新網站，他們只是在更新重新寫入規則，如檔案名稱所示。

### 包括訂單

在Enterpise Linux上安裝的Apache Webserver中擴充功能和設定時，您有一些您想了解的重要包含訂單

### Apache基線包含

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

如上圖所示， httpd二進位檔只會以其設定檔案的形式查看httpd.conf檔案。  該檔案中包含以下語句：

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS頂級包括

在套用標準時，我們新增了一些額外的檔案類型，並加入了自己的。

以下是AMS基線目錄，最上層包含
![AMS基線包含以dispatcher_vhost.conf開頭的，其中將包含/etc/httpd/conf.d/enabled_vhosts/目錄中任何包含*.vhost的檔案。  /etc/httpd/conf.d/enabled_vhosts/目錄中的項目是指向/etc/httpd/conf.d/available_vhosts/中實際配置檔案的符號連結](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

我們以Apache的基線為基礎，說明AMS如何建立其他資料夾，而頂層包含 `conf.d` 資料夾以及嵌套在 `/etc/httpd/conf.dispatcher.d/`

Apache載入時，會將 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 該檔案將包含二進位檔案 `/etc/httpd/modules/mod_dispatcher.so` 進入了運行狀態。

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

若要在 `<VirtualHost />` 我們會將設定檔案拖放至 `/etc/httpd/conf.d/` 已命名 `dispatcher_vhost.conf` 在此檔案中，您會看到使用設定模組運作所需的基本參數：

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

如您所見，這包括頂層 `dispatcher.any` 檔案，用於從 `/etc/httpd/conf.dispatcher.d/dispatcher.any`

請注意此檔案的內容：

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

頂層 `dispatcher.any` 檔案包含所有位於中已啟用的伺服器陣列檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 檔案名為 `FILENAME_farm.any` 符合我們的標準命名慣例。

稍後於 `dispatcher_vhost.conf` 前面提到的檔案，我們也會執行include陳述式，以啟用中每個啟用的虛擬主機檔案 `/etc/httpd/conf.d/enabled_vhosts/` 檔案名為 `FILENAME.vhost` 符合我們的標準命名慣例。

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

在每個.vhost檔案中，您會注意到Dispatcher模組會初始化為目錄的預設檔案處理常式。  以下是顯示語法的範例.vhost檔案：

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

在頂層包含解決之後，他們還有其他值得一提的子包含。  以下是關於伺服器陣列和主機檔案如何包含其他子元素的高階圖表

### AMS虛擬主機包括

![本圖顯示一個.vhost檔案如何包含變數、白名單和重寫資料夾的檔案](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

若有 `.vhost` 檔案 `/etc/httpd/conf.d/availabled_vhosts/` 目錄被symlinked到 `/etc/httpd/conf.d/enabled_vhosts/` 用於運行配置的目錄。

此 `.vhost` 檔案的子包含基於我們找到的共同片段。  變數、白名單和重寫規則等。

此 `.vhost` 檔案會根據每個檔案需要包含的位置，為每個檔案提供包含陳述式 `.vhost` 檔案。  以下是 `.vhost` 檔案作為良好參考：

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

如上例所示，此設定檔案中需要的變數會包含一個，以供後續使用。

檔案內 `/etc/httpd/conf.d/variables/weretail.vars` 我們可以查看定義的變數：

```
Define MAIN_DOMAIN dev.weretail.com
```

您也可以看到包含 `_whitelist.rules` 根據不同白名單條件限制可檢視此內容之使用者的檔案。  讓您查看其中一個白名單檔案的內容 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

您也可以看到包含一組重寫規則的行。  讓我們來看看 `weretail_rewrite.rules` 檔案：

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS場包括

![<FILENAME>_farms.any將包含子.any檔案以完成場配置。  在此圖中，您可以看到伺服器陣列將包含每個頂層區段檔案快取、剪貼簿、篩選器、轉譯，以及.any檔案](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

FILENAME_farm.any檔案 `/etc/httpd/conf.dispatcher.d/available_farms/` 目錄被symlinked到 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 用於運行配置的目錄。

伺服器陣列檔案的子包含基於 [農場的頂級部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) 如快取、clientheaders、篩選器、轉譯和vhosts。

此 `FILENAME_farm.any` 檔案將根據需要包含在伺服器陣列檔案中的位置，為每個檔案提供include陳述式。  以下是 `FILENAME_farm.any` 檔案作為良好參考：

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

如您所見，weretail伺服器陣列的每個區段都使用include陳述式，而不是使用所有所需語法。

讓我們看看其中幾個包含的語法，以了解每個子包含的外觀

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

如您所見，這是一份以新行分隔的網域名稱清單，應從此伺服器陣列呈現，而非其他伺服器陣列。

接下來，讓我們看 `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[下一步 — >了解快取](./understanding-cache.md)