---
title: as a Cloud Service中的遍歷警AEM告
description: 瞭解如何在AEMas a Cloud Service中緩解遍歷警告。
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
source-git-commit: 45061581322e23efb936e91c11be48ceac64183b
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 2%

---


# 遍歷警告

_遍歷警告是什麼？_

遍歷警告為 __Aem錯誤__ 指示正在AEM發佈服務上執行效能差查詢的日誌語句。 遍歷警告通常以兩種AEM方式顯示：

1. __慢速查詢__ 不使用索引，導致響應時間較慢。
1. __查詢失敗__，那個 `RuntimeNodeTraversalException`導致體驗破裂。

允許未選中遍歷警告會降AEM低效能，並可能導致用戶體驗崩潰。

## 如何解決遍歷警告

可以通過三個簡單步驟來降低遍歷警告：分析、調整和驗證。 在確定最佳調整之前，需要多次調整和驗證。

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
               <p class="is-size-6">確定並瞭解正在遍歷的查詢。</p>
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
                <p class="headline is-size-5 has-text-weight-bold">調整代碼或配置</p>
               <p class="is-size-6">更新查詢和索引以避免查詢遍歷。</p>
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
               <p class="is-size-6">驗證對查詢和索引的更改是否刪除遍歷。</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">驗證</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1。分析{#analyze}

首先，確定哪些AEM發佈服務顯示遍歷警告。 為此，從雲管理器， [下載發佈服務&#39; `aemerror` 日誌](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target=&quot;_blank&quot;}，來自過去所有環境（開發、階段和生產） __三天__。

![下載AEMas a Cloud Service日誌](./assets/traversals/download-logs.jpg)

開啟日誌檔案並搜索Java™類 `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`。 包含遍歷警告的日誌包含一系列語句，這些語句看起來類似於：

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

根據查詢執行的上下文，日誌語句可能包含有關查詢發起方的有用資訊：

+ 與查詢執行關聯的HTTP請求URL

   + 範例: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak查詢語法

   + 範例: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath查詢

   + 範例: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ 執行查詢的代碼

   + 範例:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__查詢失敗__ 後跟一個 `RuntimeNodeTraversalException` 語句，類似於：

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2.調整{#adjust}

一旦發現違規查詢及其調用代碼，必須進行調整。 可以進行兩種類型的調整以緩解遍歷警告：

### 調整查詢

__更改查詢__ 將解析為現有索引限制的新查詢限制添加到。 如果可能，請更改查詢，而不是更改索引。

+ [瞭解如何調整查詢效能](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;

### 調整索引

__更改（或建立）索AEM引__ 這樣現有查詢限制可解析為索引更新。

+ [瞭解如何調整現有索引](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;
+ [瞭解如何建立索引](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target=&quot;_blank&quot;

## 3.驗證{#verify}

必須驗證對查詢、索引或兩者所做的調整，以確保它們能減輕遍歷警告。

![解釋查詢](./assets/traversals/verify.gif)

如果 [對查詢的調整](#adjust-the-query) 通過Developer Console在AEMas a Cloud Service上直接測試查詢 [解釋查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}。 解釋查詢針對AEM Author服務運行，但是，由於索引定義在Author和Publish服務中相同，因此對AEM Author服務驗證查詢就足夠了。

如果 [對索引的調整](#adjust-the-index) 建立，必須將索引部署到AEMas a Cloud Service。 部署索引調整後，Developer Console [解釋查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}可用於執行和進一步調整查詢。

最終，所有更改（查詢和代碼）都提交到Git，並使用雲管理器AEM部署到as a Cloud Service。 部署後，將test與原始遍歷警告相關的代碼路徑進行重新測試，並驗證遍歷警告是否不再出現在 `aemerror` 日誌。

## 其他資源

檢查這些其它有用資源，以了AEM解索引、搜索和遍歷警告。

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="雲5 — 搜索和索引" tabindex="-1"><img class="is-bordered-r-small" src="../../../cloud-5/imgs/009-thumb.png" alt="雲5 — 搜索和索引"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="雲5 — 搜索和索引">雲5 — 搜索和索引</a></p>
               <p class="is-size-6">「雲5」團隊展示了在as a Cloud Service上搜索和索引的內AEM容。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">視圖</span>
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
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="內容搜尋與索引">內容搜索和索引文檔</a></p>
               <p class="is-size-6">瞭解如何在as a Cloud Service中建立和管理索AEM引。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">視圖</span>
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="更新您的Oak索引" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="更新您的Oak索引">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="更新您的Oak索引">更新您的Oak索引</a></p>
               <p class="is-size-6">瞭解如何將AEM6個Oak索引定義轉換為AEMas a Cloud Service相容，並保持索引向前。</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">視圖</span>
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
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="索引定義文檔" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="索引定義文檔">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="索引定義文檔">Lucene索引文檔</a></p>
               <p class="has-ellipsis is-size-6">Apache Oak Jackrabbit Lucene索引引用，它記錄了所有支援的Lucene索引配置。</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">視圖</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>


