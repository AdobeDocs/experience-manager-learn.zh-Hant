---
title: 設定AEM的Sling Dynamic Include
description: 在Apache HTTP Web Server上執行的Apache Sling Dynamic Include與AEM Dispatcher的安裝與使用視訊逐步導覽。
version: 6.3, 6.4, 6.5
sub-product: 基礎，網站
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# 設定 [!DNL Sling Dynamic Include]

在上執行的AEM Dispatcher中安裝和使 [!DNL Apache Sling Dynamic Include] 用 [的視訊逐步執行](https://docs.adobe.com/content/help/zh-Hant/experience-manager-dispatcher/using/dispatcher.html)[!DNL Apache HTTP Web Server]。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 請確定本機已安裝最新版的AEM Dispatcher。

1. 下載並安裝 [[!DNL Sling Dynamic Include] 套件](https://sling.apache.org/downloads.cgi)。
1. 透過 [!DNL Sling Dynamic Include] http://&lt; [!DNL OSGi Configuration Factory] host>: **&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration進行設定**。

   或者，若要新增至AEM程式碼庫，請在以下網址建 **立適當的sling:OsgiConfig** node:

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

1. （可選）重複最後一個步驟，允許在可編輯範本 [的鎖定（初始）內容上](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) ，也可透過 [!DNL SDI] 提供元件。 其他設定的原因是，可編輯範本的鎖定內容是從中提供，而 `/conf` 非提供 `/content`。

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

1. 更新 [!DNL Apache HTTPD Web server]的文 `httpd.conf` 件以啟用模 [!DNL Include] 塊。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 更新文 [!DNL vhost] 件以遵守包含指令。

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

1. 更新dispatcher.any配置檔案以支援(1)選擇器 `nocache` 和(2)啟用TTL支援。

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
   > 在上述全域 `*` 規則中留 `*.nocache.html*` 下拖尾，可能會導致 [子資源請求的問題](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 在更改其 [!DNL Apache HTTP Web Server] 配置檔案或之後，請始終重新啟動 `dispatcher.any`。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用 [!DNL Sling Dynamic Includes] 於提供邊緣端包含(ESI)，請確定在發送器快取中快 [取相關回應標題](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)。 可能的標題包括：
>
>* &quot;快取控制&quot;
>* &quot;內容處置&quot;
>* &quot;內容類型&quot;
>* &quot;過期&quot;
>* &quot;上次修改日期&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;上次修改日期&quot;

>



## 支援材料

* [下載Sling Dynamic Include搭售](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include檔案](https://github.com/Cognifide/Sling-Dynamic-Include)
