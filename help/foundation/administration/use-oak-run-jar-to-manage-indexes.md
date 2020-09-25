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
source-wordcount: '451'
ht-degree: 0%

---


# 使用oak-run.jar來管理索引

[!DNL oak-run.jar]&#39;s index命令整合了許多功能，以管理AEM中的 [!DNL Oak]200個索引，從收集索引統計資料、執行索引一致性檢查，以及重新／索引索引索引本身。

>[!NOTE]
>
>在本文和視頻中，詞語索引和重新索引可互換使用，並視為同一操作。

## [!DNL oak-run.jar] 索引命令基礎

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的 [[!DNL oak-run.jar]版本必須符合AEM例項上使用的Oak版本](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 。
* 使用帶有各 [!DNL oak-run.jar] 種標誌的 **[!DNL index]** 命令管理索引，以支援不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 轉儲所有索引定義、重要索引統計資訊和索引內容，以便離線分析。
* 索引統計資料收集可安全地在使用中的AEM例項上執行。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 一致性檢查是安全的，可在使用中的AEM例項上執行，以取得一致性檢查層級1和2。

## TarMK Online索引與 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用的聯 [!DNL TarMK] 機索 [!DNL oak-run.jar] 引比在節點上 `reindex=true` 設定更快 `oak:queryIndexDefinition` 。 儘管效能有所提高，但使用聯機索 [!DNL oak-run.jar] 引仍需要一個維護窗口來執行索引。

* 在AEM的例項 [!DNL TarMK] 維護 [!DNL oak-run.jar] 視窗外，不 **** 應針對AEM例項執行使用的線上索引。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 離線索引使 [!DNL TarMK] 用是最簡 [!DNL oak-run.jar] 單的索引方法，因為它需 [!DNL oak-run.jar] 要單一命令，但 [!DNL TarMK][!DNL oak-run.jar] 它需要關閉AEM例項。

## 使用oak-run.jar建立TarMK帶外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用帶外索引功能可將索 [!DNL TarMK] 引功 [!DNL oak-run.jar] 能對使用中AEM例項的影響降至最低。
* 帶外索引是AEM安裝的建議索引方法，其中重新／索引的時間超過可用的維護視窗。

## MongoMK Online索引與oak-run.jar

* Online index with [!DNL oak-run.jar] on [!DNL MongoMK] and [!DNL RDBMK] 是重新／索引（和）AEM安裝 [!DNL MongoMK] 的建議 [!DNL RDBMK]方法。 **或不應使用其他方[!DNL MongoMK]法[!DNL RDBMK]。**
* 此索引必須僅針對叢集中的單一AEM例項執行。
* 線上索引可 [!DNL MongoMK][!DNL MongoDB] 以安全地對正在運行的AEM群集執行，因為儲存庫遍歷將僅發生在單個節點上，使其他節點能夠繼續服務請求，而不會對效能產生顯著影響。

執 [!DNL oak-run.jar] 行聯機索引的index命令與Online索引相 [!DNL MongoMK] 同，其 [差異在於段儲存參數指向包含Node儲存的 [!DNL TarMK]  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)[!DNL MongoDB] 實例。

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
