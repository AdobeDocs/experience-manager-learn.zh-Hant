---
title: 與AEM Assets建立智慧翻譯搜索
description: 智慧翻譯搜索允許使用非英文搜索詞來解析英文內容。 要設定智慧AEM翻譯搜索，必須安裝和配置Apache Oak Search Machine Translation OSGi捆綁包，以及包含翻譯規則的相關免費和開源的Apache Joshua語言包。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# 與AEM Assets建立智慧翻譯搜索{#set-up-smart-translation-search-with-aem-assets}

智慧翻譯搜索允許使用非英文搜索詞來解析英文內容。 要設定智慧AEM翻譯搜索，必須安裝和配置Apache Oak Search Machine Translation OSGi捆綁包，以及包含翻譯規則的相關免費和開源的Apache Joshua語言包。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>必須在每個需要智慧翻譯的實AEM例上設定智慧翻譯搜索。

1. 下載並安裝Oak Search Machine Translation OSGi捆綁包
   * [下載Oak Search Machine翻譯OSGi捆綁包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 對應橡AEM木版。
   * 將下載的Oak Search Machine Translation OSGi捆綁包安裝AEM到 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles)。

2. 下載和更新Apache Joshua語言包
   * 下載並解壓縮所需 [Apache Joshua語言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)。
   * 編輯 `joshua.config` 檔案並注釋掉以下2行開頭：

      ```
      feature-function = LanguageModel ...
      ```

   * 確定並記錄語言包的模型資料夾的大小，因為這會影響所需的額外堆AEM空間量。
   * 移動已解壓的Apache Joshua語言包資料夾(使用 `joshua.config` 編輯：

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

      * 預語AEM言 — 缺少堆大小+模型目錄大小捨入到最接近的2GB
      * 例如：如果預語言包AEM安裝需要8GB的堆才能運行，而語言包的模型資料夾未壓縮3.8GB，則新堆大小為：

         原始 `8GB` +( `3.75GB` 四捨五入到最接近的 `2GB`，即 `4GB`)，共 `12GB`
   * 驗證電腦是否具有此數量的額外可用記憶體。
   * 更新AEM啟動指令碼以適應新堆大小

      * 前。 `java -Xmx12g -jar cq-author-p4502.jar`
   * 以增加AEM的堆大小重新啟動。

   >[!NOTE]
   >
   >語言包所需的堆空間可能會增大，尤其是當使用多語言包時。
   >
   >
   >總是確保 **實例有足夠的記憶體** 以適應已分配堆空間的增加。
   >
   >
   >的 **必須始終計算基堆以支援可接受的效能，而不需要任何語言包** 已安裝。

4. 通過Apache Jackrabbit Oak機器翻譯全文查詢術語提供程式OSGi配置註冊語言包

   * 對於每個語言包， [建立新的Apache Jackrabbit Oak機器翻譯全文查詢術語提供程式OSGi配置](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) 通過AEMWeb控制台的配置管理器。

      * `Joshua Config Path` 是joshua.config檔案的絕對路徑。 進AEM程必須能夠讀取語言包資料夾中的所有檔案。
      * `Node types` 是候選節點類型，其全文搜索將與此語言包進行翻譯。
      * `Minimum score` 是要使用的已翻譯術語的最小置信度分數。

         * 例如，hombre（西班牙語中&quot;man&quot;）可翻譯成英語單詞&quot;man&quot;，置信分數為 `0.9` 並翻譯成&quot;human&quot;這個詞，並帶有信心分數 `0.2`。 將最小分數調整為 `0.3`，將&quot;hombre&quot;與&quot;man&quot;的翻譯保留，但將&quot;hombre&quot;與&quot;human&quot;的翻譯作為該翻譯分數 `0.2` 小於 `0.3`。

5. 對資產執行全文搜索
   * 因為：Asset是重新註冊此語言包的節點類型，我們必須使用全文搜索搜索來搜索AEM Assets以驗證這一點。
   * 定位至「AEM資產」並開啟Omnisearch。 以已安裝語言包的語言搜索術語。
   * 根據需要，調整OSGi配置中的最小分數以確保結果的準確性。

6. 正在更新語言包
   * Apache Joshua語言包由Apache Joshua項目維護完整，其更新或更正是Apache Joshua項目的酌處權。
   * 如果更新了語言包，則為了在中安裝更新AEM，必須執行上述步驟2 - 4，根據需要調整堆大小。

      * 請注意，將解壓縮的語言包移動到crx-quickstart/opt資料夾時，在複製到新檔案之前，請移動任何現有的語言包資料夾。
   * 如AEM果不需要重新啟動，則必須重新保存與更新語言包相關的相關Apache Jackrabbit Oak Machien翻譯全文查詢術語提供程式OSGi配置，以便AEM處理更新檔案。


## 更新damAssetLucene索引 {#updating-damassetlucene-index}

為了 [智AEM能標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) 受智慧翻譯AEM的影響AEM, `/oak   :index  /damAssetLucene` 必須更新索引以將預測標籤（「智慧標籤」的系統名稱）標籤為資產的聚合Lucene索引的一部分。

下 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`，確保配置如下：

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

* [Apache Oak搜索機器翻譯OSGi捆綁包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua語言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [智AEM能標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查詢和索引的最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
