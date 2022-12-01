---
title: AMS Dispatcher健康狀況檢查
description: AMS提供運作狀況檢查cgi-bin指令碼，雲端負載平衡器會執行此指令碼，以查看AEM是否運作狀況良好，且應持續為公共流量服務。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# AMS Dispatcher健康狀況檢查

[目錄](./overview.md)

[&lt; — 上一個：只讀檔案](./immutable-files.md)

當您已安裝AMS基線Dispatcher時，會隨附一些免費贈品。  其中一項功能是一組運行狀況檢查指令碼。
這些指令碼可讓前面的AEM堆疊負載平衡器得知哪些支腳運作正常，並讓它們持續運作。

![顯示流量的動畫GIF](assets/load-balancer-healthcheck/health-check.gif "運行狀況檢查步驟")

## 基本負載平衡器運行狀況檢查

當客戶流量透過網際網路連線至您的AEM執行個體時，他們會通過負載平衡器

![影像顯示透過負載平衡器從網際網路傳送至aem的流量](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

每個透過負載平衡器提出的請求會將循序循環至每個執行個體。  負載平衡器已內建健康狀態檢查機制，以確保將流量傳送至健康主機。

預設檢查通常是埠檢查，以查看負載平衡器中目標的伺服器是否正在偵聽埠通信量是否開啟（即TCP 80和443）

> `Note:` 雖然這能奏效，但AEM是否健康並沒有真正的指標。  它只會測試Dispatcher（Apache Web伺服器）是否正在運作。

## AMS運行狀況檢查

為了避免將流量傳送至處於不健康AEM例項前端的健康Dispatcher,AMS建立了一些額外項目來評估腿部的健康狀況，而不只是Dispatcher。

![影像顯示健康檢查的不同部分](assets/load-balancer-healthcheck/health-check-pieces.png "健康檢查")

健康檢查包括以下部分
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

我們將介紹每件作品的設定及其重要性

### AEM套件

若要指出AEM是否正常運作，您需要它執行一些基本頁面編譯並提供頁面。  Adobe Managed Services建立了包含測試頁面的基本套件。  頁面會測試存放庫是否已開啟，以及是否可呈現資源和頁面範本。

![影像顯示CRX套件管理器中的AMS套件](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")

頁面如下。  它會顯示安裝的存放庫ID

![該圖顯示了「AMS攝政」頁](assets/load-balancer-healthcheck/health-check-page.png "健康檢查頁")

> `Note:` 我們確定頁面無法快取。  如果每次只傳回快取頁面，就不會檢查實際狀態！

這是我們可測試的輕量端點，以查看AEM是否正常運作。

### 負載平衡器配置

我們將負載平衡器配置為指向CGI-BIN端點，而不是使用埠檢查。

![影像顯示了AWS負載平衡器運行狀況檢查配置](assets/load-balancer-healthcheck/aws-settings.png "aws-lb設定")

![影像顯示Azure負載平衡器運行狀況檢查配置](assets/load-balancer-healthcheck/azure-settings.png "azure-lb設定")

### Apache健康狀況檢查虛擬主機

#### CGI-BIN虛擬主機 `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

這是 `<VirtualHost>` 允許運行CGI-Bin檔案的Apache配置檔案。

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin檔案是可運行的指令碼。  這可能是易受攻擊的攻擊向量，而AMS使用的這些指令碼不僅可供負載平衡器測試才能公開存取。


#### 維護不良的虛擬主機

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

這些檔案的名稱為 `000_` 作為首碼。  其內部設定會使用與即時網站相同的網域名稱。  意圖是當執行狀況檢查偵測到其中一個AEM後端發生問題時，啟用此檔案。  然後提供錯誤頁面，而不只是沒有頁面的503 HTTP回應代碼。  會偷走正常交通 `.vhost` 檔案，因為它是在 `.vhost` 檔案時共用 `ServerName` 或 `ServerAlias`.  導致以特定網域為目的地的頁面移至不健康的主機，而非其正常流量流經的預設主機。

運行運行狀況檢查指令碼時，它們註銷其當前運行狀況狀態。  每分鐘一次，伺服器上運行一個cronjob，在日誌中查找不健康的條目。  如果它偵測到製作AEM例項不正常，則會啟用symlink:

記錄項目：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron提取錯誤並做出反應：

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

您可以借由在 `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有效選項：
- 作者
   - 這是預設選項。
   - 當作者不健康時，這會為作者設定維護頁面
- 發佈
   - 此選項會為發佈者設定維護頁面，當它不正常時
- 全部
   - 此選項會為作者或發佈者（或如果兩者不健康）設定維護頁面
- 無
   - 此選項會略過健康狀態檢查的此功能

查看 `VirtualHost` 為這些設定，您會看到它們為每個啟用請求時收到的請求，載入相同的檔案作為錯誤頁面：

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

回應代碼仍為 `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

他們會改用此頁面，而非空白頁面。

![影像顯示預設維護頁面](assets/load-balancer-healthcheck/unhealthy-page.png "不健康頁面")

### CGI-Bin指令碼

CSE可在負載平衡器設定中設定5個不同的指令碼，以變更將Dispatcher從負載平衡器中拉出的行為或條件。

#### /bin/checkauthor

使用時，此指令碼會檢查並記錄其前端的任何執行個體，但只有在 `author` AEM例項不健康

> `Note:` 請記得，如果發佈AEM例項不正常，Dispatcher會維持服務，以允許流量流向製作AEM例項

#### /bin/checkpublish（預設）

使用時，此指令碼會檢查並記錄其前端的任何執行個體，但只有在 `publish` AEM例項不健康

> `Note:` 請記得，如果製作AEM例項不正常，Dispatcher會維持運作，以允許流量流至發佈AEM例項

#### /bin/checkeither

使用時，此指令碼會檢查並記錄其前端的任何執行個體，但只有在 `author` 或 `publisher` AEM例項不健康

> `Note:` 請記得，如果publish AEM例項或author AEM例項不良，則Dispatcher會退出服務。  也就是說，如果其中一個健康，它也不會收到流量

#### /bin/checkboth

使用時，此指令碼會檢查並記錄其前端的任何執行個體，但只有在 `author` 和 `publisher` AEM例項不健康

> `Note:` 請記得，如果發佈AEM例項或製作AEM例項不良，Dispatcher就不會退出服務。  這表示，如果其中一個不健康，它會繼續收到流量，並導致請求資源的人發生錯誤。

#### /bin/healthy

此指令碼在使用時會檢查並記錄其正在播放的任何執行個體，但無論AEM是否傳回錯誤，都只會傳回正常。

> `Note:` 當健康狀況檢查未如需要運作，且允許覆寫將AEM例項保留在負載平衡器時，就會使用此指令碼。

[下一個 — > GIT符號連結](./git-symlinks.md)