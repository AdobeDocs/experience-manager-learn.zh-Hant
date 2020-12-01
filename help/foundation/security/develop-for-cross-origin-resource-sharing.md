---
title: 與AEM共同開發跨原始資源共用(CORS)
description: 利用CORS透過用戶端JavaScript從外部Web應用程式存取AEM內容的簡短範例。
version: 6.3, 6,4, 6.5
sub-product: 基礎，內容服務，網站
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---


# 開發跨原始資源共用(CORS)

利用[!DNL CORS]透過用戶端JavaScript從外部Web應用程式存取AEM內容的簡短範例。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

在此影片中：

* **www.example.** commaps to localhost via  `/etc/hosts`
* **aem-publish.** localmaps至localhost(透過  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (SimpleHTTPServer的包裝函 [[!DNL Python]式](https://docs.python.org/2/library/simplehttpserver.html))正透過連接埠8000為HTML頁面提供服務。
* [!DNL AEM Dispatcher] 正在 [!DNL Apache HTTP Web Server] 2.4上執行，並反向代理請求 `aem-publish.local` 至 `localhost:4503`。

如需詳細資訊，請參閱「瞭解AEM[中的跨原始資源共用(CORS)」。](./understand-cross-origin-resource-sharing.md)

## www.example.com HTML和JavaScript

此網頁具有

1. 按一下按鈕時
1. 向`http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`發出[!DNL AJAX GET]請求
1. 從JSON回應擷取`jcr:title`
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

## OSGi工廠配置

[!DNL Cross-Origin Resource Sharing]的OSGi配置工廠可通過以下方式獲得：

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

## Dispatcher configuration {#dispatcher-configuration}

若要允許快取和提供快取內容上的[!DNL CORS]標題，請將下列組態新增至所有支援的AEM Publish `dispatcher.any`檔案。

```
/cache { 
  ...
  /headers {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**對檔案進行更改** 後，重新啟動Web伺服器應 `dispatcher.any` 用程式。

在`/headers`組態更新後，可能需要完全清除快取，以確保標頭會在下次要求時正確快取。

## 支援材料{#supporting-materials}

* [AEM OSGi Configuration Factory for Cross-Origin Resource Sharing Policy](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer for macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) （Windows/macOS/Linux相容）

* [瞭解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

