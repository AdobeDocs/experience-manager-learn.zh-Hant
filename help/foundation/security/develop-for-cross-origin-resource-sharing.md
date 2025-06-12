---
title: 使用 AEM 進行跨來源資源共用 (CORS) 開發
description: 運用 CORS 從外部網頁應用程式，透過用戶端 JavaScript 存取 AEM 內容的簡短範例。
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '318'
ht-degree: 100%

---

# 跨來源資源共用 (CORS) 開發

運用 [!DNL CORS] 從外部網頁應用程式，透過用戶端 JavaScript 存取 AEM 內容的簡短範例。此範例使用 CORS OSGi 設定在 AEM 上啟用 CORS 存取。OSGi 設定方法在下列情況下是可行的：

* 單一來源正在存取 AEM Publish 內容
* AEM Author 需要 CORS 存取權

如果需要對 AEM Publish 進行多重來源存取，請參閱[此文件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=zh-Hant#dispatcher-configuration)。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

在本影片中：

* **www.example.com** 透過 `/etc/hosts` 對應至 localhost
* **aem-publish.local** 透過 `/etc/hosts` 對應至 localhost
* SimpleHTTPServer ([[!DNL Python] SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) 的包裝器) 透過連接埠 8000 提供 HTML 頁面。
   * _Mac App Store 不再提供。使用相似項目，如 [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)。_
* [!DNL AEM Dispatcher] 正在 [!DNL Apache HTTP Web Server] 2.4 上執行，並將對 `aem-publish.local` 的要求反向代理至 `localhost:4503`。

如需更多詳細資料，請檢閱[了解 AEM 中的跨來源資源共用 (CORS)](./understand-cross-origin-resource-sharing.md)。

## www.example.com HTML 和 JavaScript

此網頁的邏輯會

1. 根據按鈕的點按
1. 向 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json` 發送 [!DNL AJAX GET] 要求
1. 從 JSON 回應中擷取 `jcr:title` 表單
1. 將 `jcr:title` 注入到 DOM 中

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGi 工廠設定

[!DNL Cross-Origin Resource Sharing] 的 OSGi 設定工廠可以透過以下方式取得：

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher 設定 {#dispatcher-configuration}

### 允許 CORS 要求標頭

若要允許必要的 [HTTP 要求標頭傳遞至 AEM 進行處理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#specifying-the-http-headers-to-pass-through-clientheaders)，必須在 Dispatcher 的 `/clientheaders` 設定中允許標頭。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 快取 CORS 回應標頭

若要允許在快取內容上快取及提供 CORS 標頭，請將以下 [/cache /headers 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#caching-http-response-headers)新增至 AEM Publish `dispatcher.any` 檔案中。

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

在 `dispatcher.any` 檔案進行變更後，請&#x200B;**重新啟動網頁伺服器應用程式**。

可能需要完全清除快取，以確保於 `/cache /headers` 設定更新後，在下一個要求中能適當地快取標頭。

## 支援素材 {#supporting-materials}

* [適用於 macOS 的 Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (相容於 Windows/macOS/Linux)

* [了解 AEM 中的跨來源資源共用 (CORS)](./understand-cross-origin-resource-sharing.md)
* [跨來源資源共用 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP 存取控制 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
