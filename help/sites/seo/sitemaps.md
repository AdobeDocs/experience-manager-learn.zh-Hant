---
title: 網站地圖
description: 了解如何為AEM Sites建立網站地圖，以協助提升您的SEO。
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# 網站地圖

了解如何為AEM Sites建立網站地圖，以協助提升您的SEO。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 資源

+ [AEM Sitemap檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap檔案](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM核心WCM元件Github](https://github.com/adobe/aem-core-wcm-components)
   + v2.17.6中新增的Sitemap功能
+ [Sitemap.org Sitemap檔案](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引檔案檔案檔案](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 設定

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

定義 [OSGi工廠配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻率(使用 [cron運算式](http://www.cronmaker.com))網站地圖會在AEM中重新產生和快取。

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

允許Sitemap索引和Sitemap檔案的HTTP要求。

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

確保 `.xml` 系統會將Sitemap HTTP請求路由至正確的基礎AEM頁面。 如果未使用URL縮短，或使用Sling對應來達成URL縮短，則不需要此設定。

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
