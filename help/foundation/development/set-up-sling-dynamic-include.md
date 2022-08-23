---
title: 為設定吊具動態包AEM括
description: 安裝和使用Apache Sling Dynamic Include與運行在Apache HTTP Web Server上AEM的Dispatcher的視頻瀏覽。
version: 6.4, 6.5
sub-product: foundation, sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 5%

---

# 設定 [!DNL Sling Dynamic Include]

安裝和使用的視頻瀏覽 [!DNL Apache Sling Dynamic Include] 與 [調度AEM程式](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant) 運行 [!DNL Apache HTTP Web Server]。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 確保已在本地安AEM裝最新版本的Dispatcher。

1. 下載並安裝 [[!DNL Sling Dynamic Include] 束](https://sling.apache.org/downloads.cgi)。
1. 配置 [!DNL Sling Dynamic Include] 通過 [!DNL OSGi Configuration Factory] 在 **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**。

   或者，要添加到代AEM碼庫，請建立相應的 **sling:OsgiConfig** 節點位於：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. （可選）重複最後一步，允許在 [已鎖定（初始）可編輯模板的內容](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 將通過 [!DNL SDI] 也是。 其他配置的原因是，可編輯模板的鎖定內容可從 `/conf` 而不是 `/content`。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. 更新 [!DNL Apache HTTPD Web server]`s `httpd.conf` 檔案以啟用 [!DNL Include] 中。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 更新 [!DNL vhost] 檔案以包含指令。

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. 更新dispatcher.any配置檔案以支援(1) `nocache` 選擇器和(2)啟用TTL支援。

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > 離開拖尾 `*` 在地球上 `*.nocache.html*` 上面的規則，可能 [子資源請求中的問題](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 始終重新啟動 [!DNL Apache HTTP Web Server] 更改其配置檔案或 `dispatcher.any`。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用 [!DNL Sling Dynamic Includes] 用於服務邊包括(ESI)，然後確保快取相關 [調度程式快取中的響應標頭](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)。 可能的標題包括：
>
>* &quot;快取控制&quot;
>* &quot;內容處置&quot;
>* &quot;內容類型&quot;
>* &quot;過期&quot;
>* &quot;上次修改時間&quot;
>* &quot;ETag&quot;
>* &quot;X內容類型選項&quot;
>* &quot;上次修改時間&quot;
>


## 支撐材料

* [下載Sling動態包含捆綁包](https://sling.apache.org/downloads.cgi)
* [Apache Sling動態包含文檔](https://github.com/Cognifide/Sling-Dynamic-Include)
