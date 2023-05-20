---
title: Dispatcher配置檔案的說明
description: 瞭解配置檔案、命名約定等。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---

# 配置檔案的說明

[目錄](./overview.md)

[&lt; — 上一個：基本檔案佈局](./basic-file-layout.md)

本文檔將分析並解釋在Adobe Managed Services中預配的標準構建Dispatcher伺服器中部署的每個配置檔案。 它們的使用，命名約定等……

## 命名約定

Apache Web Server在將檔案作為目標時實際上並不關心檔案的檔案副檔名 `Include` 或 `IncludeOptional` 的雙曲餘切值。  用消除衝突和混淆的名稱正確命名它們有助於 <b>噸</b>。 使用的名稱將描述檔案應用的範圍，使生活更輕鬆。 如果所有東西都被命名 `.conf` 這真讓人困惑。 我們希望避免檔案和副檔名名稱不佳。  下面是典型AMS配置的Dispatcher中使用的不同自定義檔案副檔名和命名約定的清單。

## conf.d/中包含的檔案

| 檔案 | 檔案目標 | 說明 |
| ---- | ---------------- | ----------- |
| 檔案名`.conf` | `/etc/httpd/conf.d/` | 預設的Enterprise Linux安裝使用此檔案副檔名並將資料夾作為覆蓋httpd.conf中聲明的設定的位置，並允許您在Apache的全局級別添加其他功能。 |
| 檔案名`.vhost` | 已轉移： `/etc/httpd/conf.d/available_vhosts/`<br>活動： `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b> .vhost檔案不會複製到enabled_vhosts資料夾中，而是使用指向available_vhosts/\*.vhost檔案相對路徑的符號連結</div></u><br><br> | \*.vhost（虛擬主機）檔案 `<VirtualHosts>`  匹配主機名的條目，並允許Apache使用不同的規則處理每個域通信。 從 `.vhost` 檔案，其他檔案 `rewrites`。 `whitelisting`。 `etc` 將包含。 |
| 檔案名`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 檔案儲存 `mod_rewrite` 明確包含和使用的規則 `vhost` 檔案 |
| 檔案名`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 檔案從內部包含 `*.vhost` 的子菜單。 它包含允許IP白名單的IP規則運算式或允許拒絕規則。 如果您試圖限制基於IP地址查看虛擬主機，您將生成其中一個檔案，並從 `*.vhost` 檔案 |

## conf.dispatcher.d/中包含的檔案

| 檔案 | 檔案目標 | 說明 |
| --- | --- | --- |
| 檔案名`.any` | `/etc/httpd/conf.dispatcher.d/` | Dispatcher AEM Apache模組將其設定源自 `*.any` 的子菜單。 預設父包含檔案為 `conf.dispatcher.d/dispatcher.any` |
| 檔案名`_farm.any` | 已轉移： `/etc/httpd/conf.dispatcher.d/available_farms/`<br>活動： `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b> 這些場檔案不會被複製到 `enabled_farms` 資料夾，但使用 `symlinks` 到相對路徑 `available_farms/*_farm.any` 檔案 </div> <br/>`*_farm.any` 檔案包含在 `conf.dispatcher.d/dispatcher.any` 的子菜單。 這些父場檔案用於控制每個呈現或網站類型的模組行為。 檔案是在 `available_farms` 目錄和啟用 `symlink` 到 `enabled_farms` 的子菜單。  <br/>它按名稱自動包括 `dispatcher.any` 的子菜單。<br/><b>基線</b> 場檔案以開頭 `000_` 確保先裝上。<br><b>自定義</b> 應在以下位置啟動場檔案編號方案後載入場檔案： `100_` 來確保行為正確。 |
| 檔案名`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 每個伺服器場都有一組規則，這些規則會更改應過濾掉的流量，而不是將流量傳送給投遞者。 |
| 檔案名`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 這些檔案是要由blob匹配匹配的主機名或URI路徑的清單，用於確定用哪個呈現器來服務該請求 |
| 檔案名`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 檔案從內部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 這些檔案指定哪些項被快取，哪些項未快取 |
| 檔案名`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 它們指定允許發送刷新和無效請求的IP地址。 |
| 檔案名`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 它們指定應將哪些客戶端標頭傳遞到每個呈現器。 |
| 檔案名`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 的子菜單。 它們為每個呈現器指定IP、埠和超時設定。 正確的呈現器可以是生命週期伺服器或調度AEM程式可以從中獲取/代理請求的任何系統 |

## 避免的問題

在遵循命名約定時，您可以避免一些容易出錯的錯誤，這些錯誤可能會導致災難性的結果。  我們將介紹幾個例子。

### 問題示例

作為ExampleCo的站點示例，兩個配置檔案是由Dispatcher配置的開發人員建立的。

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

如果 `vhost` 檔案被意外放入 `rewrites` 資料夾和 `rewrites file` 被放入 `vhosts` 的子菜單。  它似乎按檔案名正確部署，但Apache將 *錯誤* 問題不會馬上顯現。

<b>這通常是一個問題</b>

如果 `two files` 下載到 `same` 他們可以 `overwrite themselves` 或者使部署過程變得無法區分。

<b>檔案副檔名相同且易於自動包含</b>

檔案副檔名與Apache使用的自動包含副檔名相同 `auto include` 任何 `.conf` 許多檔案都是預設資料夾。

<b>這通常是一個問題</b>

如果vhost檔案的副檔名為 `.conf` 在 `/etc/httpd/conf.d/` 將嘗試將其載入到Apache上的記憶體中，但如果重寫規則檔案的副檔名為 `.conf` 被放在 `/etc/httpd/conf.d/` 資料夾，它將獲得自動包含並應用全局，從而導致混淆和不希望的結果。

