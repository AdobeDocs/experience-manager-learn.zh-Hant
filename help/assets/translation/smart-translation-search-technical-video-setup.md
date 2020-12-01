---
title: 使用AEM資產設定智慧型翻譯搜尋
seo-title: 使用AEM資產設定智慧型翻譯搜尋
description: 智慧型翻譯搜尋可讓使用非英文搜尋詞解析為英文內容。 若要設定智慧型轉譯搜尋的AEM，必須安裝並設定Apache Oak Search Machine Translation OSGi套件，以及包含轉譯規則的相關免費開放原始碼Apache Joshua語言套件。
seo-description: 智慧型翻譯搜尋可讓使用非英文搜尋詞解析為英文內容。 若要設定智慧型轉譯搜尋的AEM，必須安裝並設定Apache Oak Search Machine Translation OSGi套件，以及包含轉譯規則的相關免費開放原始碼Apache Joshua語言套件。
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# 使用AEM Assets設定智慧型翻譯搜尋{#set-up-smart-translation-search-with-aem-assets}

智慧型翻譯搜尋可讓使用非英文搜尋詞解析為英文內容。 若要設定智慧型轉譯搜尋的AEM，必須安裝並設定Apache Oak Search Machine Translation OSGi套件，以及包含轉譯規則的相關免費開放原始碼Apache Joshua語言套件。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>智慧型轉譯搜尋必須設定在每個需要的AEM例項上。

1. 下載並安裝Oak Search Machine Translation OSGi套件
   * [下載與AEM的Oak版本相](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 對應的Oak Search Machine Translation OSGi bundle。
   * 透過[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)將下載的Oak Search Machine Translation OSGi套件安裝至AEM。

2. 下載並更新Apache Joshua語言套件
   * 下載並解壓縮所需的[Apache Joshua語言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)。
   * 編輯`joshua.config`檔案並注釋掉以下兩行：

      ```
      feature-function = LanguageModel ...
      ```

   * 確定並記錄語言包的模型資料夾的大小，因為這會影響AEM需要多少額外堆積空間。
   * 將解壓縮的Apache Joshua語言包資料夾（使用`joshua.config`編輯）移至

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更新的堆積記憶體配置重新啟動AEM
   * 停止AEM
   * 決定AEM的新必要堆大小

      * AEM的前語言——缺乏堆大小+模型目錄的大小四捨五入至最接近的2GB
      * 例如：如果預先語言封裝的AEM安裝需要8GB的堆疊才能執行，而語言封裝的模型資料夾是3.8GB的未壓縮檔案，則新的堆疊大小為：

         原始`8GB` +（`3.75GB`四捨五入到最接近`2GB`的`4GB`），總共`12GB`
   * 驗證電腦是否有此量的額外可用記憶體。
   * 更新AEM的啟動指令碼，以因應新堆積大小進行調整

      * 例如`java -Xmx12g -jar cq-author-p4502.jar`
   * 以增加的堆大小重新啟動AEM。

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

   * 對於每個語言套件，[會透過AEM Web Console的「設定管理員」建立新的Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuration](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)。

      * `Joshua Config Path` 是joshua.config檔案的絕對路徑。AEM程式必須能夠讀取語言套件資料夾中的所有檔案。
      * `Node types` 是候選節點類型，其全文搜索將與此語言包交互進行翻譯。
      * `Minimum score` 是要使用的翻譯詞語的最低信賴分數。

         * 例如，hombre（西班牙文表示&quot;man&quot;）可翻譯成信賴分數`0.9`的英文單字&quot;man&quot;，也可翻譯成信賴分數`0.2`的英文單字&quot;human&quot;。 將最小分數調整為`0.3`，會將&quot;hombre&quot;保留為&quot;man&quot;，但會捨棄&quot;hombre&quot;為&quot;human&quot;的翻譯，因為此`0.2`的翻譯分數小於`0.3`的最小分數。

5. 對資產執行全文搜尋
   * 由於dam:Asset是重新註冊此語言套件的節點類型，因此我們必須使用全文搜尋來搜尋AEM Assets以驗證此點。
   * 導覽至「AEM >資產」並開啟Omnisearch。 搜尋已安裝語言套件的語言詞語。
   * 視需要調整OSGi組態中的「最低分數」，以確保結果的準確性。

6. 更新語言套件
   * Apache Joshua語言包由Apache Joshua項目維護，其更新或更正是Apache Joshua項目的酌情決定。
   * 如果語言套件已更新，若要在AEM中安裝更新，則必須遵循上述步驟2 - 4，視需要調整堆積大小。

      * 請注意，將解壓縮的語言套件移至crx-quickstart/opt檔案夾時，請先移動任何現有的語言套件檔案夾，然後再在新檔案夾中複製。
   * 如果AEM不需要重新啟動，則與更新語言套件相關的相關Apache Jackrabbit Oak Machien Translation Fulltext Query Terms Provider OSGi設定必須重新儲存，以便AEM處理更新的檔案。


## 更新damAssetLucene索引{#updating-damassetlucene-index}

為了讓[AEM智慧型標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)受AEM智慧型轉譯影響，必須更新AEM的`/oak   :index  /damAssetLucene`索引，以將預計的標籤（「智慧型標籤」的系統名稱）標示為資產的匯總Lucene索引的一部分。

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
* [AEM智慧型標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查詢和索引的最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)