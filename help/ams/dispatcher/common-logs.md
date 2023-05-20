---
title: 調AEM度程式公用日誌
description: 查看Dispatcher中的常見日誌條目，瞭解它們的含義以及如何解決它們。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# 常見日誌

[目錄](./overview.md)

[&lt; — 上一個：虛榮URL](./disp-vanity-url.md)

## 概觀

文檔將描述您將看到的常見日誌條目及其含義和處理方法。

## GLOB警告

示例日誌條目：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

自Dispatcher模組4.2.x以來，他們開始勸阻人們在其篩選器檔案中使用以下類型的匹配項：

```
/0041 { /type "allow" /glob "* *.css *"   }
```

相反，最好使用新語法，如：

```
/0041 { /type "allow" /url "*.css" }
```

或者，最好不要在通配符匹配器上匹配：

```
/0041 { /type "allow" /extension "css" }
```

執行任何一種建議的方法都會從日誌中刪除該錯誤消息。

## 篩選拒絕


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b>
即使在日誌級別設定太低時發生拒絕，這些條目也不總是顯示。 將其設定為「資訊」或「調試」，以確保您能夠查看過濾器是否拒絕請求。
</div>

示例日誌條目：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

或:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>注意:</b>

瞭解已設定Dispatcher規則以過濾該請求。 在這種情況下，嘗試訪問的頁面被有意拒絕，我們不想對此執行任何操作。
</div>

如果您的日誌看起來像以下條目：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

讓我們知道我們的設計檔案 `.eot` 正在被阻止，我們會想要解決。
因此，我們應查看我們的篩選器檔案並添加以下行，以允許 `.eot` 檔案

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

這將允許檔案通過並停止記錄。
如果想查看正在篩選的內容，可以在日誌檔案中運行以下命令：

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 呈現超時

套接字超時示例日誌條目：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

在伺服器場的呈現部分中配置了錯誤的IP地址時，會發生這種情況。 該實例或實AEM例已停止響應或偵聽，而Dispatcher無法訪問它。

檢查防火牆規則，AEM並檢查實例是否正在運行。

網關超時示例日誌條目：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

這表示實AEM例具有一個開啟的套接字，它可以訪問該套接字，並在響應時超時。 這表示您AEM的實例速度太慢或運行不良，Dispatcher已在伺服器場的呈現部分中達到它配置的超時設定。 增加超時設定或使實例AEM正常。

## 快取級別

示例日誌條目：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

這表示從呈現級別讀取與從快取讀取的資料量。 您希望從快取中達到80%以上，您應按照幫助操作 [這裡](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

盡可能高這個數字。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b>
即使您在場檔案中設定了快取設定，以快取您可能經常或過於頻繁地刷新的所有內容，這可能導致發生較低百分比的快取命中率
</div>

## 缺少目錄

示例日誌條目：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

通常，在讀取項目時，當同時執行快取清除時，會顯示此資訊。

該資料夾或文檔根目錄對其權限錯誤，或者資料夾上的SELinux檔案上下文錯誤，該資料夾拒絕apache建立所需的新子目錄。

有關權限問題，請查看文檔的權限，並確保它們看起來類似於：

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 找不到虛榮URL

示例日誌條目：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

當您將Dispatcher配置為使用動態自動篩選器允許虛擬URL，但未通過在呈現器上安裝包完成安裝時，會AEM發生此錯誤。

要解決此問題，請在實例上安裝虛AEM擬url功能包，並允許匿名用戶準備。 詳細資訊 [這裡](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

設定的工作虛榮URL如下所示：

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 缺少場

示例日誌條目：

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

此錯誤表示從中提供的所有場檔案 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 他們無法從 `/virtualhost` 的子菜單。

伺服器場檔案根據請求傳入的域名或路徑匹配通信量。 它使用全局匹配，如果不匹配，則您要麼沒有正確配置伺服器場，要麼鍵入伺服器場中的條目，要麼完全丟失該條目。 當伺服器場與任何條目不匹配時，它最終將預設為包含的伺服器場檔案堆棧中包含的最後一個伺服器場。 在這個例子裡 `999_ams_publish_farm.any` 名稱為publishfarm的通用名稱。

下面是場檔案示例 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 被縮小以突出相關部件。

## 服務源項

示例日誌條目：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

通過內容的GEThttp方法讀取了該頁 `/content/we-retail/us/en.html` 用了24034毫秒。 我們最想關注的是 `publishfarm/0`。 你會發現它瞄準了 `publishfarm`。 已從render 0讀取請求。 表示必須從快取時請求AEM此頁。 現在，讓我們再次請求此頁，並查看日誌將發生的情況。

[下一步 — >只讀檔案](./immutable-files.md)
