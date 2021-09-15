---
title: 用於除錯AEM SDK的其他工具
description: 其他各種工具皆可協助您對AEM SDK的本機Quickstart進行除錯。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

---

# 用於除錯AEM SDK的其他工具

其他各種工具都可協助您在AEM SDK的本機Quickstart上為應用程式除錯。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是以網頁為基礎的介面，可與JCR、AEM資料存放庫互動。 CRXDE Lite可完全顯示JCR，包括節點、屬性、屬性值和權限。

CRXDE Lite位於：

+ 工具>一般>CRXDE Lite
+ 或直接在[http://localhost:4502/crx/de/index.jsp&lt;a1/](http://localhost:4502/crx/de/index.jsp)

## 說明查詢

![說明查詢](./assets/other-tools/explain-query.png)

說明AEM SDK本機Quickstart中的查詢網頁型工具，此工具提供AEM如何解譯及執行查詢的重要深入分析，且是確保AEM以效能執行查詢的寶貴工具。

解釋查詢位於：

+ 「工具」>「診斷」>「查詢效能」>「說明查詢」頁簽
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >「說明查詢」索引標籤

## QueryBuilder除錯程式

![QueryBuilder除錯程式](./assets/other-tools/query-debugger.png)

QueryBuilder除錯工具是網頁型工具，可協助您使用AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)語法來除錯及了解搜尋查詢。

QueryBuilder Debugger位於：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
