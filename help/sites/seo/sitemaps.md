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
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 4%

---

# 錫泰馬普

學習如何通過為AEM Sites建立模板來幫助提高SEO。

>[!WARNING]
>
>此視頻演示在站點地圖中使用相對URl。 錫泰馬普 [應使用絕對URL](https://sitemaps.org/protocol.html)。 請參閱 [配置](#absolute-sitemap-urls) 如何啟用絕對URL，因為以下視頻中未包括此內容。

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## 設定

### 絕對站點地圖URL{#absolute-sitemap-urls}

站AEM點地圖支援絕對URL [吊索映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)。 這是通過在生成站點集(通AEM常是AEM發佈服務)的服務上建立映射節點來完成的。

Sling映射節點定義示例 `https://wknd.com` 可在 `/etc/map/https` 如下：

| 路徑 | 屬性名稱 | 屬性類型 | 屬性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字串 | `wknd.com/$1` |

下面的螢幕快照說明了類似的配置，但 `http://wknd.local` (本地主機名映射運行於 `http`)。

![站點地圖絕對URL配置](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### 站點地圖計畫程式OSGi配置

定義 [OSGi工廠配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 頻率(使用 [cron表達式](http://www.cronmaker.com))將重新生成或快取站點AEM。

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

## 資源

+ [站點AEM地圖文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap文檔](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org站點地圖文檔](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引檔案文檔](https://www.sitemaps.org/protocol.html#index)
+ [克朗馬克](http://www.cronmaker.com/)