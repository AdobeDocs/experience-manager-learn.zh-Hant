---
title: 除錯AEMSDK的其他工具
description: 其他各種工具可協助除錯AEMSDK的本機快速入門。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 3%

---


# 除錯AEMSDK的其他工具

其他各種工具可協助您在SDK的本機快速入門AEM上除錯應用程式。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是一個基於Web的介面，用於與JCR（資料儲存庫）AEM交互。 CRXDE Lite可完全洞察JCR，包括節點、屬性、屬性值和權限。

CRXDE Lite位於：

+ 工具>一般>CRXDE Lite
+ 或直接寄至[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## 說明查詢

![說明查詢](./assets/other-tools/explain-query.png)

在AEMSDK的本機快速入門中說明查詢網路工具，提供如何解譯和執行查詢的關鍵深入資訊，以及確保以效能方式執行查詢的重要工具AEM。

Explain Query位於：

+ 「工具>診斷>查詢效能>說明查詢標籤」
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >「說明查詢」標籤

## QueryBuilder除錯程式

![QueryBuilder除錯程式](./assets/other-tools/query-debugger.png)

QueryBuilder除錯程式是網路工具，可協助您使用[QueryBuilderAEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)語法來除錯和瞭解搜尋查詢。

QueryBuilder除錯程式位於：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer和AEMChrome增效模組

![Sling Log Tracer和AEMChrome增效模組](./assets/other-tools/log-tracer.png)

[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)隨附於AEMSDK的本機快速入門，可讓您深入追蹤HTTP請求，並依請求揭露深度除錯資訊。[Log Tracer OSGi配置必須配置](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1)才能啟用此功能。

[AEM Google Chrome網頁瀏覽器](https://www.google.com/chrome/)的開放原始碼[ Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)與Log Tracer整合，直接在Chrome的Dev Tools中顯示除錯資訊。

_ChromeAEM增效模組是開放原始碼工具，而Adobe不提供支援。_

