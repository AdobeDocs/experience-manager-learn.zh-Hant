---
title: AEM Dispatcher公用記錄檔
description: 檢視Dispatcher的常見記錄項目，並了解其含義及處理方式。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# 常見記錄檔

[目錄](./overview.md)

[&lt; — 上一個：虛名URL](./disp-vanity-url.md)

## 概觀

文檔將描述您將看到的常見日誌條目、它們的含義以及如何處理它們。

## GLOB警告

日誌條目示例：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

自從Dispatcher模組4.2.x以來，他們開始勸阻人們在其篩選器檔案中使用下列類型的符合：

```
/0041 { /type "allow" /glob "* *.css *"   }
```

請改為使用新語法，例如：

```
/0041 { /type "allow" /url "*.css" }
```

或者更好不要在萬用字元比對器上比對：

```
/0041 { /type "allow" /extension "css" }
```

執行任一建議方法將會移除記錄檔中的錯誤訊息。

## 篩選拒絕


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
即使發生拒絕（如果記錄層級設定得太低），這些項目也不會一律顯示。 將其設為「資訊」或「除錯」，以確保您可以查看篩選器是否拒絕請求。
</div>

日誌條目示例：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

或:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>注意:</b>

了解Dispatcher的規則已設定為篩選該請求。 在此情況下，嘗試瀏覽的頁面被故意拒絕，我們不想對此執行任何操作。
</div>

如果您的日誌看起來類似以下條目：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

這讓我們知道我們的設計檔案 `.eot` 正被封鎖，我們將希望修復。
因此，我們應查看篩選器檔案，並新增下列行，以允許 `.eot` 檔案通過

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

這會允許檔案通過，並停止記錄。
如果您想查看正在篩選的內容，可以在日誌檔案上運行以下命令：

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 轉譯的逾時

通訊端逾時範例記錄項目：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

當您在伺服器陣列的「renders」區段中設定了錯誤的IP位址時，就會發生此情況。 該或AEM例項已停止回應或監聽，而Dispatcher無法存取。

檢查防火牆規則，確認AEM例項執行正常。

網關超時日誌項示例：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

這表示AEM例項有可觸及的開啟通訊端，且回應時逾時。 這表示您的AEM例項速度太慢或不良，且Dispatcher已達到在伺服器陣列的呈現區段中設定的逾時設定。 增加逾時設定，或讓AEM執行個體健康。

## 快取層級

日誌條目示例：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

這表示會測量從轉譯層級與從快取的擷取。 您希望從快取中點擊80%以上，而您應該遵循說明 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

盡可能高。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
即使您在伺服器陣列檔案中設定了快取設定，以快取您正在排清的所有內容，可能會過於頻繁或過於嚴重，導致快取命中率降低
</div>

## 缺少目錄

日誌條目示例：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

當快取清除同時發生時，通常會在擷取項目時顯示。

該或文檔根目錄的權限不正確，或資料夾上的錯誤SELinux檔案上下文拒絕apache建立新需要的子目錄。

若為權限問題，請查看檔案的權限，並確認其看起來類似：

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 找不到虛名URL

日誌條目示例：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

當您將Dispatcher設定為使用動態自動篩選允許虛名URL，但未透過在AEM轉譯器上安裝套件來完成設定時，即會發生此錯誤。

若要修正此問題，請在AEM例項上安裝虛名url功能套件，並允許匿名使用者準備好。 詳細資料 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

有效的虛名URL設定如下：

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 缺少場

日誌條目示例：

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

此錯誤表示從中可用的所有伺服器陣列檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 他們無法從 `/virtualhost` 區段。

伺服器陣列檔案會根據要求傳入的網域名稱或路徑來比對流量。 它使用全局匹配，如果不匹配，則表示您未正確配置場、鍵入場中的項，或該項完全丟失。 當伺服器陣列與任何項目不匹配時，它最終只會預設為包含的伺服器陣列檔案堆棧中包含的最後一個伺服器陣列。 在這個例子裡， `999_ams_publish_farm.any` 此名稱為publishfarm的一般名稱。

以下是範例伺服器陣列檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 被縮小以突出相關部分。

## 已送達項目

日誌條目示例：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

已透過內容的GEThttp方法擷取頁面 `/content/we-retail/us/en.html` 它用了24034毫秒。 我們最想關注的就是 `publishfarm/0`. 您會看到它已鎖定目標並符合 `publishfarm`. 已從呈現0中擷取請求。 這表示必須先從AEM請求此頁面，然後快取。 現在，讓我們再次請求此頁面，並查看記錄有何變化。

[Next ->只讀檔案](./immutable-files.md)