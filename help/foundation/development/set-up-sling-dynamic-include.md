---
title: 設定AEM適用的Sling Dynamic Include
description: 安裝和使用Apache Sling Dynamic Include搭配在Apache HTTP Web Server上執行的AEM Dispatcher的影片逐步說明。
version: 6.3, 6.4, 6.5
sub-product: foundation, sites
feature: API
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: 開發
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 5%

---


# 設定[!DNL Sling Dynamic Include]

安裝和使用[!DNL Apache Sling Dynamic Include]搭配[](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)在[!DNL Apache HTTP Web Server]上執行的AEM Dispatcher的影片逐步說明。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 請確定最新版AEM Dispatcher已安裝在本機。

1. 下載並安裝[[!DNL Sling Dynamic Include] 套件組合](https://sling.apache.org/downloads.cgi)。
1. 透過[!DNL OSGi Configuration Factory]在&#x200B;**http://&lt;host>配置[!DNL Sling Dynamic Include]:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**。

   或者，若要新增至AEM程式碼基底，請在下列位置建立適當的&#x200B;**sling:OsgiConfig**&#x200B;節點：

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

1. （可選）重複最後一步，允許[上鎖定的（初始）可編輯範本](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/page-templates-editable.html)的內容也通過[!DNL SDI]提供。 其他配置的原因是，可編輯模板的鎖定內容是從`/conf`提供，而不是從`/content`提供。

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

1. 更新[!DNL Apache HTTPD Web server]的`httpd.conf`檔案以啟用[!DNL Include]模組。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 更新[!DNL vhost]檔案以遵循include指令。

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

1. 更新dispatcher.any設定檔案以支援(1)`nocache`選取器及(2)啟用TTL支援。

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
   > 若將結尾的`*`保留在上述全域`*.nocache.html*`規則中，可能會導致子資源](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)要求中出現[問題。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 在對其配置檔案或`dispatcher.any`進行更改後，始終重新啟動[!DNL Apache HTTP Web Server]。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用[!DNL Sling Dynamic Includes]來提供邊緣端包含(ESI)，請務必在Dispatcher快取](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)中快取相關的[回應標題。 可能的標題包括：
>
>* &quot;快取控制&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;過期&quot;
>* &quot;上次修改時間&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;上次修改時間&quot;

>



## 支援材料

* [下載Sling Dynamic Include套件組合](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include檔案](https://github.com/Cognifide/Sling-Dynamic-Include)
