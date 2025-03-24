---
title: 使用AEM開發跨原始資源共用(CORS)
description: 運用CORS透過使用者端AEM從外部Web應用程式存取JavaScript內容的簡短範例。
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# 開發跨原始資源共用(CORS)

運用[!DNL CORS]透過使用者端JavaScript從外部Web應用程式存取AEM內容的簡短範例。 此範例使用CORS OSGi設定在AEM上啟用CORS存取。 OSGi設定方法適用於以下情況：

* 單一來源正在存取AEM發佈內容
* AEM Author需要CORS存取權

如果需要對AEM Publish的多重來源存取權，請參閱[此檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration)。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

在本影片中：

* **www.example.com**&#x200B;透過`/etc/hosts`對應至localhost
* **aem-publish.local**&#x200B;透過`/etc/hosts`對應至localhost
* SimpleHTTPServer （[[!DNL Python]的SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)的包裝函式）正在透過連線埠8000提供HTML頁面。
   * _在Mac App Store中不再提供。 使用類似的，例如[Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher]正在[!DNL Apache HTTP Web Server] 2.4上執行並向`aem-publish.local`反向代理要求至`localhost:4503`。

如需更多詳細資料，請檢閱[瞭解AEM](./understand-cross-origin-resource-sharing.md)中的跨原始資源共用(CORS)。

## www.example.com HTML和JavaScript

此網頁的邏輯如下

1. 按一下按鈕時
1. 向`http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`提出[!DNL AJAX GET]要求
1. 從JSON回應中擷取`jcr:title`
1. 將`jcr:title`插入DOM

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

## OSGi工廠設定

[!DNL Cross-Origin Resource Sharing]的OSGi Configuration Factory可透過以下方式取得：

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

## Dispatcher設定 {#dispatcher-configuration}

### 允許CORS要求標頭

若要允許必要的[HTTP要求標頭傳遞至AEM進行處理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)，必須在Disaptcher的`/clientheaders`設定中允許這些標頭。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 快取CORS回應標頭

若要允許在快取的內容上快取及提供CORS標頭，請將下列[/cache /headers設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#caching-http-response-headers)新增至AEM發佈`dispatcher.any`檔案。

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

**變更`dispatcher.any`檔案後，重新啟動Web伺服器應用程式**。

可能需要完全清除快取，以確保在`/cache /headers`設定更新後，在下一個請求上適當地快取標頭。

## 支援材料 {#supporting-materials}

* macOS的[Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (與Windows/macOS/Linux相容)

* [瞭解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
