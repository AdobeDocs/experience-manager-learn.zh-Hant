---
title: 簡易搜尋實作指南
description: 「簡單」搜尋實作是2017年峰會實驗室搜尋解說AEM明檔案。 本頁包含本實驗的資料。 如需實驗的導覽，請檢視本頁「簡報」區段中的「實驗活頁簿」。
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
feature: 搜尋
topic: 開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 2%

---


# 簡易搜尋實作指南{#simple-search-implementation-guide}

「簡單搜索」實施是&#x200B;**Adobe峰會實驗室的「搜AEM索演示」中的材料。**&#x200B;本頁包含本實驗的資料。 如需實驗的導覽，請檢視本頁「簡報」區段中的「實驗活頁簿」。

![搜尋架構概觀](assets/l4080/simple-search-application.png)

## 簡報資料{#bookmarks}

* [實驗活頁簿](assets/l4080/l4080-lab-workbook.pdf)
* [簡報](assets/l4080/l4080-presentation.pdf)

## 書籤{#bookmarks-1}

### 工具 {#tools}

* [索引管理員](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [說明查詢](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder除錯程式](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### 章節 {#chapters}

*以下章節連結假設AEM Author [上](#initialpackages) 已安裝Initial Packages，網址為`http://localhost:4502`*

* [第1章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第二章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第三章](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [第4章](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [第五章](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [第6章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第七章](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [第8章](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [第九章](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## 套件 {#packages}

### 初始包{#initial-packages}

* [標記](assets/l4080/summit-tags.zip)
* [簡易搜尋應用程式套件](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 章節包{#chapter-packages}

* [第1章解決方案](assets/l4080/l4080-chapter1.zip)
* [第二章解決方案](assets/l4080/l4080-chapter2.zip)
* [第三章解決方案](assets/l4080/l4080-chapter3.zip)
* [第4章解決方案](assets/l4080/l4080-chapter4.zip)
* [第五章設定](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解決方案](assets/l4080/l4080-chapter5-solution.zip)
* [第六章解決方案](assets/l4080/l4080-chapter6.zip)
* [第9章解決方案](assets/l4080/l4080-chapter9.zip)

## 參考材料{#reference-materials}

* [Github儲存庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://docs.adobe.com/docs/en/aem/6-2/develop/search/querybuilder-api.html)
* [AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([檔案頁面](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 修正與後續行動{#corrections-and-follow-up}

實驗室討論的更正和說明，以及與會者對後續問題的回答。

1. **如何停止重新建立索引？**

   可以透過[AEM Web Console > JMX](http://localhost:4502/system/console/jmx)提供的IndexStats MBean停止重新建立索引

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 執行`abortAndPause()`以中止重新建立索引。 這將鎖定索引以進一步重新索引，直到調用`resume()`。
      * 執行`resume()`將重新啟動索引過程。
   * 檔案：[https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **橡樹索引如何支援多個租戶？**

   Oak支援將索引置於內容樹狀結構中，而這些索引將只會在該子樹狀結構中建立索引。 例如，**`/content/site-a/oak:index/cqPageLucene`**&#x200B;可以建立為僅在&#x200B;**`/content/site-a`下的內容編製索引。**

   等效方法是在&#x200B;**`/oak:index`**&#x200B;下的索引上使用&#x200B;**`includePaths`**&#x200B;和&#x200B;**`queryPaths`**&#x200B;屬性。 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的考慮因素包括：

   * 查詢必須指定與索引的查詢路徑範圍相等的路徑限制，或者是其子體。
   * 範圍更廣的索引（例如`/oak:index/cqPageLucene`）也將為資料編製索引，從而導致重複的提取和磁碟使用成本。
   * 可能需要重複的配置管理(例如 在多個租用戶索引上新增相同的indexRules（如果它們必須滿足相同的查詢集）
   * 此方法在AEM Publish層最適合用於自訂網站搜尋，就像在AEM Author中一樣，常會針對不同租戶（例如，透過OmniSearch）在內容樹狀結構上高處執行查詢——不同的索引定義只會根據路徑限制產生不同的行為。


3. **所有可用分析器的清單在哪裡？**

   Oak公開一組lucene-provides分析器配置元素，用於AEM。

   * [Apache Oak Analyzers檔案](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [濾鏡](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在相同查詢中搜尋頁面和資產？**

   6.3AEM中的新功能，是能夠在同一個提供的查詢中查詢多個節點類型。 下列QueryBuilder查詢。 請注意，每個&quot;sub-query&quot;都可解析為自己的索引，因此在此範例中，`cq:Page`子查詢會解析為`/oak:index/cqPageLucene`，而`dam:Asset`子查詢會解析為`/oak:index/damAssetLucene`。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   結果：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   透過[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)探索查詢和結果。

5. **如何在同一查詢中搜尋多個路徑？**

   6.3AEM中的新功能，是能夠在同一個提供的查詢中跨多個路徑進行查詢。 下列QueryBuilder查詢。 請注意，每個&quot;sub-query&quot;都可解析為其自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   在以下查詢和查詢計畫中生成結果

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   透過[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)探索查詢和結果。
