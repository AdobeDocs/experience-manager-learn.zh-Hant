---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了多項功能，可管理AEM中的Oak索引、收集索引統計資料、執行索引一致性檢查，以及重新/索引索引本身。
version: 6.4, 6.5
feature: 搜尋
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: 效能
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了一些功能，以管理AEM中的 [!DNL Oak]200個索引，包括收集索引統計資訊、運行索引一致性檢查以及重新/索引索引本身。

>[!NOTE]
>
>在本文和影片中，詞語索引和重新索引可交互使用，並考慮同一操作。

## [!DNL oak-run.jar] index命令基本資訊

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必須符合AEM例項上使用的Oak版本。
* 使用[!DNL oak-run.jar]管理索引時，會利用&#x200B;**[!DNL index]**&#x200B;命令及各種標幟，以支援不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 轉儲離線分析的所有索引定義、重要索引統計資訊和索引內容。
* 索引統計資料收集在使用中的AEM例項上執行是安全的。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 一致性檢查在使用中的AEM例項上執行是安全的，用於一致性檢查級別1和2。

## 使用[!DNL oak-run.jar]建立TarMK線上索引 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]線上索引[!DNL TarMK]比在`oak:queryIndexDefinition`節點上設定`reindex=true`更快。 儘管效能有所提高，但使用[!DNL oak-run.jar]的聯機索引仍需要維護窗口才能執行索引。

* 使用[!DNL oak-run.jar]的[!DNL TarMK]線上索引應對AEM例項維護視窗外的AEM例項執行&#x200B;**not**。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行離線索引是[!DNL TarMK]最簡單的基於[!DNL oak-run.jar]的索引方法，因為它需要單個[!DNL oak-run.jar]命令，但它需要關閉AEM實例。

## 使用oak-run.jar進行TarMK帶外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上建立帶外索引，可將索引對使用中AEM例項的影響降至最低。
* 帶外索引是AEM安裝的建議索引方法，其中重新/索引的時間超過可用的維護時間。

## 使用oak-run.jar建立MongoMK線上索引

* 在[!DNL MongoMK]和[!DNL RDBMK]上具有[!DNL oak-run.jar]的線上索引是重新/索引[!DNL MongoMK]（和[!DNL RDBMK]）AEM安裝的建議方法。 **或不應使用其他 [!DNL MongoMK] 方 [!DNL RDBMK]法。**
* 此索引只需對叢集中的單一AEM例項執行。
* [!DNL MongoMK]的線上索引對於正在運行的AEM群集是安全的，因為儲存庫遍歷將僅發生在單個[!DNL MongoDB]節點上，這樣其他節點就可以繼續服務請求，而不會對效能產生重大影響。

用於執行[!DNL MongoMK]聯機索引的[!DNL oak-run.jar]索引命令與 [!DNL TarMK] 具有 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)的「聯機索引」的[相同，其差異在於段儲存參數指向包含節點儲存的[!DNL MongoDB]實例。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支援材料

* [下載 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *確認下載的版本符合AEM上安裝的Oak版本，如上所述*
* [Apache Jackrabbit Oak-run.jar索引命令檔案](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
