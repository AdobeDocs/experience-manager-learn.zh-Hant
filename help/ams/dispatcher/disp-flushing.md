---
title: AEM Dispatcher排清
description: 了解AEM如何讓Dispatcher的舊快取檔案失效。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---


# Dispatcher虛名URL

[目錄](./overview.md)

[&lt; — 上一個：使用和了解變數](./variables.md)

本檔案將說明排清的發生方式，並說明執行快取排清和失效的機制。


## 運作方式

### 操作順序

最好說明內容作者何時啟動頁面，當發佈者收到新內容時，就會觸發排清請求給Dispatcher，如下圖所示：
![作者會啟用內容，而觸發發佈者傳送要排清至Dispatcher的請求](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
事件的這個連結，會反白顯示只有在項目是新項目或已變更時才會排清項目。  這可確保發佈者在清除快取之前已收到內容，以避免競爭條件，因為在從發佈者擷取變更之前可能會發生排清。

## 複寫代理程式

在作者上，有一個復寫代理程式設定為指向發佈者，在啟動某個項目時，它會觸發將檔案及其所有相依性傳送至發佈者。

發佈者收到檔案時，其復寫代理程式已設定為指向Dispatcher，以在接收時觸發事件。  接著會序列化排清請求，並將其張貼至Dispatcher。

### 製作復寫代理

以下是已配置標準複製代理的一些螢幕截圖示例
![AEM網頁上標準復寫代理的螢幕擷圖/etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

製作上通常會針對每個發佈者設定1或2個復寫代理，以復寫內容。

首先是將內容啟動推送至的標準復寫代理。

第二個是反向代理。  此為選用項目，且設定為勾選每個發佈者寄件匣，以查看是否有新內容可以作為反向復寫活動提取至作者中

### 發佈商復寫代理

以下是已配置標準刷新複製代理的示例螢幕截圖
![AEM網頁中標準刷新複製代理的螢幕截圖/etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### DISPATCHER排清復寫接收虛擬主機

Dispatcher模組會尋找特定標題，以知道POST請求是何時要傳遞至AEM轉譯，或是序列化為排清請求，且需要由Dispatcher處理常式本身處理。

以下是顯示這些值之設定頁面的螢幕擷圖：
![主配置螢幕設定頁簽的圖片，序列化類型顯示為Dispatcher刷新](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

預設設定頁面會顯示 `Serialization Type` as `Dispatcher Flush` 並設定錯誤等級

![複製代理的傳輸頁簽螢幕截圖。  這會顯示將排清請求過帳到的URI。  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

在 `Transport` 標籤 `URI` 設為指向將接收排清請求的Dispatcher的IP位址。  路徑 `/dispatcher/invalidate.cache` 不是模組判斷是否為排清的方式，它只是您在存取記錄中看到的明顯端點，以知道它是排清請求。  在 `Extended` 索引標籤我們會逐一查看其中的項目，以確認這是對Dispatcher模組的排清請求。

![複製代理的「擴展」頁簽的螢幕截圖。  記下隨傳送的POST請求傳送的標題，以告知Dispatcher排清](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

此 `HTTP Method` 對於排清請求， `GET` 帶有某些特殊請求標題的請求：
- CQ-Action
   - 這會根據請求使用AEM變數，而值通常為 *啟動或刪除*
- CQ-Handle
   - 這會根據請求使用AEM變數，例如，值通常是所清除項目的完整路徑 `/content/dam/logo.jpg`
- CQ-Path
   - 這會根據請求使用AEM變數，例如，值通常是要清除之項目的完整路徑 `/content/dam`
- 主機
   - 這是 `Host` 標頭被欺騙以鎖定特定 `VirtualHost` 在dispatcher Apache Web伺服器(`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`)。  這是與 `aem_flush.vhost` 檔案的 `ServerName` 或 `ServerAlias`

![標準復寫代理的畫面，顯示從製作發佈內容的復寫事件收到新項目時，具有react和trigger的復寫代理](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

在 `Triggers` 標籤我們會記下我們使用的切換觸發器及其用途

- `Ignore default`
   - 這已啟用，因此復寫代理不會在頁面啟動時觸發。  當製作例項對頁面進行變更時，會觸發排清。  因為這是發佈商，我們不想觸發這類事件。
- `On Receive`
   - 收到新檔案時，我們要觸發刷新。  因此，當作者傳送更新的檔案給我們時，我們會觸發並傳送排清請求給Dispatcher。
- `No Versioning`
   - 我們會檢查此項，以避免發佈者因收到新檔案而產生新版本。  我們只會取代現有的檔案，並依賴作者來追蹤版本，而非發佈者。

現在，如果我們查看典型排清請求的形式 `curl` 命令

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

此排清範例會排清 `/content/dam` 路徑 `.stat` 檔案。

## 此 `.stat` 檔案

沖洗機制在自然界中是簡單的，我們想解釋 `.stat` 在建立快取檔案的文檔根目錄中生成的檔案。

內 `.vhost` 和 `_farm.any` 檔案我們會設定檔案根指令，以指定快取的位置，以及當來自一般使用者的請求傳入時，要從何處儲存/提供檔案。

如果您要在Dispatcher伺服器上執行下列命令，就會開始發現 `.stat` 檔案

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

以下圖表說明當快取中有項目，且Dispatcher模組已傳送和處理排清請求時，此檔案結構的外觀

![混合了內容和日期的statfiles，顯示了stat級別](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### 統計檔案級別

請注意，在每個目錄中， `.stat` 檔案存在。  這表示已發生刷新。  在上述範例中， `statfilelevel` 設定設為 `3` 在對應的伺服器陣列設定檔案中。

此 `statfilelevel` 設定指示模組將遍歷和更新多少個資料夾 `.stat` 檔案。  .stat檔案為空，它只不過是帶有日期戳的檔案名，甚至可以手動建立，但在Dispatcher伺服器的命令列上執行觸控命令。

如果stat檔案級別設定設定太高，則每個刷新請求都會遍歷與stat檔案相接觸的目錄樹。  這可能會成為大型快取樹上的主要效能點擊，並且會影響Dispatcher的整體效能。

將此檔案級別設定得太低，可能導致刷新請求清除的數量超出預期。  這進而會導致快取流失更頻繁，而從快取提供的請求較少，且可能造成效能問題。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

設定 `statfilelevel` 在合理的水準。  查看資料夾結構，並確保其設定為允許精簡刷新，而不必遍歷太多目錄。   在系統效能測試期間測試它，確保它符合您的需求。

支援語言的網站就是很好的例子。  典型內容樹將具有以下目錄

`/content/brand1/en/us/`

在本示例中，使用4的統計檔案級別設定。  這將確保當您排清位於 <b>`us`</b> 不會導致語言資料夾也被刷新的資料夾。
</div>

### 統計檔案時間戳握手

當內容要求進入相同常式時

1. 的時間戳記 `.stat` 會將檔案與請求檔案的時間戳記進行比較
2. 若 `.stat` 檔案比請求的檔案更新，它會刪除快取內容，並從AEM擷取新內容並快取該內容。  然後提供內容
3. 若 `.stat` 檔案比請求的檔案舊，它會知道檔案是最新的，並且可以提供內容。

### 快取握手 — 示例1

在上例中，內容要求 `/content/index.html`

時間 `index.html` 檔案為2019-11-01 @ 6:21PM

最近的時間 `.stat` 檔案為2019-11-01 @ 12:22PM

了解我們在上面閱讀的內容，您可以看到索引檔案比 `.stat` 檔案和檔案會從快取提供給請求該檔案的最終用戶

### 快取握手 — 示例2

在上例中，內容要求 `/content/dam/logo.jpg`

時間 `logo.jpg` 檔案為2019-10-31 @下午1:13

最近的時間 `.stat` 檔案為2019-11-01 @ 12:22PM

如此範例所示，檔案比 `.stat` 檔案和將會移除，且會從AEM中提取新檔案，以在快取中取代，然後再提供給要求使用者。

## 伺服器陣列檔案設定

以下是完整組態選項集的說明檔案： [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache)

我們將重點介紹其中與快取刷新相關的幾項

### 沖洗農場

有兩個關鍵 `document root` 從作者和發佈者流量中快取檔案的目錄。  要使這些目錄具有最新內容，我們需要刷新快取。  這些排清請求不想與您正常的客戶流量伺服器陣列設定糾纏在一起，這些設定可能會拒絕請求或執行不想要的操作。  相反，我們為此任務提供了兩個刷新場：

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

這些伺服器陣列檔案除了刷新文檔根目錄外，不執行任何操作。

```
/publishflushfarm {  
	/virtualhosts {
		"flush"
	}
	/cache {
		/docroot "${PUBLISH_DOCROOT}"
		/statfileslevel "${DEFAULT_STAT_LEVEL}"
		/rules {
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
		}
		/invalidate {
			/0000 {
				/glob "*"
				/type "allow"
			}
		}
		/allowedClients {
			/0000 {
				/glob "*.*.*.*"
				/type "deny"
			}
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
		}
	}
}
```

### 文檔根

此配置項位於伺服器場檔案的以下部分：

```
/myfarm { 
    /cache { 
        /docroot
```

您可以指定要Dispatcher填入並管理為快取目錄的目錄。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
此目錄應與Web伺服器配置為使用的域的Apache文檔根設定匹配。

基於許多原因，讓每個伺服器陣列的巢狀資料夾都存在Apache檔案根的子資料夾，是個糟糕的主意。
</div>

### 統計檔案級別

此配置項位於伺服器場檔案的以下部分：

```
/myfarm { 
    /cache { 
        /statfileslevel
```

此設定會測量深度 `.stat` 需要在刷新請求傳入時生成檔案。

`/statfileslevel` 以下數字設定，文檔根為 `/var/www/html/` 在排清 `/content/dam/brand1/en/us/logo.jpg`

- 0 — 將建立以下統計檔案
   - `/var/www/html/.stat`
- 1 — 將建立以下統計檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 — 將建立以下統計檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 — 將建立以下統計檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 — 將建立以下統計檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 — 將建立以下統計檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，當時間戳記握手發生時，會尋找最接近的 `.stat` 檔案。

有 `.stat` 檔案級別0和stat檔案 `/var/www/html/.stat` 意味著那些生活在 `/var/www/html/content/dam/brand1/en/us/` 會找到 `.stat` 檔案並遍歷5個資料夾，以查找唯一的 `.stat` 檔案，並比較日期。  這表示在某個層級的高處進行一次排清，實際上會使所有快取的項目無效。
</div>

### 允許的失效

此配置項位於伺服器場檔案的以下部分：

```
/myfarm { 
    /cache { 
        /allowedClients {
```

在此配置中，可以放入允許發送刷新請求的IP地址清單。  如果排清請求進入Dispatcher，則必須來自信任的IP。  如果配置錯誤或從不受信任的IP地址發送刷新請求，您將在日誌檔案中看到以下錯誤：

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### 失效規則

此配置項位於伺服器場檔案的以下部分：

```
/myfarm { 
    /cache { 
        /invalidate {
```

這些規則通常會指出哪些檔案可透過排清請求失效。

為避免重要檔案在頁面啟動後失效，您可以讓規則開始播放，指定哪些檔案可以失效，哪些檔案必須手動失效。  以下是僅允許html檔案失效的組態範例集：

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## 測試/疑難排解

當您啟動頁面並取得頁面啟動成功的綠燈時，您應該也會預期您啟動的內容會從快取中清除。

重新整理頁面，看看舊的內容！ 什麼!? 有綠燈了?!

讓我們手動執行幾個排清程式，以便深入了解可能發生的錯誤。  從發佈者殼層使用curl執行下列排清請求：

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

測試排清請求範例

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

一旦您將request命令觸發給Dispatcher，您就會想要查看它在記錄中的完成項目，以及它在 `.stat files`.  追蹤記錄檔，您應會看到下列項目，以確認排清請求已點擊Dispatcher模組

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

現在，我們看到模組已接收並確認排清請求，以了解它對 `.stat` 檔案。  執行下列命令，並在您發出另一次排清時查看時間戳更新：

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

如您從命令中所見，輸出目前的時間戳 `.stat` 檔案

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

現在，如果我們再次運行刷新，您將看到時間戳更新

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

讓我們比較內容時間戳記與 `.stat` 檔案時間戳

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

如果您查看任何時間戳記，您會注意到內容的時間比 `.stat` 檔案，該檔案告知模組從快取中提供檔案，因為它比 `.stat` 檔案。

簡單說明已更新此檔案的時間戳記，但該時間戳記不符合「刷新」或更換的資格。

[下一個 — >虛名URL](./disp-vanity-url.md)