---
title: 使用AEM開發跨原始資源共用(CORS)
description: 運用CORS以透過用戶端JavaScript從外部Web應用程式存取AEM內容的簡短範例。
version: 6.3, 6,4, 6.5
sub-product: 基礎，內容服務， sites
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
topic: 安全性
role: Developer
level: Beginner
feature: null
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---


# 跨原始資源共用(CORS)開發

透過用戶端JavaScript運用[!DNL CORS]從外部Web應用程式存取AEM內容的簡短範例。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

此影片中：

* **www.example.** commaps透過  `/etc/hosts`
* **aem-publish.** localmaps透過localhost  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (SimpleHTTPServer [[!DNL Python]的包裝函式](https://docs.python.org/2/library/simplehttpserver.html))正透過連接埠8000為HTML頁面提供服務。
* [!DNL AEM Dispatcher] 在2.4 [!DNL Apache HTTP Web Server] 上執行，且會向反向代理 `aem-publish.local` 請求 `localhost:4503`。

如需詳細資訊，請檢閱AEM](./understand-cross-origin-resource-sharing.md)中的[了解跨原始資源共用(CORS)。

## www.example.com HTML和JavaScript

此網頁的邏輯是

1. 按一下按鈕時
1. 對`http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`發出[!DNL AJAX GET]要求
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

## Dispatcher設定{#dispatcher-configuration}

若要允許快取內容上快取和提供CORS標頭，請在[/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders)後新增至所有支援AEM Publish `dispatcher.any`檔案。

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**對檔案進行更改** 後，重新啟動Web伺服器應 `dispatcher.any` 用程式。

在`/clientheaders`配置更新後，可能需要完全清除快取，以確保在下一個請求上正確快取標題。

## 支援材料{#supporting-materials}

* [AEM OSGi跨原始資源共用原則的設定工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [適用於macOS的SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) （與Windows/macOS/Linux相容）

* [了解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

