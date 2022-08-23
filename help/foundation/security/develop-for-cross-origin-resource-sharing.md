---
title: 開發跨源資源共用(CORS)AEM
description: 利用CORS通過客戶端JavaScript從AEM外部Web應用程式訪問內容的一個簡短示例。
version: 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# 跨源資源共用(CORS)開發

利用的一個簡短示例 [!DNL CORS] 通過AEM客戶端JavaScript從外部Web應用程式訪問內容。

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

在此視頻中：

* **www.example.com** 通過映射到本地主機 `/etc/hosts`
* **aem.publish.local** 通過映射到本地主機 `/etc/hosts`
* SimpleHTTPServer(用於 [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))正在通過埠8000為HTML頁提供服務。
   * _Mac·App Store不再提供。 使用類似 [吉夫斯](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)。_
* [!DNL AEM Dispatcher] 正在運行 [!DNL Apache HTTP Web Server] 2.4和反向代理請求 `aem-publish.local` 至 `localhost:4503`。

有關詳細資訊，請查看 [理解中的跨源資源共AEM享](./understand-cross-origin-resource-sharing.md)。

## www.example.comHTML和JavaScript

此網頁具有邏輯

1. 按一下按鈕
1. 製作 [!DNL AJAX GET] 請求 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. 檢索 `jcr:title` 形成JSON響應
1. 彈出 `jcr:title` 進入DOM

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

OSGi配置工廠 [!DNL Cross-Origin Resource Sharing] 可通過以下途徑獲得：

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

## 調度程式配置 {#dispatcher-configuration}

要允許快取內容上的CORS標頭並提供服務，請添加以下內容 [/clieader配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) 到所有支援AEM發佈 `dispatcher.any` 的子菜單。

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

**重新啟動Web伺服器應用程式** 對 `dispatcher.any` 的子菜單。

可能需要完全清除快取，以確保在下次請求後，標頭被適當快取 `/clientheaders` 配置更新。

## 支撐材料 {#supporting-materials}

* [OSGiAEM跨源資源共用策略配置工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [吉夫斯為macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (Windows/macOS/Linux相容)

* [理解中的跨源資源共AEM享](./understand-cross-origin-resource-sharing.md)
* [跨源資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP訪問控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
