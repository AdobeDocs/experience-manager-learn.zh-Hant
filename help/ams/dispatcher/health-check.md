---
title: AMS Dispatcher健康狀態檢查
description: AMS提供健康狀態檢查cgi-bin指令碼，雲端負載平衡器將執行此指令碼以檢視AEM是否健康且應該持續為公共流量提供服務。
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
duration: 247
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---

# AMS Dispatcher健康狀態檢查

[目錄](./overview.md)

[&lt; — 上一步：唯讀檔案](./immutable-files.md)

當您安裝AMS基準版Dispatcher時，它會隨附一些免費贈品。  其中一項功能是一組健康情況檢查指令碼。
這些指令碼可讓前端AEM棧疊的負載平衡器知道哪些支腿狀況良好，並讓它們繼續運作。

![顯示流量流程的動畫GIF](assets/load-balancer-healthcheck/health-check.gif "健康狀態檢查步驟")

## 基本負載平衡器健康狀態檢查

當客戶流量透過網際網路到達您的AEM執行個體時，他們將透過負載平衡器

![影像顯示透過負載平衡器](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "負載平衡器流量流")從網際網路到aem的流量流

通過負載平衡器的每個請求都會將配置資源循環給每個執行個體。  負載平衡器已內建健康情況檢查機制，以確保其會將流量傳送至健康主機。

預設檢查通常是連線埠檢查，以檢視負載平衡器中的目標伺服器是否接聽連線埠流量進入（即TCP 80和443）

> `Note:`雖然此功能可運作，但AEM是否運作狀況良好，並沒有真正的量測計。  它只會測試Dispatcher (Apache Web Server)是否啟動並執行。

## AMS健康狀態檢查

為避免將流量傳送給遇到不健康的AEM執行個體的健康排程程式，AMS建立了一些評估腿部健康而不只是Dispatcher的額外專案。

![影像顯示運作狀況檢查的不同專案](assets/load-balancer-healthcheck/health-check-pieces.png "運作狀況檢查專案")

健康狀態檢查包含下列部分
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

我們將說明每個專案的設定及其重要性

### AEM套件

若要指示AEM是否正常運作，您需要它執行一些基本的頁面編譯並提供頁面。  Adobe Managed Services已建立包含測試頁面的基本套件。  頁面會測試存放庫是否正常運作，以及資源與頁面範本是否可轉譯。

![影像顯示CRX封裝管理員](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")中的AMS封裝

頁面如下。  它會顯示安裝的存放庫ID

![影像顯示AMS Regent頁面](assets/load-balancer-healthcheck/health-check-page.png "健康情況檢查頁面")

> `Note:`我們確定此頁面不可快取。  如果每次傳回快取頁面時，都不會檢查實際狀態！

這是我們可以測試的輕量端點，以檢視AEM是否正常運作。

### 負載平衡器設定

我們將負載平衡器設定為指向CGI-BIN端點，而不使用連線埠檢查。

![影像顯示AWS負載平衡器健康狀態檢查設定](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![影像顯示Azure負載平衡器健康狀態檢查組態](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache健康情況檢查虛擬主機

#### CGI-BIN虛擬主機`(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

這是`<VirtualHost>` Apache設定檔，可執行CGI-Bin檔案。

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin檔案是可執行的指令碼。  這可能是有漏洞的攻擊向量，並且AMS使用的這些指令碼無法公開發佈，只能供負載平衡器測試對。


#### 維護狀況不良的虛擬主機

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

這些檔案的名稱是`000_`，目的為做為前置詞。  其最初設定為使用與即時網站相同的網域名稱。  其用意是在健康情況檢查偵測到其中一個AEM後端發生問題時，啟用此檔案。  然後提供錯誤頁面，而不是只有沒有頁面的503 HTTP回應代碼。  它會從一般`.vhost`檔案竊取流量，因為它會在共用相同的`ServerName`或`ServerAlias`時，在該`.vhost`檔案之前載入。  導致預定要前往特定網域的頁面移至不正常的主機，而不是正常流量流經的預設主機。

健康情況檢查指令碼執行時，會登出目前的健康情況狀態。  每分鐘一次，伺服器上會執行一個cronjob，在記錄中尋找不健康的專案。  如果偵測到作者AEM執行個體運作不正常，則會啟用符號連結：

記錄專案：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron擷取錯誤並做出反應：

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

您可以在`/var/www/cgi-bin/health_check.conf`中設定重新載入模式設定，以控制作者或發佈的網站是否可載入此錯誤頁面

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有效選項：
- 作者
   - 這是預設選項。
   - 這會在作者狀態不佳時為其建立維護頁面
- 發佈
   - 此選項會在發佈者狀態不佳時為其建立維護頁面
- 全部
   - 此選項會為作者或發佈者（或兩者）建立維護頁面（如果它們狀態不良）
- 無
   - 此選項會略過健康狀態檢查的這項功能

檢視這些專案的`VirtualHost`設定時，您會看到它們載入相同的檔案，作為啟用時收到的每個請求的錯誤頁面：

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

回應代碼仍為`HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

他們將會取得此頁面，而不是空白頁面。

![影像顯示預設維護頁面](assets/load-balancer-healthcheck/unhealthy-page.png "unhealthy-page")

### CGI-Bin指令碼

您的CSE可以在負載平衡器設定中設定5個不同的指令碼，這些指令碼會變更將Dispatcher從負載平衡器提取時的行為或條件。

#### /bin/checkauthor

使用這個指令碼時，會檢查並記錄它所在的任何執行個體，但只有在`author`個AEM執行個體狀況不正常時才會傳回錯誤

> `Note:`請記住，如果發佈AEM執行個體不正常，Dispatcher將維持服務狀態，以允許流量流向作者AEM執行個體

#### /bin/checkpublish （預設）

使用這個指令碼時，會檢查並記錄它所在的任何執行個體，但只有在`publish`個AEM執行個體狀況不正常時才會傳回錯誤

> `Note:`請記住，如果作者AEM執行個體狀況不良，Dispatcher將維持服務狀態，讓流量流向發佈AEM執行個體

#### /bin/checkeither

使用這個指令碼時，會檢查並記錄它所在的任何執行個體，但只有在`author`或`publisher` AEM執行個體狀況不正常時，才會傳回錯誤

> `Note:`請記住，如果發佈AEM執行個體或編寫AEM執行個體不正常，Dispatcher將會退出服務。  這表示如果其中一個狀況良好，也不會收到流量

#### /bin/checkboth

使用這個指令碼時，會檢查並記錄它正在執行的任何執行個體，但只有在`author`和`publisher` AEM執行個體狀況不良時才會傳回錯誤

> `Note:`請記住，如果發佈AEM執行個體或編寫AEM執行個體不正常，Dispatcher將不會退出服務。  這表示如果其中一個狀況不良，系統仍會繼續接收流量，並對要求資源的人造成錯誤。

#### /bin/healthy

使用這個指令碼時，會檢查並記錄其面對的任何執行個體，但無論AEM是否傳回錯誤，都會正常傳回。

> `Note:`此指令碼用於健康情況檢查未如預期運作時，並允許覆寫以將AEM執行個體保留在負載平衡器中。

[下一個 — > GIT符號連結](./git-symlinks.md)
