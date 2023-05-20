---
title: AMS Dispatcher運行狀況檢查
description: AMS提供運行狀況檢查cgi-bin指令碼，雲負載平衡器將運行該指令碼，AEM以查看是否正常，並應繼續為公共通信服務。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# AMS Dispatcher運行狀況檢查

[目錄](./overview.md)

[&lt; — 上一個：只讀檔案](./immutable-files.md)

安裝AMS基線調度程式時，會附帶一些免費贈品。  其中一項功能是一組運行狀況檢查指令碼。
這些指令碼使堆棧前面的負載平衡AEM器能夠知道哪些腿是健康的，並使其保持服務。

![顯示流量的動畫GIF](assets/load-balancer-healthcheck/health-check.gif "運行狀況檢查步驟")

## 基本負載平衡器運行狀況檢查

當客戶流量通過Internet到達您的實AEM例時，他們將通過負載平衡器

![圖顯示通過負載平衡器從Internet到Aem的流量](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "負載平衡器流量流")

通過負載平衡器發出的每個請求將循環到每個實例。  負載平衡器內置了運行狀況檢查機制，以確保它正在將流量發送到正常主機。

預設檢查通常是埠檢查，以查看負載平衡器中的目標伺服器是否正在偵聽埠通信的傳入（即TCP 80和443）

> `Note:` 儘管這種方法有效，但它並沒有真正衡量AEM健康與否。  僅當Dispatcher（Apache Web伺服器）啟動並運行時才test。

## AMS運行狀況檢查

為避免將流量發送到正在運行非健康實例的正常調度程式AEM, AMS建立了一些額外功能來評估腿的健康狀況，而不僅僅是調度程式。

![圖顯示了運行狀況檢查的不同部分](assets/load-balancer-healthcheck/health-check-pieces.png "健康檢查")

健康檢查包括以下部分
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

我們將介紹每件作品的設定及其重要性

### 包AEM

要指示AEM是否正常工作，您需要它執行一些基本頁面編譯並提供頁面服務。  Adobe Managed Services建立了一個包含test頁的基本包。  儲存庫已開啟的頁面test以及資源和頁面模板可以呈現的頁面。

![圖顯示了CRX包管理器中的AMS包](assets/load-balancer-healthcheck/health-check-package.png "健康檢查包")

這是頁面。  它將顯示安裝的儲存庫ID

![影像顯示「AMS攝政」頁](assets/load-balancer-healthcheck/health-check-page.png "健康檢查頁")

> `Note:` 我們確保頁面不可快取。  如果每次只返回快取頁面，它不會檢查實際狀態！

這是我們可以test的輕量端點，看AEM看它正在運行。

### 負載平衡器配置

我們將負載平衡器配置為指向CGI-BIN終結點，而不是使用埠檢查。

![影像顯示AWS負載平衡器運行狀況檢查配置](assets/load-balancer-healthcheck/aws-settings.png "aws-lb設定")

![影像顯示Azure負載平衡器運行狀況檢查配置](assets/load-balancer-healthcheck/azure-settings.png "azure-lb設定")

### Apache運行狀況檢查虛擬主機

#### CGI-BIN虛擬主機 `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

這是 `<VirtualHost>` 使CGI-Bin檔案能夠運行的Apache配置檔案。

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin檔案是可以運行的指令碼。  這可能是易受攻擊的攻擊向量，而AMS使用的這些指令碼不能公開訪問，只有負載平衡器才可以test。


#### 未正常維護的虛擬主機

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

這些檔案被命名 `000_` 作為目的前置詞。  它內部配置為使用與活動站點相同的域名。  本意是當運行狀況檢查檢測到某個後端有問題時啟用此AEM檔案。  然後提供錯誤頁，而不是僅提供無頁的503 HTTP響應代碼。  會偷走正常交通 `.vhost` 檔案，因為它在 `.vhost` 檔案，同時共用 `ServerName` 或 `ServerAlias`。  導致發往特定域的頁面流向不正常的主機，而不是它所通過的正常通信流的預設主機。

運行運行狀況檢查指令碼時，它們註銷其當前運行狀況狀態。  每分鐘一次，伺服器上運行cronjob ，它在日誌中查找不健康的條目。  如果它檢測到作者實AEM例不正常，則會啟用符號連結：

日誌條目：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron拾起錯誤並做出反應：

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

通過在中配置重裝模式設定，可以控製作者或發佈站點是否可以載入此錯誤頁 `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有效選項：
- 作者
   - 這是預設選項。
   - 這將為作者設定一個維護頁面，當它不健康時
- 發佈
   - 此選項將在發佈伺服器不正常時為其設定維護頁面
- 全部
   - 此選項將為作者或發佈者設定維護頁面，如果它們變得不健康，則還會為兩者設定維護頁面
- 無
   - 此選項跳過運行狀況檢查的此功能

當看到 `VirtualHost` 為這些請求進行設定時，您將看到它們載入的文檔與啟用時發出的每個請求的錯誤頁面相同：

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

響應代碼仍為 `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

而不是空白的頁面，他們將得到這個頁面。

![影像顯示預設維護頁](assets/load-balancer-healthcheck/unhealthy-page.png "不健康頁面")

### CGI-Bin指令碼

CSE可以在負載平衡器設定中配置5個不同的指令碼，這些指令碼在將Dispatcher從負載平衡器中拉出時更改行為或條件。

#### /bin/checkauthor

此指令碼在使用時將檢查並記錄它正在前面的所有實例，但僅在 `author` AEM實例不正常

> `Note:` 請記住，如果發佈實AEM例不健康，則調度程式將繼續工作，以允許流量流向作者實AEM例

#### /bin/checkpublish（預設）

此指令碼在使用時將檢查並記錄它正在前面的所有實例，但僅在 `publish` AEM實例不正常

> `Note:` 請記住，如果作者實AEM例不健康，調度程式將保持服務狀態，以允許流量流向發佈實AEM例

#### /bin/checkeither

此指令碼在使用時將檢查並記錄它正在前面的所有實例，但僅在 `author` 或 `publisher` AEM實例不正常

> `Note:` 請記住，如果發佈實例AEM或作者實AEM例運行不正常，調度程式將退出服務。  也就是說，如果其中一個人健康，也不會收到流量

#### /bin/checkbooth

此指令碼在使用時將檢查並記錄它正在前面的所有實例，但僅在 `author` 和 `publisher` AEM實例不正常

> `Note:` 請記住，如果發佈實AEM例或作AEM者實例不健康，調度程式不會退出服務。  也就是說，如果其中一個不健康，它將繼續接收流量，並給請求資源的人帶來錯誤。

#### /bin/健康

使用此指令碼時，將檢查並記錄它正在前面的所有實例，但無論是否返回錯誤，AEM都只會恢復正常。

> `Note:` 當運行狀況檢查未按需要運行並允許覆蓋將實例保留在負載平衡器AEM中時，將使用此指令碼。

[下一個 — > GIT符號連結](./git-symlinks.md)
