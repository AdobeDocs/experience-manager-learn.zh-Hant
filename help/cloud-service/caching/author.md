---
title: AEM作者服務快取
description: AEMas a Cloud Service作者服務快取的一般總覽。
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 3%

---

# AEM 作者

由於所提供內容的高度動態性和許可權敏感性，AEM Author的快取能力有限。 一般而言，不建議為AEM Author自訂快取，而是依賴Adobe提供的快取設定來確保效能體驗。

![AEM作者快取概觀圖表](./assets/author/author-all.png){align="center"}

雖然不建議在AEM Author上自訂快取，但瞭解AEM Author具有Adobe管理的CDN，但不具有AEM Dispatcher會很有幫助。 請記住，AEM Author會忽略所有AEM Dispatcher設定，因為它沒有AEM Dispatcher。

## CDN

AEM Author服務使用CDN，但其目的在於增強產品資源的傳送，且不應進行大量設定，而是讓其照原樣運作。

![AEM發佈快取概觀圖表](./assets/author/author-cdn.png){align="center"}

AEM Author CDN位於一般使用者（通常是行銷人員或內容作者）和AEM Author之間。 它會快取不可變檔案，例如支援AEM編寫體驗的靜態資產，而不是編寫的內容。

AEM Author的CDN確實快取了多種可能感興趣的資源，包括 [持續性查詢上的可自訂TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances)，和 [自訂使用者端資料庫的長TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### 預設快取存留期

下列面向客戶的資源會由AEM Author CDN快取，且具有下列預設快取期限：

| 內容類型 | 預設CDN快取期限 |
|:------------ |:---------- |
| [持久查詢(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1分鐘 |
| [使用者端資料庫(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 天 |
| [其他一切](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | 未快取 |


## AEM Dispatcher

AEM Author服務不包含AEM Dispatcher，僅使用 [CDN](#cdn) 用於快取。
