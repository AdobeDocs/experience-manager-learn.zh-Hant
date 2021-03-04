---
title: 使用oak-run.jar來管理索引
description: oak-run.jar的index命令整合了許多功能，以管理中的Oak索引AEM，包括收集索引統計資料、執行索引一致性檢查以及重新／索引索引索引本身。
version: 6.4, 6.5
feature: 搜尋
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: 效能
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---


# 使用oak-run.jar來管理索引

[!DNL oak-run.jar]&#39;s index命令整合了許多功能，以管理 [!DNL Oak]200個索引AEM，包括收集索引統計資訊、運行索引一致性檢查和索引本身。

>[!NOTE]
>
>在本文和視頻中，詞語索引和重新索引可互換使用，並視為同一操作。

## [!DNL oak-run.jar] 索引命令基礎

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必須符合執行個體上使用的Oak版AEM本。
* 使用[!DNL oak-run.jar]管理索引使用&#x200B;**[!DNL index]**&#x200B;命令及各種標幟，以支援不同的作業。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 轉儲所有索引定義、重要索引統計資訊和索引內容，以便離線分析。
* 索引統計資訊收集在使用中實例上可以安全AEM執行。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 一致性檢查可安全地在使用中實例上運行，AEM用於一致性檢查級別1和2。

## 使用[!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}建立TarMK線上索引

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行聯機索引比在`oak:queryIndexDefinition`節點上設定`reindex=true`更快。 儘管效能有所提高，但使用[!DNL oak-run.jar]的聯機索引仍然需要一個維護窗口來執行索引。

* 使用[!DNL oak-run.jar]的[!DNL TarMK]的聯機索引應對例項維護窗口外的例項執行&#x200B;**not&lt;a3/AEM>AEM。**

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行離線索引是[!DNL TarMK]最簡單的[!DNL oak-run.jar]索引方法，因為它需要單一[!DNL oak-run.jar]命令，但是它需要關閉AEM實例。

## 使用oak-run.jar建立TarMK帶外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上建立帶外索引，可將索引對使用中例項的影AEM響降至最低。
* 對於重新／索引時間超過可用維護窗口的安AEM裝，建議使用帶外索引方法。

## MongoMK Online索引與oak-run.jar

* 在[!DNL MongoMK]和[!DNL RDBMK]上具有[!DNL oak-run.jar]的聯機索引是重新／索引[!DNL MongoMK]（和[!DNL RDBMK]）安裝的推薦方AEM法。 **或不應使用其他方 [!DNL MongoMK] 法 [!DNL RDBMK]。**
* 此索引只需對群集中的單個實例AEM執行。
* [!DNL MongoMK]的聯機索引可以安全地對正在運行的群集執行，AEM因為儲存庫遍歷將僅發生在單個[!DNL MongoDB]節點上，允許其他節點繼續服務請求而不會對效能產生顯著影響。

用於執行[!DNL MongoMK]聯機索引的[!DNL oak-run.jar]索引命令與 [!DNL TarMK] 具有 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)的「聯機索引」命令相同，其差異在於段儲存參數指向包含Node儲存的[!DNL MongoDB]實例。[

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支援材料

* [下載 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *請確定下載的版本與上述安裝的OakAEM版本相符*
* [Apache Jackrabbit Oak-run.jar索引命令檔案](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
