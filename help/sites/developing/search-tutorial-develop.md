---
title: 簡易搜尋實作指南
description: 簡單搜尋實作是來自2017 Summit lab AEM Search Demystified的資料。 本頁包含本實驗室的材料。 如需實驗室的導覽，請檢視本頁簡報區段中的實驗室活頁簿。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---

# 簡易搜尋實作指南{#simple-search-implementation-guide}

簡易搜尋實作是來自&#x200B;**Adobe Summit實驗室AEM Search Demystified**&#x200B;的資料。 本頁包含本實驗室的材料。 如需實驗室的導覽，請檢視本頁簡報區段中的實驗室活頁簿。

![搜尋架構概述](assets/l4080/simple-search-application.png)

## 簡報資料 {#bookmarks}

* [實驗室活頁簿](assets/l4080/l4080-lab-workbook.pdf)
* [簡報](assets/l4080/l4080-presentation.pdf)

## 書籤 {#bookmarks-1}

### 工具 {#tools}

* [索引管理員](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [說明查詢](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak：index/cqPageLucene
* [CRX封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html？)
* [Oak索引定義產生器](https://oakutils.appspot.com/generate/index)

### 章節 {#chapters}

*下列章節連結假設[初始套件](#initialpackages)已安裝在AEM Author`http://localhost:4502`*&#x200B;上

* [第1章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第2章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)章
* [第4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)章
* [第5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)章
* [第6章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)章
* [第8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)章
* [第9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)章

## 套件 {#packages}

### 初始封裝 {#initial-packages}

* [標記](assets/l4080/summit-tags.zip)
* [簡易搜尋應用程式套件](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 章節套件 {#chapter-packages}

* [第1章解決方案](assets/l4080/l4080-chapter1.zip)
* [第2章解決方案](assets/l4080/l4080-chapter2.zip)
* [第3章解決方案](assets/l4080/l4080-chapter3.zip)
* [第4章解決方案](assets/l4080/l4080-chapter4.zip)
* [第5章設定](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解決方案](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解決方案](assets/l4080/l4080-chapter6.zip)
* [第9章解決方案](assets/l4080/l4080-chapter9.zip)

## 引用的材料 {#reference-materials}

* [Github存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [Sling模型匯出工具](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/?lang=zh-Hant)
* [AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) （[檔案頁面](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/)）

## 更正與跟進 {#corrections-and-follow-up}

實驗室討論的更正和說明，以及與會者後續問題的回答。

1. **如何停止重新索引？**

   可透過[AEM Web Console > JMX](http://localhost:4502/system/console/jmx)提供的IndexStats MBean停止重新索引

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 執行`abortAndPause()`以中止重新索引。 這會鎖定索引以進一步重新索引，直到叫用`resume()`為止。
      * 執行`resume()`將重新啟動索引程式。
   * 檔案： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak索引如何支援多個租使用者？**

   Oak支援將索引置入整個內容樹狀結構，而且這些索引只會在該子樹狀結構中建立索引。 例如，可以建立&#x200B;**`/content/site-a/oak:index/cqPageLucene`**&#x200B;以僅在&#x200B;**`/content/site-a`.**&#x200B;下索引內容

   等效的方法是使用&#x200B;**`/oak:index`**&#x200B;下索引上的&#x200B;**`includePaths`**&#x200B;和&#x200B;**`queryPaths`**&#x200B;屬性。 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的考量事項為：

   * 查詢必須指定等於索引的查詢路徑範圍的路徑限制，或指定其子系。
   * 範圍較廣的索引（例如`/oak:index/cqPageLucene`）也會將資料編入索引，導致重複擷取和磁碟使用成本。
   * 可能需要重複的組態管理(例如 在多個租使用者索引中新增相同的indexRules （如果它們必須滿足相同的查詢集）
   * 此方法最適合用於AEM Publish層級的自訂網站搜尋，因為在AEM Author中，查詢通常會針對不同租使用者（例如透過OmniSearch）在內容樹狀結構的高處執行 — 不同的索引定義可能會產生僅根據路徑限制的不同行為。

3. **所有可用分析器的清單在何處？**

   Oak公開一組供AEM使用的lucene提供的分析器設定元素。

   * [Apache Oak Analyzer檔案](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [代碼器](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [篩選器](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在相同的查詢中搜尋頁面和Assets？**

   AEM 6.3的新功能是在相同提供的查詢中查詢多個節點型別。 下列QueryBuilder查詢。 請注意，每個「子查詢」都可以解析為自己的索引，因此在此範例中，`cq:Page`子查詢解析為`/oak:index/cqPageLucene`，`dam:Asset`子查詢解析為`/oak:index/damAssetLucene`。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   結果會產生下列查詢與查詢計畫：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   透過[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)探索查詢和結果。

5. **如何在相同查詢中搜尋多個路徑？**

   AEM 6.3的新功能是在相同提供的查詢中跨多個路徑進行查詢。 下列QueryBuilder查詢。 請注意，每個「子查詢」都可以解析為自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   下列查詢和查詢計畫中的結果

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   透過[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)探索查詢和結果。
