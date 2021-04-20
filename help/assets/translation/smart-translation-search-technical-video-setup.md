---
title: 設定智慧翻譯搜索與AEM Assets
description: 智慧型翻譯搜尋可讓使用非英文搜尋詞解析為英文內容。 要設定智AEM能翻譯搜索，必須安裝和配置Apache Oak Search Machine Translation OSGi包，以及包含翻譯規則的相關免費和開放原始碼的Apache Joshua語言包。
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 0%

---


# 使用AEM Assets設定智慧翻譯搜索{#set-up-smart-translation-search-with-aem-assets}

智慧型翻譯搜尋可讓使用非英文搜尋詞解析為英文內容。 要設定智AEM能翻譯搜索，必須安裝和配置Apache Oak Search Machine Translation OSGi包，以及包含翻譯規則的相關免費和開放原始碼的Apache Joshua語言包。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>必須在每個需要智慧翻譯的實例AEM上設定智慧翻譯搜索。

1. 下載並安裝Oak Search Machine Translation OSGi套件
   * [下載與Oak版本對應的Oak Search Machine Translation OSGi ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) bundle。
   * 透過[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)將下載的Oak Search Machine Translation OSGiAEM套件安裝至。

2. 下載並更新Apache Joshua語言套件
   * 下載並解壓縮所需的[Apache Joshua語言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)。
   * 編輯`joshua.config`檔案並注釋掉以下兩行：

      ```
      feature-function = LanguageModel ...
      ```

   * 確定並記錄語言包的模型資料夾的大小，因為這會影響需要多少額外堆AEM空間。
   * 將解壓縮的Apache Joshua語言包資料夾（使用`joshua.config`編輯）移至

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更AEM新的堆記憶體分配重新啟動
   * 停AEM止
   * 確定新的所需堆大小AEM

      * AEMpre language-lash堆大小+ model目錄的大小四捨五入到最接近的2GB
      * 例如：如果預先語言包AEM的安裝需要8GB的堆才能運行，而語言包的模型資料夾是3.8GB的未壓縮，則新堆大小為：

         原始`8GB` +（`3.75GB`四捨五入到最接近`2GB`的`4GB`），總共`12GB`
   * 驗證電腦是否有此量的額外可用記憶體。
   * 更新AEM啟動指令碼以調整新堆大小

      * 例如`java -Xmx12g -jar cq-author-p4502.jar`
   * 以增加AEM的堆大小重新啟動。

   >[!NOTE]
   >
   >語言包所需的堆空間可能會變大，尤其是當使用多語言包時。
   >
   >
   >請務必確保&#x200B;**實例具有足夠的記憶體**&#x200B;以容納已分配堆空間的增加。
   >
   >
   >必須始終計算&#x200B;**基本堆，以支援可接受的效能，而不安裝任何語言包**。

4. 透過Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi組態註冊語言套件

   * 對於每個語言包，[通過AEMWeb Console的「配置」管理器建立新的Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuration](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)。

      * `Joshua Config Path` 是joshua.config檔案的絕對路徑。該AEM進程必須能夠讀取語言包資料夾中的所有檔案。
      * `Node types` 是候選節點類型，其全文搜索將與此語言包交互進行翻譯。
      * `Minimum score` 是要使用的翻譯詞語的最低信賴分數。

         * 例如，hombre（西班牙文表示&quot;man&quot;）可翻譯成信賴分數`0.9`的英文單字&quot;man&quot;，也可翻譯成信賴分數`0.2`的英文單字&quot;human&quot;。 將最小分數調整為`0.3`，會將&quot;hombre&quot;保留為&quot;man&quot;，但會捨棄&quot;hombre&quot;為&quot;human&quot;的翻譯，因為此`0.2`的翻譯分數小於`0.3`的最小分數。

5. 對資產執行全文搜尋
   * 由於dam:Asset是重新註冊此語言包的節點類型，因此我們必須使用全文搜索來搜索AEM Assets，以驗證此點。
   * 導覽至「AEM>資產」並開啟Omnisearch。 搜尋已安裝語言套件的語言詞語。
   * 視需要調整OSGi組態中的「最低分數」，以確保結果的準確性。

6. 更新語言套件
   * Apache Joshua語言包由Apache Joshua項目維護，其更新或更正是Apache Joshua項目的酌情決定。
   * 如果語言包已更新，為了安裝更新，AEM必須遵循上述步驟2 - 4，視需要調整堆大小。

      * 請注意，將解壓縮的語言套件移至crx-quickstart/opt檔案夾時，請先移動任何現有的語言套件檔案夾，然後再在新檔案夾中複製。
   * 如AEM果不需要重新啟動，則必須重新儲存與更新語言包相關的相關Apache Jackrabbit Oak Machine翻譯全文查詢詞語提供者OSGi配置，以便AEM處理更新的檔案。


## 更新damAssetLucene索引{#updating-damassetlucene-index}

要使[智AEM慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)受智慧轉譯影AEM響，必須更新AEM`/oak   :index  /damAssetLucene`索引，以將預計的標籤（「智慧標籤」的系統名稱）標示為資產匯整Lucene索引的一部分。

在`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`下，確保配置如下：

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## 其他資源{#additional-resources}

* [Apache Oak Search Machine Translation OSGi bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [智AEM慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查詢和索引的最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)