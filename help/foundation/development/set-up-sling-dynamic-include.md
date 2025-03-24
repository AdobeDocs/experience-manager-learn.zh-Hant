---
title: 為AEM設定Sling動態包含
description: 在Apache HTTP Web Server上執行AEM Dispatcher時，安裝和使用Apache Sling Dynamic Include的影片逐步說明。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 設定[!DNL Sling Dynamic Include]

在[!DNL Apache HTTP Web Server]上執行[AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)時，安裝及使用[!DNL Apache Sling Dynamic Include]的影片逐步解說。

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 確保本機已安裝最新版AEM Dispatcher。

1. 下載並安裝[[!DNL Sling Dynamic Include] 套件](https://sling.apache.org/downloads.cgi)。
1. 透過&#x200B;**http://&lt;host>：&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;的[!DNL OSGi Configuration Factory]設定[!DNL Sling Dynamic Include]。

   或者，若要新增至AEM程式碼基底，請在以下位置建立適當的&#x200B;**sling：OsgiConfig**&#x200B;節點：

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

1. （選擇性）重複上一個步驟，允許透過[!DNL SDI]也提供[可編輯範本](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html)的鎖定（初始）內容上的元件。 額外設定的原因是可編輯範本的鎖定內容是從`/conf`而非`/content`提供。

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

1. 更新[!DNL vhost]檔案以遵循include指示。

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

1. 更新dispatcher.any設定檔以支援(1) `nocache`選取器和(2)啟用TTL支援。

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
   > 將上述glob `*.nocache.html*`規則中的尾端`*`保留為關閉，可能會導致子資源的要求](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)中有[個問題。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 變更組態檔或`dispatcher.any`後，請一律重新啟動[!DNL Apache HTTP Web Server]。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用[!DNL Sling Dynamic Includes]來提供Edge-side Include (ESI)，請務必在Dispatcher快取中快取相關的[回應標頭](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)。 可能的標頭包括：
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;過期&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>

## 支援材料

* [下載Sling動態包含套件](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include檔案](https://github.com/Cognifide/Sling-Dynamic-Include)
