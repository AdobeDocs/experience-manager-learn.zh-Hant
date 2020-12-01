---
title: 使用oak-run.jar來管理索引
description: oak-run.jar的index命令整合了許多功能，以管理AEM中的Oak索引，從收集索引統計資料、執行索引一致性檢查，以及重新／索引索引索引本身。
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# 使用oak-run.jar來管理索引

[!DNL oak-run.jar]&#39;s index命令整合了許多功能，以管理AEM中的 [!DNL Oak]200個索引，從收集索引統計資料、執行索引一致性檢查，以及重新／索引索引索引本身。

>[!NOTE]
>
>在本文和視頻中，詞語索引和重新索引可互換使用，並視為同一操作。

## [!DNL oak-run.jar] 索引命令基礎

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必須符合AEM例項上使用的Oak版本。
* 使用[!DNL oak-run.jar]管理索引使用&#x200B;**[!DNL index]**&#x200B;命令及各種標幟，以支援不同的作業。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 轉儲所有索引定義、重要索引統計資訊和索引內容，以便離線分析。
* 索引統計資料收集可安全地在使用中的AEM例項上執行。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 一致性檢查是安全的，可在使用中的AEM例項上執行，以取得一致性檢查層級1和2。

## 使用[!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}建立TarMK線上索引

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行聯機索引比在`oak:queryIndexDefinition`節點上設定`reindex=true`更快。 儘管效能有所提高，但使用[!DNL oak-run.jar]的聯機索引仍然需要一個維護窗口來執行索引。

* 使用[!DNL oak-run.jar]的[!DNL TarMK]線上索引應&#x200B;**not**&#x200B;在AEM的例項維護視窗外針對AEM例項執行。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行離線索引是[!DNL TarMK]最簡單的[!DNL oak-run.jar]索引方法，因為它需要單一[!DNL oak-run.jar]命令，但是它需要關閉AEM實例。

## 使用oak-run.jar建立TarMK帶外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上建立帶外索引，可將索引對使用中AEM例項的影響降至最低。
* 帶外索引是AEM安裝的建議索引方法，其中重新／索引的時間超過可用的維護視窗。

## MongoMK Online索引與oak-run.jar

* 在[!DNL MongoMK]和[!DNL RDBMK]上具有[!DNL oak-run.jar]的線上索引是重新／索引[!DNL MongoMK]（和[!DNL RDBMK]）AEM安裝的建議方法。 **或不應使用其他方 [!DNL MongoMK] 法 [!DNL RDBMK]。**
* 此索引必須僅針對叢集中的單一AEM例項執行。
* [!DNL MongoMK]的線上索引可安全地針對執行中的AEM叢集執行，因為資料庫周遊將僅發生在單一[!DNL MongoDB]節點上，讓其他節點繼續提供要求，而不會大幅影響效能。

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
   * *請確定下載的版本與AEM上安裝的Oak版本相符，如上所述*
* [Apache Jackrabbit Oak-run.jar索引命令檔案](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
