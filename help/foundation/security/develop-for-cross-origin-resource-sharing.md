---
title: 使用AEM開發跨原始資源共用(CORS)
description: 運用CORS以透過用戶端JavaScript從外部Web應用程式存取AEM內容的簡短範例。
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# 跨原始資源共用(CORS)開發

利用的簡短範例 [!DNL CORS] 透過用戶端JavaScript從外部Web應用程式存取AEM內容。

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

此影片中：

* **www.example.com** 透過將對應到localhost `/etc/hosts`
* **aem-publish.local** 透過將對應到localhost `/etc/hosts`
* SimpleHTTPServer(的包裝函式 [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html))透過連接埠8000提供HTML頁面。
   * _Mac App Store已不提供。 使用類似的，例如 [吉夫斯](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] 正在運行 [!DNL Apache HTTP Web Server] 2.4和反向代理請求 `aem-publish.local` to `localhost:4503`.

如需詳細資訊，請檢閱 [了解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md).

## www.example.comHTML和JavaScript

此網頁的邏輯是

1. 按一下按鈕時
1. 成為 [!DNL AJAX GET] 要求 `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. 擷取 `jcr:title` 表單JSON回應
1. 插入 `jcr:title` 進入DOM

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

的OSGi配置工廠 [!DNL Cross-Origin Resource Sharing] 可透過：

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

若要允許快取內容上快取和提供CORS標題，請新增下列項目 [/clientheaders配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) 發佈至所有支援的AEM `dispatcher.any` 檔案。

```
/myfarm { 
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

**重新啟動Web伺服器應用程式** 變更 `dispatcher.any` 檔案。

可能需要完全清除快取，以確保在 `/clientheaders` 設定更新。

## 支援材料 {#supporting-materials}

* [AEM OSGi跨原始資源共用原則的設定工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [吉夫斯為macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (與Windows/macOS/Linux相容)

* [了解AEM中的跨原始資源共用(CORS)](./understand-cross-origin-resource-sharing.md)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
