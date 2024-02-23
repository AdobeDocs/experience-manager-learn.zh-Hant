---
title: AEM Dispatcher通用記錄檔
description: 檢視Dispatcher提供的常見記錄專案，並瞭解它們的含義以及如何解決它們。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 252
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 0%

---

# 通用記錄檔

[目錄](./overview.md)

[&lt; — 上一步：虛名URL](./disp-vanity-url.md)

## 概觀

檔案將說明您會看到的常見記錄專案，以及它們的含義和處理方式。

## GLOB警告

範例記錄專案：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

自從Dispatcher模組4.2.x開始，他們開始勸阻人們在其篩選器檔案中使用以下型別的相符專案：

```
/0041 { /type "allow" /glob "* *.css *"   }
```

最好改用新語法，例如：

```
/0041 { /type "allow" /url "*.css" }
```

或者，最好根本不以萬用字元比對器比對：

```
/0041 { /type "allow" /extension "css" }
```

執行其中一種建議的方法都會從記錄中移除該錯誤訊息。

## 篩選拒絕

>[!NOTE]
>
>如果記錄層級設定得太低，即使發生拒絕，也不會始終顯示這些專案。 將其設為「資訊」或「除錯」，以確保您可檢視篩選器是否拒絕請求。

範例記錄專案：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

或：

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>瞭解Dispatcher的規則已設定為篩選掉該請求。 在此情況下，嘗試瀏覽的頁面會被有意拒絕，我們不需要對此執行任何動作。

如果您的記錄檔看起來像這個專案：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

讓我們知道我們的設計檔案 `.eot` 正遭到封鎖，我們將會修正此問題。
因此，我們應該檢視篩選器檔案，並新增下列行來允許 `.eot` 檔案透過

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

這將允許檔案通過並停止記錄。
如果您想檢視篩選掉的內容，可以在記錄檔上執行此命令：

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 轉譯器逾時

通訊端逾時範例記錄專案：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

當您在伺服器陣列的renders區段中設定錯誤的IP位址時，就會發生這種情況。 該AEM執行個體停止回應或偵聽，Dispatcher無法存取它。

檢查防火牆規則，確認AEM執行個體正在執行且狀況良好。

閘道逾時範例記錄專案：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

這表示AEM執行個體有一個開啟的通訊端，它可以透過回應觸及並逾時。 這表示您的AEM執行個體太慢或不健康，Dispatcher已在陣列的轉譯區段中到達其設定的逾時設定。 請增加逾時設定或讓AEM執行個體正常運作。

## 快取層級

範例記錄專案：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

這表示會測量您從轉譯器層級和從快取擷取的次數。 您想從快取中達到80%以上，您應該按照說明操作 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den)：

讓此數目儘可能高。

>[!NOTE]
>
>即使您在伺服器陣列檔案中有快取設定來快取所有內容，您可能會過於頻繁或過於激進地排清，這可能會導致發生快取命中率的百分比降低

## 缺少目錄

記錄專案範例：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

這通常會在擷取專案時同時進行快取清除時顯示。

該目錄或檔案根目錄的許可權不正確，或資料夾上的SELinux檔案內容錯誤，拒絕Apache建立所需的新子目錄來進行快取。

有關許可權問題，請檢視documentroot的許可權，確保其內容與以下內容類似：

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 找不到虛名URL

記錄專案範例：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

當您已將Dispatcher設定為使用動態自動篩選允許虛名URL，但未透過在AEM轉譯器上安裝套件來完成設定時，會發生此錯誤。

若要修正此問題，請在AEM執行個體上安裝虛名URL功能套件，並允許匿名使用者準備就緒。 詳細資料 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

設定的有效虛名URL如下所示：

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 缺少陣列

記錄專案範例：

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

此錯誤表示從所有可用的伺服器陣列檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 他們無法從以下位置找到相符的專案 `/virtualhost` 區段。

伺服器陣列檔案會根據要求所傳入的網域名稱或路徑來比對流量。 這會使用glob比對，如果不符合，則表示您未正確設定陣列、輸入陣列中的專案拼字，或專案完全遺失。 當陣列不符合任何專案時，它最終只會預設為陣列檔案棧疊中包含的最後一個陣列。 在此範例中， `999_ams_publish_farm.any` 命名為publishfarm的一般名稱。

以下是範例伺服器陣列檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 會縮小以反白相關零件。

## 服務專案來源

記錄專案範例：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

已透過內容的GEThttp方法擷取頁面 `/content/we-retail/us/en.html` 花了24034毫秒。 我們要注意的是最後部分 `publishfarm/0`. 您會看到它鎖定目標並符合 `publishfarm`. 已從轉譯器0擷取請求。 這表示必須從AEM要求此頁面，然後快取。 現在，讓我們再次要求此頁面，並檢視記錄的情況。

[下一個 — >唯讀檔案](./immutable-files.md)
