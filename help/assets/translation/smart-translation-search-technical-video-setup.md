---
title: 使用AEM Assets設定智慧翻譯搜尋
description: 智慧翻譯搜尋可讓您使用非英文的搜尋辭彙來解析為英文內容。 若要設定AEM以進行智慧型翻譯搜尋，必須安裝並設定Apache Oak Search Machine Translation OSGi套件組合，以及包含翻譯規則的相關免費開放原始碼Apache Joshua語言套件。
version: 6.3, 6.4, 6.5
feature: 搜尋
topic: 內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 0%

---


# 使用AEM Assets{#set-up-smart-translation-search-with-aem-assets}設定智慧翻譯搜尋

智慧翻譯搜尋可讓您使用非英文的搜尋辭彙來解析為英文內容。 若要設定AEM以進行智慧型翻譯搜尋，必須安裝並設定Apache Oak Search Machine Translation OSGi套件組合，以及包含翻譯規則的相關免費開放原始碼Apache Joshua語言套件。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>必須在需要的智慧型翻譯例項上設定智慧型翻譯搜尋。

1. 下載及安裝Oak Search機器翻譯OSGi套件組合
   * [下載與AEM Oak版本對應](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 的Oak Search機器翻譯OSGi套件組合。
   * 透過[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)將下載的Oak Search機器轉譯OSGi套件組合安裝至AEM。

2. 下載和更新Apache Joshua語言套件
   * 下載並解壓縮所需的[Apache Joshua語言套件](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)。
   * 編輯`joshua.config`檔案並注釋掉以下開頭的2行：

      ```
      feature-function = LanguageModel ...
      ```

   * 確定並記錄語言包的模型資料夾的大小，因為這會影響AEM需要多少額外的堆空間。
   * 將解壓縮的Apache Joshua語言包資料夾（連同`joshua.config`編輯內容）移至

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更新的堆記憶體分配重新啟動AEM
   * 停止AEM
   * 確定AEM的新所需堆大小

      * AEM前置語言 — 缺少堆大小+模型目錄的大小捨入到最接近的2GB
      * 例如：如果預先語言包安裝AEM需要8GB的堆才能運行，並且語言包的模型資料夾未壓縮為3.8GB，則新堆大小為：

         原始的`8GB` +（`3.75GB`捨入到最接近的`2GB`，即`4GB`），總計為`12GB`
   * 驗證電腦是否具有此數量的額外可用記憶體。
   * 更新AEM啟動指令碼以根據新堆大小進行調整

      * 例如`java -Xmx12g -jar cq-author-p4502.jar`
   * 以增加的堆大小重新啟動AEM。

   >[!NOTE]
   >
   >語言包所需的堆空間可能會增大，尤其是當使用多語言包時。
   >
   >
   >請始終確保實例&#x200B;**有足夠的記憶體**&#x200B;以適應已分配堆空間的增加。
   >
   >
   >必須始終計算&#x200B;**基本堆以支援可接受的效能，而不安裝任何語言包**。

4. 透過Apache Jackrabbit Oak Machine Translation全文查詢詞提供者OSGi設定註冊語言套件

   * 對於每個語言套件， [透過AEM Web主控台的「設定管理員」，建立新的Apache Jackrabbit Oak Machine Translation全文查詢詞提供者OSGi設定](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)。

      * `Joshua Config Path` 是joshua.config檔案的絕對路徑。AEM程式必須能夠讀取語言包資料夾中的所有檔案。
      * `Node types` 是候選節點類型，其全文搜索將使此語言包參與翻譯。
      * `Minimum score` 是要使用的翻譯詞語的最低信賴分數。

         * 例如，hombre（&quot;man&quot;的西班牙文）可翻譯成信賴分數為`0.9`的英文單詞&quot;man&quot;，也可翻譯成信賴分數為`0.2`的英文單詞&quot;human&quot;。 將最小分數調整為`0.3`，將保留「hombre」到「man」的翻譯，但將「hombre」轉換為「human」，因為此`0.2`的翻譯分數小於`0.3`的最小分數。

5. 對資產執行全文搜尋
   * 因為dam:Asset是重新註冊此語言套件的節點類型，因此我們必須使用全文搜尋來搜尋AEM Assets，才能驗證這一點。
   * 導覽至「AEM >資產」 ，然後開啟Omnisearch。 以安裝語言包的語言搜索詞。
   * 視需要調整OSGi設定中的最小分數，以確保結果的準確性。

6. 更新語言套件
   * Apache Joshua語言包由Apache Joshua項目全部維護，其更新或更正是Apache Joshua項目的酌處權。
   * 如果語言包已更新，為了在AEM中安裝更新，必須執行上述步驟2 - 4，根據需要向上或向下調整堆大小。

      * 請注意，將解壓縮的語言包移至crx-quickstart/opt資料夾時，請先移動任何現有的語言包資料夾，再在新檔案中複製。
   * 如果AEM不需要重新啟動，則與更新語言包相關的相關Apache Jackrabbit Oak Machien翻譯全文查詢詞提供者OSGi設定必須重新儲存，以便AEM處理更新的檔案。


## 更新damAssetLucene索引{#updating-damassetlucene-index}

為了讓[AEM智慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)受到AEM智慧轉譯的影響，必須更新AEM `/oak   :index  /damAssetLucene`索引，以將預計標籤（「智慧標籤」的系統名稱）標示為資產匯總Lucene索引的一部分。

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

* [Apache Oak Search Machine Translation OSGi套件組合](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM智慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查詢和索引的最佳實務](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)