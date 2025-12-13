---
title: Sitemaps
description: 瞭解如何為AEM Sites建立Sitemap，以利提升SEO。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 4%

---

# Sitemaps

瞭解如何為AEM Sites建立Sitemap，以利提升SEO。

>[!WARNING]
>
>此影片示範如何在網站地圖中使用相對URL。 Sitemap [應使用絕對URL](https://sitemaps.org/protocol.html)。 請參閱[組態](#absolute-sitemap-urls)以瞭解如何啟用絕對URL，因為下列影片並未說明此問題。

>[!VIDEO](https://video.tv.adobe.com/v/3454372?captions=chi_hant&quality=12&learn=on)

## 設定

### 絕對網站地圖URL{#absolute-sitemap-urls}

AEM的Sitemap使用[Sling對應](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)支援絕對URL。 這是透過在AEM服務上建立對應節點以產生Sitemap (通常是AEM Publish服務)來完成。

`https://wknd.com`的Sling對應節點定義範例可在`/etc/map/https`下定義，如下所示：

| 路徑 | 屬性名稱 | 屬性型別 | 屬性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字串 | `wknd.com/$1` |

以下熒幕擷圖說明類似設定，但適用於`http://wknd.local` （在`http`上執行的本機主機名稱對應）。

![Sitemap絕對URL組態](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Sitemap排程器OSGi設定

定義在AEM中重新/產生並快取Sitemap之頻率（使用[cron運算式](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler)）的[OSGi工廠設定](https://cron.help/)。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.author`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher允許篩選器規則

允許Sitemap索引和Sitemap檔案的HTTP要求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重寫規則

請確定`.xml`個Sitemap HTTP要求已路由至正確的基礎AEM頁面。 如果未使用URL縮短功能，或使用Sling對應來縮短URL，則不需要進行此設定。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## 資源

+ [AEM Sitemap檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=zh-Hant)
+ [Apache Sling Sitemap檔案](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap檔案](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引檔案檔案](https://www.sitemaps.org/protocol.html#index)
+ [Cron協助程式](https://cron.help/)
