---
title: 設定Sling Dynamic Include for AEM
description: 在Apache HTTP Web Server上執行Apache Sling Dynamic Include搭配Dispatcher的AEM安裝與使用視訊逐步執行。
version: 6.3, 6.4, 6.5
sub-product: 基礎，網站
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 6%

---


# 設定[!DNL Sling Dynamic Include]

在[!DNL Apache HTTP Web Server]上安裝並使用[!DNL Apache Sling Dynamic Include]和[AEM Dispatcher](https://docs.adobe.com/content/help/zh-Hant/experience-manager-dispatcher/using/dispatcher.html)的視頻逐步執行。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 確保本地安裝了最AEM新版本的Dispatcher。

1. 下載並安裝[[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi)。
1. 透過[!DNL OSGi Configuration Factory]位於&#x200B;**http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;的來設定[!DNL Sling Dynamic Include]。

   或者，若要新增至程式AEM碼庫，請在以下網址建立適當的&#x200B;**sling:OsgiConfig**&#x200B;節點：

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

1. （可選）重複最後一個步驟，允許通過[!DNL SDI]提供可編輯模板](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/page-templates-editable.html)的鎖定（初始）內容上的元件。 [其他配置的原因是，可編輯模板的鎖定內容是從`/conf`而不是`/content`提供的。

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

1. 更新[!DNL vhost]檔案以遵守包含指令。

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

1. 更新dispatcher.any配置檔案以支援(1)`nocache`選擇器和(2)啟用TTL支援。

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
   > 在上述全域`*.nocache.html*`規則中將尾隨`*`留下，可能會導致子資源](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)請求出現[問題。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 對[!DNL Apache HTTP Web Server]的配置檔案或`dispatcher.any`進行更改後，請始終重新啟動。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用[!DNL Sling Dynamic Includes]來提供邊緣端包含(ESI)，請確定在調度器快取](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)中快取相關的[響應標頭。 可能的標題包括：
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
