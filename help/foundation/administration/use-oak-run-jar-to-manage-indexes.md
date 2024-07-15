---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令可整合許多功能來管理AEM中的Oak索引，包括收集索引統計資料、執行索引一致性檢查以及重新索引索引本身。
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# 使用oak-run.jar管理索引

[!DNL oak-run.jar]的index命令整合許多功能以管理AEM中的[!DNL Oak]200個索引，包括收集索引統計資料、執行索引一致性檢查以及重新索引索引本身。

>[!NOTE]
>
>在本文與影片中，索引與重新索引這兩個詞可互換使用，且被視為相同的操作。

## [!DNL oak-run.jar] index命令基本需知

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必須與AEM執行個體上使用的Oak版本相符。
* 使用[!DNL oak-run.jar]管理索引時，會利用具有各種旗標的&#x200B;**[!DNL index]**&#x200B;命令來支援不同的作業。

   * `java -jar oak-run*.jar index ...`

## 索引統計資料

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar`會傾印所有索引定義、重要索引統計資料和索引內容，以供離線分析。
* 可以在使用中的AEM執行個體上安全地執行索引統計資料收集。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar`會快速判斷lucene Oak索引是否損毀。
* 一致性檢查可在使用中的AEM執行個體上安全執行以進行一致性檢查層級1和2。

## 使用[!DNL oak-run.jar]建立TarMK線上索引 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 使用[!DNL oak-run.jar]建立[!DNL TarMK]的線上索引比在`oak:queryIndexDefinition`節點上設定`reindex=true`更快。 儘管效能提升，使用[!DNL oak-run.jar]的線上索引仍需要維護視窗來執行索引。

* 使用[!DNL oak-run.jar]的[!DNL TarMK]線上索引應&#x200B;**不**&#x200B;針對AEM執行個體維護期間以外的AEM執行個體執行。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 使用[!DNL oak-run.jar]對[!DNL TarMK]進行離線索引是[!DNL TarMK]最簡單的[!DNL oak-run.jar]型索引方法，因為它需要單一[!DNL oak-run.jar]命令，但需要關閉AEM執行個體。

## 使用oak-run.jar編制的TarMK頻外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上編制頻外索引可將編制索引對使用中AEM執行個體造成的影響降至最低。
* 頻外索引是建議的AEM安裝索引方法，當重新編列/索引的時間超過可用的維護時段時。

## 使用oak-run.jar建立MongoMK線上索引

* [!DNL MongoMK]和[!DNL RDBMK]上有[!DNL oak-run.jar]的線上索引是重新索引[!DNL MongoMK] （和[!DNL RDBMK]） AEM安裝的建議方法。 **[!DNL MongoMK]或[!DNL RDBMK]不應使用其他方法。**
* 此索引只需要針對叢集中的單一AEM執行個體執行。
* [!DNL MongoMK]的線上索引可針對執行中的AEM叢集安全執行，因為存放庫周遊只會發生在單一[!DNL MongoDB]節點上，而允許其他節點繼續提供要求而不會對效能造成重大影響。

執行[!DNL MongoMK]線上索引的[!DNL oak-run.jar]索引命令與 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)的 [!DNL TarMK] 線上索引相同，不同之處在於區段存放區引數指向包含節點存放區的[!DNL MongoDB]執行個體。[

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支援材料

* [下載 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *請確定下載的版本符合如上所述安裝在AEM上的Oak版本*
* [Apache Jackrabbit Oak oak-run.jar索引命令檔案](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
