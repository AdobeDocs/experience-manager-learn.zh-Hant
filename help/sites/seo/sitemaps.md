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
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 5%

---

# 網站地圖

了解如何為AEM Sites建立網站地圖，以協助提升您的SEO。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 資源

+ [AEM Sitemap檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap檔案](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap檔案](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引檔案檔案檔案](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 設定

### Sitemap排程器OSGi設定

定義 [OSGi工廠配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻率(使用 [cron運算式](http://www.cronmaker.com))網站地圖會在AEM中重新產生和快取。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### 絕對Sitemap URL

AEM Sitemap支援絕對URL，方法是使用 [Sling對應](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). 若要這麼做，請在AEM服務上建立對應節點，以產生網站地圖（通常為AEM發佈服務）。

的Sling對應節點定義範例 `https://wknd.com` 可在下定義 `/etc/map/https` 如下所示：

| 路徑 | 屬性名稱 | 屬性類型 | 屬性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字串 | `wknd.com/$1` |

下面的螢幕擷圖說明類似的設定，但 `http://wknd.local` (運行於的本地主機名映射 `http`)。

![Sitemap絕對URL設定](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Dispatcher允許篩選規則

允許Sitemap索引和Sitemap檔案的HTTP要求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重寫規則

確保 `.xml` 系統會將Sitemap HTTP請求路由至正確的基礎AEM頁面。 如果未使用URL縮短，或使用Sling對應來達成URL縮短，則不需要此設定。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
