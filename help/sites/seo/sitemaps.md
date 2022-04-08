---
title: 錫泰馬普
description: 學習如何通過為AEM Sites建立模板來幫助提高SEO。
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# 錫泰馬普

學習如何通過為AEM Sites建立模板來幫助提高SEO。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 資源

+ [站點AEM地圖文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap文檔](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org站點地圖文檔](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引檔案文檔](https://www.sitemaps.org/protocol.html#index)
+ [克朗馬克](http://www.cronmaker.com/)

## 設定

### 站點地圖計畫程式OSGi配置

定義 [OSGi工廠配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻率(使用 [cron表達式](http://www.cronmaker.com))將在中重新生成和緩AEM存。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### 調度程式允許篩選器規則

允許對站點地圖索引和站點地圖檔案進行HTTP請求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重寫規則

確保 `.xml` 站點地圖HTTP請求被路由到正確的基礎AEM頁。 如果不使用URL縮短，或使用Sling映射實現URL縮短，則不需要此配置。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