## 解析度

根據檔案的操作和安全地離開自動包含規則命名空間命名檔案。

如果是虛擬主機檔案名，則 `.vhost` 作為擴展。

如果是重寫規則檔案，請將其命名為站點`_rewrite.rules` 作為尾碼和擴展。 此命名約定將明確其用於哪個站點，並且是一組重寫規則。

如果是IP白名單規則檔案，請將其命名為說明`_whitelist.rules` 作為尾碼和擴展。 此命名約定將提供一些說明，說明它用於什麼，並且是一組IP匹配規則。

如果檔案被移動到不屬於的自動包含目錄，則使用這些命名約定將避免問題。

例如，將名為的檔案 `.rules`。 `.any`或 `.vhost` 在的「自動包含」資料夾中 `/etc/httpd/conf.d/` 不會有任何影響。

如果部署更改請求說明「請將examplecorewrite.rules部署到生產調度程式」，則部署更改的人員已經知道他們沒有添加新站點，他們只是在更新檔案名所指示的重寫規則。

### 包括訂單

在Enterpise Linux上安裝的Apache Webserver中擴展功能和配置時，您需要瞭解一些重要的訂單

### Apache基線包括

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

如上圖所示，httpd二進位檔案只將httpd.conf檔案作為配置檔案進行查找。  該檔案中包含以下語句：

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS頂層包括

在應用我們的標準時，我們添加了一些附加的檔案類型並包括我們自己的檔案類型。

這是AMS基線目錄和頂層包括
![AMS基線包括以dispatcher_vhost.conf開頭，該dispatcher_vhost.conf將包括/etc/httpd/conf.d/enabled_vhosts/目錄中*.vhost中的任何檔案。  /etc/httpd/conf.d/enabled_vhosts/目錄中的項是指向/etc/httpd/conf.d/available_vhosts/中實際配置檔案的符號連結](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Include")

根據Apache的基線，我們將顯示AMS如何建立某些附加資料夾和頂級包括 `conf.d` 資料夾以及嵌套在 `/etc/httpd/conf.dispatcher.d/`

當Apache載入時，它將 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 該檔案將包含二進位檔案 `/etc/httpd/modules/mod_dispatcher.so` 進入運行狀態。

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

在我們的 `<VirtualHost />` 我們將配置檔案放入 `/etc/httpd/conf.d/` 命名 `dispatcher_vhost.conf` 在此檔案中，您將看到使用設定模組工作所需的基本參數：

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

如您所見，這包括頂層 `dispatcher.any` 檔案，用於Dispatcher模組從 `/etc/httpd/conf.dispatcher.d/dispatcher.any`

請注意此檔案的內容：

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

頂層 `dispatcher.any` 檔案包括所有位於的已啟用的場檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 檔案名為 `FILENAME_farm.any` 符合我們標準的命名規範。

在 `dispatcher_vhost.conf` 前面提到的檔案我們還執行include語句以啟用位於中的每個已啟用的虛擬主機檔案 `/etc/httpd/conf.d/enabled_vhosts/` 檔案名為 `FILENAME.vhost` 符合我們標準的命名規範。

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

在我們的每個.vhost檔案中，您都會注意到Dispatcher模組被初始化為目錄的預設檔案處理程式。  下面是一個示例.vhost檔案，用於顯示語法：

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

在頂層包含解決之後，他們還有其他子包含值得一提。  下面是關於場和vhosts檔案如何包括其他子元素的高級圖

### AMS虛擬主機包括

![此圖顯示了一個.vhost檔案如何包括變數、白名單和重寫資料夾中的檔案](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Include")

當 `.vhost` 檔案 `/etc/httpd/conf.d/availabled_vhosts/` 目錄被連結到 `/etc/httpd/conf.d/enabled_vhosts/` 用於運行配置的目錄。

的 `.vhost` 根據我們找到的常用項，檔案包含子項。  變數、白名單和重寫規則等。

的 `.vhost` 檔案將根據每個檔案需要包含的位置為每個檔案包含語句 `.vhost` 的子菜單。  以下是 `.vhost` 檔案作為良好引用：

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

如上例所示，此配置檔案中需要的變數在以後使用時包含。

檔案內 `/etc/httpd/conf.d/variables/weretail.vars` 我們可以看到定義了哪些變數：

```
Define MAIN_DOMAIN dev.weretail.com
```

您還可以看到包含 `_whitelist.rules` 根據不同的白名單條件限制誰可以查看此內容的檔案。  讓我們查看其中一個白名單檔案的內容 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

您還可以看到包含一組重寫規則的行。  我們來看一下 `weretail_rewrite.rules` 檔案：

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS場包括

![<FILENAME>_farms.any將包含子.any檔案以完成場配置。  在此圖中，您可以看到伺服器場將包括每個頂級節檔案快取、客戶端引導程式、篩選器、呈現和vhosts .any檔案](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Include")

當任何FILENAME_farm.any檔案來自 `/etc/httpd/conf.dispatcher.d/available_farms/` 目錄被連結到 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 用於運行配置的目錄。

場檔案具有子包含，基於 [農場的頂級部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) 如快取、客戶端引導程式、篩選器、呈現和vhosts。

的 `FILENAME_farm.any` 檔案將根據需要包含在場檔案中的位置為每個檔案包含語句。  以下是 `FILENAME_farm.any` 檔案作為良好引用：

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

正如您可以看到weretail場的每個部分，而不是使用include語句，而是使用所需的所有語法。

讓我們看一下其中幾個包含的語法，以瞭解每個子包含的樣式

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

正如您所看到的，它是一個新行分隔的域名清單，應從此伺服器場呈現到其他伺服器場上。

下面來看 `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[下一步 — >瞭解快取](./understanding-cache.md)
