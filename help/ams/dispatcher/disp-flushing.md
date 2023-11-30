---
title: AEM Dispatcher清除
description: 瞭解AEM如何讓Dispatcher中的舊快取檔案失效。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 461873a1-1edf-43a3-b4a3-14134f855d86
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2223'
ht-degree: 0%

---

# Dispatcher虛名URL

[目錄](./overview.md)

[&lt; — 上一步：使用及瞭解變數](./variables.md)

本檔案將提供有關排清發生的方式，並解釋執行快取排清和失效的機制。


## 運作方式

### 作業順序

對典型的工作流程的最佳詮釋是，當內容作者啟動頁面時，發佈者接收到新內容時會向Dispatcher觸發排清請求，如下圖所示：
![作者啟用內容，這會觸發發佈者傳送排清請求給Dispatcher](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
這個事件鏈結強調我們僅在專案是新的或變更時排清專案。  這可確保在清除快取之前，發佈者已收到內容，以避免在發佈者擷取變更之前可能發生排清的競爭條件。

## 複寫代理程式

在作者上，有一個復寫代理程式設定為指向發行者，當有東西啟動時，它會觸發將檔案及其所有相依性傳送給發行者。

當發佈者收到檔案時，會將復寫代理程式設定為指向接收時觸發的Dispatcher。  接著會序列化排清請求，並將其發佈至Dispatcher。

### 作者復寫代理程式

以下是已設定標準復寫代理程式的一些熒幕擷取畫面範例
![AEM網頁/etc/replication.html中的標準復寫代理程式熒幕擷圖](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

在作者上，通常為其復寫內容的每個發行者設定1或2個復寫代理。

第一個是將內容啟用推送到的標準復寫代理。

第二個是反向代理。  這是選擇性的，設定為檢查每個發佈者的寄件匣，以檢視是否有新內容可當作反向復寫活動提取至作者

### 發行者復寫代理程式

以下是已設定的標準排清復寫代理程式的熒幕擷取畫面範例
![AEM網頁/etc/replication.html中的標準排清復寫代理程式熒幕擷圖](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### DISPATCHER FLUSH復寫接收虛擬主機

Dispatcher模組會尋找特定標頭，以瞭解POST請求何時可以傳遞給AEM轉譯器，或者該請求是否已序列化為排清請求，且需要由Dispatcher處理常式本身處理。

以下是顯示這些值的設定頁面熒幕擷圖：
![「序列化型別」顯示為「Dispatcher排清」的主要設定畫面設定索引標籤的圖片](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

預設設定頁面會顯示 `Serialization Type` 作為 `Dispatcher Flush` 並設定錯誤等級

![復寫代理程式之傳輸頁簽的熒幕擷圖。  這會顯示將排清請求發佈到的URI。  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

在 `Transport` 標籤您可以看到 `URI` 設為指向將接收排清請求的Dispatcher的IP位址。  路徑 `/dispatcher/invalidate.cache` 模組無法判斷其是否為排清，這只是您可以在存取記錄檔中看到的明顯端點，用來判斷其為排清請求。  在 `Extended` 索引標籤：我們將詳閱所有專案，確認這是傳送器模組的排清請求。

![復寫代理程式「延伸」索引標籤的熒幕擷圖。  請注意和已傳送的POST請求一起傳送以通知Dispatcher排清的標頭](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

此 `HTTP Method` 對於排清請求只是 `GET` 具有某些特殊請求標頭的請求：
- CQ-Action
   - 這會根據請求使用AEM變數，其值通常為 *啟動或刪除*
- CQ-Handle
   - 這會根據請求使用AEM變數，其值通常是已清除專案的完整路徑，例如 `/content/dam/logo.jpg`
- CQ-Path
   - 這會根據請求來使用AEM變數，例如，值通常是刷新專案的完整路徑 `/content/dam`
- 主機
   - 這就是 `Host` 頁首遭到欺騙，目標為特定 `VirtualHost` 在Dispatcher Apache網頁伺服器上設定的(`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`)。  這是硬式編碼值，符合 `aem_flush.vhost` 檔案的 `ServerName` 或 `ServerAlias`

![標準復寫代理的畫面，顯示當從作者發佈內容收到來自復寫事件的新專案時，復寫代理會作出反應並觸發](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

在 `Triggers` 索引標籤我們會記下我們使用的切換觸發器及其內容

- `Ignore default`
   - 啟用此功能，頁面啟用時就不會觸發復寫代理程式。  這是指當作者執行個體對頁面進行變更時會觸發排清的情況。  因為這是發行者，所以我們不想觸發該型別的事件。
- `On Receive`
   - 收到新檔案時，我們想要觸發排清。  因此，當作者傳送更新的檔案給我們時，我們將觸發並傳送排清請求給Dispatcher。
- `No Versioning`
   - 核取此項以避免發行者因為收到新檔案而產生新版本。  我們將取代現有的檔案，並仰賴作者而非發佈者來追蹤版本。

現在，我們來看看典型的排清請求是什麼樣子 `curl` 命令

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

此排清範例會排清 `/content/dam` 路徑(透過更新 `.stat` 檔案時每小時精細度無法運作的問題。

## 此 `.stat` 檔案

排清機制本質上很簡單，我們要說明 `.stat` 在檔案根目錄中產生的檔案，其中會建立快取檔案。

內部 `.vhost` 和 `_farm.any` 檔案我們設定檔案根指令，以指定當一般使用者的請求傳入時，快取的位置以及儲存/服務檔案的位置。

如果您要在Dispatcher伺服器上執行下列命令，您會開始找到 `.stat` 檔案

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

下圖是當快取中有專案，且Dispatcher模組傳送並處理排清請求時，此檔案結構的外觀

![含有顯示統計層級之內容和日期的statfiles](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### stat檔案層級

請注意，每個目錄中都會有 `.stat` 檔案存在。  這是表示已發生排清的指標。  在上述範例中 `statfilelevel` 設定已設為 `3` 在對應陣列設定檔案中。

此 `statfilelevel` 設定表示模組將周遊並更新的資料夾深度 `.stat` 檔案。  .stat檔案是空的，只不過是帶有datestamp的檔案名稱，甚至可以手動建立，但在Dispatcher伺服器的命令列上執行touch命令。

如果stat檔案層級設定得太高，則每個排清請求都會周遊接觸stat檔案的目錄樹狀結構。  這可能會嚴重影響大型快取樹狀結構的效能，並可能會影響Dispatcher的整體效能。

將此檔案層級設定得太低，可能會導致排清請求清除的內容超出預期。  這進而會造成快取經常流失，使快取中提供的請求變少，進而導致效能問題。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

設定 `statfilelevel` 在合理的層次。  檢視您的資料夾結構，並確定其設定為允許簡潔的排清，而不需周遊太多目錄。   在系統效能測試期間進行測試並確定它符合您的需求。

支援語言的網站就是一個很好的範例。  典型的內容樹狀目錄如下

`/content/brand1/en/us/`

在此範例中，使用stat檔案層級設定4。  這可確保您何時排清位於下的內容 <b>`us`</b> 不會造成語言資料夾被排清的資料夾。
</div>

### STAT檔案時間戳記交握

當對內容的請求進入相同的常式時

1. 的時間戳記 `.stat` 會將檔案與要求的檔案時間戳記進行比較
2. 如果 `.stat` 檔案比要求的檔案新，它會刪除快取的內容並從AEM擷取新內容，然後快取該內容。  然後提供內容
3. 如果 `.stat` 檔案的年齡大於要求的檔案，因此它知道檔案是全新狀態，可以提供內容。

### 快取交握 — 範例1

在上述範例中，請求內容 `/content/index.html`

時間 `index.html` 檔案為2019-11-01 @ 6:21PM

最接近的時間 `.stat` 檔案是2019-11-01 @ 12:22PM

瞭解我們上述所讀內容，您會發現索引檔案比 `.stat` 檔案和檔案會從快取中提供給提出請求的一般使用者

### 快取交握 — 範例2

在上述範例中，請求內容 `/content/dam/logo.jpg`

時間 `logo.jpg` 檔案為2019-10-31 @ 1:13PM

最接近的時間 `.stat` 檔案是2019-11-01 @ 12:22PM

如本範例所示，此檔案的年代早於 `.stat` 檔案和將會移除，而從AEM提取新的檔案會取代快取中的檔案，然後提供給提出請求的一般使用者使用。

## 伺服器陣列檔案設定

完整的設定選項集說明檔案已完整齊全： [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant)

我們將重點介紹其中幾個與快取排清相關的設定

### 排清陣列

有兩個索引鍵 `document root` 將快取作者和發佈者流量中的檔案的目錄。  為了以最新的內容保持這些目錄，我們需要清除快取。  這些排清請求不想與可能拒絕請求或做出不需要之事的正常客戶流量陣列設定糾纏在一起。  我們改為為此任務提供兩個排清陣列：

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

這些伺服器陣列檔案只會排清檔案根目錄，不會執行任何動作。

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

### 檔案根目錄

此設定專案位於伺服器陣列檔案的下列區段：

```
/myfarm { 
    /cache { 
        /docroot
```

您可以指定讓Dispatcher填入並管理的目錄做為快取目錄。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
此目錄應該與網頁伺服器設定要使用的網域的Apache檔案根目錄設定相符。

出於許多原因，在每個陣列中讓巢狀docroot資料夾位於Apache檔案根目錄的子資料夾所在的位置是糟糕的想法。
</div>

### stat檔案層級

此設定專案位於伺服器陣列檔案的下列區段：

```
/myfarm { 
    /cache { 
        /statfileslevel
```

此設定會判斷深度有多深 `.stat` 傳入排清請求時，需要產生檔案。

`/statfileslevel` 在下列數字處設定，檔案根目錄為 `/var/www/html/` 排清時會產生下列結果 `/content/dam/brand1/en/us/logo.jpg`

- 0 — 將建立下列stat檔案
   - `/var/www/html/.stat`
- 1 — 將建立下列stat檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 — 將建立下列stat檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 — 將建立下列stat檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 — 將建立下列stat檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 — 將建立下列stat檔案
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

請記住，發生時間戳記交握時，會尋找最接近的 `.stat` 檔案。

具有 `.stat` 檔案層級0和stat檔案僅限於 `/var/www/html/.stat` 表示存在於底下的內容 `/var/www/html/content/dam/brand1/en/us/` 會尋找最接近的 `.stat` 檔案並遍歷5個資料夾以找出唯一 `.stat` 位於層級0且將日期與其比較的檔案。  這表示在如此高的層級排清實際上會使所有快取的專案失效。
</div>

### 允許失效

此設定專案位於伺服器陣列檔案的下列區段：

```
/myfarm { 
    /cache { 
        /allowedClients {
```

在此設定內部，放置了允許傳送排清請求的IP位址清單。  如果Dispatcher收到排清請求，該請求必須來自受信任的IP。  如果您設定錯誤或從不受信任的IP位址傳送排清請求，會在記錄檔中看到下列錯誤：

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### 無效規則

此設定專案位於伺服器陣列檔案的下列區段：

```
/myfarm { 
    /cache { 
        /invalidate {
```

這些規則通常會指出哪些檔案可透過排清請求失效。

為了避免重要檔案因頁面啟用而失效，您可以設定規則，指定哪些檔案可以失效以及哪些檔案必須手動失效。  以下是僅允許html檔案失效的設定範例：

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## 測試/疑難排解

當您啟動頁面並獲得成功啟動頁面的綠燈時，您應該希望您啟動的內容也會從快取中清除。

您重新整理頁面，就能看到舊內容！ 什麼!? 有綠燈?!

讓我們依照幾個手動步驟進行排清程式，深入瞭解可能出現的問題。  從發佈者Shell使用curl執行以下排清請求：

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

向Dispatcher發出請求命令後，您將會想要在紀錄中檢視它完成的工作以及它使用 `.stat files`.  追蹤記錄檔，您應該會看到下列專案，以確認排清請求已點選Dispatcher模組

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

我們看到模組擷取並確認排清請求，因此需要檢視它如何影響 `.stat` 檔案。  執行以下命令，並在您發出另一個排清時監視時間戳記更新：

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

從命令輸出中可以看到目前的時間戳記 `.stat` 檔案

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

現在，如果我們再次執行排清，您將看到時間戳記更新

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

讓我們將內容的時間戳記與我們的 `.stat` 檔案時間戳記

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

如果您檢視任何時間戳記，您會注意到內容的時間比 `.stat` 此檔案會告訴模組從快取中提供檔案，因為此檔案比 `.stat` 檔案。

簡而言之，更新此檔案的時間戳記並不符合「清除」或取代的資格。

[下一個 — >虛名URL](./disp-vanity-url.md)
