---
title: AEMas a Cloud Service中的周遊警告
description: 了解如何減輕AEMas a Cloud Service中的周遊警告。
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
source-git-commit: a439c72a7b080633d3777eefad3b47f01c92b970
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 8%

---

# 遍歷警告

>[!TIP]
>將此頁面加入書籤，以供日後參考。

_遍歷警告是什麼？_

遍歷警告為 __aemerror__ 記錄陳述式，指出AEM Publish服務上執行的查詢效能不佳。 遍歷警告通常以兩種方式出現在AEM中：

1. __慢速查詢__ 不使用索引，導致回應時間緩慢。
1. __查詢失敗__，扔 `RuntimeNodeTraversalException`，導致體驗損毀。

取消勾選允許周遊警告，會降低AEM效能，並可能導致使用者體驗損毀。

## 如何解決遍歷警告

可使用三個簡單步驟來處理降低遍歷警告：分析、調整和驗證。 在確定最佳調整之前，需要多次調整和驗證。

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="分析" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="分析">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">分析問題</p>
               <p class="is-size-6">識別並了解正在遍歷的查詢。</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">分析</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="調整" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="調整">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">調整程式碼或設定</p>
               <p class="is-size-6">更新查詢和索引以避免查詢周遊。</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">調整</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="驗證" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="驗證">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">驗證調整是否有效</p>                       
               <p class="is-size-6">驗證對查詢和索引的更改將刪除遍歷。</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">驗證</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1.分析{#analyze}

首先，找出哪些AEM Publish服務顯示周遊警告。 要執行此操作，請從Cloud Manager, [下載發佈服務的 `aemerror` 記錄](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target="_blank"} 從所有環境（開發、預備和生產） __三天__.

![下載AEMas a Cloud Service記錄檔](./assets/traversals/download-logs.jpg)

開啟日誌檔案，並搜索Java™類 `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. 包含遍歷警告的日誌包含一系列看起來類似以下語句：

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

根據查詢執行的上下文，日誌語句可能包含有關查詢發起者的有用資訊：

+ 與查詢執行相關聯的HTTP要求URL

   + 範例: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak查詢語法

   + 範例: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath查詢

   + 範例: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ 執行查詢的代碼

   + 範例:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__查詢失敗__ 後跟a `RuntimeNodeTraversalException` 語句，類似於：

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2.調整{#adjust}

一旦發現違規查詢及其調用代碼，就必須進行調整。 可進行兩種調整以緩解遍歷警告：

### 調整查詢

__更改查詢__ 將解析的查詢限制添加到現有索引限制。 如果可能，則更改查詢而不是更改索引。

+ [了解如何調整查詢效能](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}

### 調整索引

__變更（或建立）AEM索引__ 這樣，現有查詢限制可解析為索引更新。

+ [了解如何調整現有索引](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}
+ [了解如何建立索引](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target="_blank"}

## 3.驗證{#verify}

必須驗證對查詢、索引或兩者所做的調整，以確保它們可緩解遍歷警告。

![說明查詢](./assets/traversals/verify.gif)

如果 [查詢調整](#adjust-the-query) ，可透過開發人員控制台在AEMas a Cloud Service上直接測試查詢 [說明查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target="_blank"}. 說明查詢會針對AEM製作服務執行，但由於製作和發佈服務的索引定義相同，因此就足以驗證AEM製作服務的查詢。

若 [對指數的調整](#adjust-the-index) 建立時，必須將索引部署至AEMas a Cloud Service。 部署索引調整後，開發人員控制台 [說明查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target="_blank"} 可用來執行及進一步調整查詢。

最終，所有變更（查詢和程式碼）都會提交至Git，並透過Cloud Manager部署至AEMas a Cloud Service。 部署後，會重新測試與原始遍歷警告相關聯的代碼路徑，並驗證遍歷警告不再出現在 `aemerror` 記錄檔。

## 其他資源

查看這些其他有用的資源，以了解AEM索引、搜尋和周遊警告。

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 — 搜尋與索引" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5 — 搜尋與索引"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 — 搜尋與索引">Cloud 5 — 搜尋與索引</a></p>
               <p class="is-size-6">Cloud 5團隊會展示AEM as a Cloud Service上搜尋和索引的內容和外觀。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">深入了解</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="內容搜尋與索引" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="內容搜尋與索引">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="內容搜尋與索引">內容搜尋和索引檔案</a></p>
               <p class="is-size-6">了解如何在AEMas a Cloud Service中建立和管理索引。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">深入了解</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="更新Oak索引" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="更新Oak索引">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="更新Oak索引">更新Oak索引</a></p>
               <p class="is-size-6">了解如何將AEM 6 Oak索引定義轉換為與AEMas a Cloud Service相容，並持續維護索引。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">深入了解</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="索引定義檔案" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="索引定義檔案">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="索引定義檔案">Lucene索引文檔</a></p>
               <p class="has-ellipsis is-size-6">Apache Oak Jackrabbit Lucene索引參考可記錄所有支援的Lucene索引設定。</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">深入了解</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
