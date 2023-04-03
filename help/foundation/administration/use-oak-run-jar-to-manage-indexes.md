---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了多項功能，可管理AEM中的Oak索引、收集索引統計資料、執行索引一致性檢查，以及重新/索引索引本身。
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了要管理的多個功能 [!DNL Oak]AEM中有200個索引，包括收集索引統計資料、運行索引一致性檢查，以及索引本身的重新/索引。

>[!NOTE]
>
>在本文和影片中，詞語索引和重新索引可交互使用，並考慮同一操作。

## [!DNL oak-run.jar] index命令基本資訊

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 版本 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 使用的必須符合AEM例項上使用的Oak版本。
* 使用管理索引 [!DNL oak-run.jar] 利用 **[!DNL index]** 命令，包含各種標誌，以支援不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 轉儲離線分析的所有索引定義、重要索引統計資訊和索引內容。
* 索引統計資料收集在使用中的AEM例項上執行是安全的。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 一致性檢查在使用中的AEM例項上執行是安全的，用於一致性檢查級別1和2。

## TarMK線上索引與 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 線上索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 快於設定 `reindex=true` 在 `oak:queryIndexDefinition` 節點。 儘管效能有所提高，但使用 [!DNL oak-run.jar] 仍需要維護窗口才能執行索引。

* 線上索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] show **not** 會在AEM例項維護視窗外的AEM例項上執行。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 離線索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 是最簡單的 [!DNL oak-run.jar] 基於索引的方法 [!DNL TarMK] 因為它需要單一 [!DNL oak-run.jar] 命令，但需要關閉AEM執行個體。

## 使用oak-run.jar進行TarMK帶外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 帶外索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 索引對使用中AEM例項的影響降至最低。
* 帶外索引是AEM安裝的建議索引方法，其中重新/索引的時間超過可用的維護時間。

## 使用oak-run.jar建立MongoMK線上索引

* 線上索引，具有 [!DNL oak-run.jar] on [!DNL MongoMK] 和 [!DNL RDBMK] 是重新索引的推薦方法 [!DNL MongoMK] (和 [!DNL RDBMK])AEM安裝。 **不應對 [!DNL MongoMK] 或 [!DNL RDBMK].**
* 此索引只需對叢集中的單一AEM例項執行。
* 線上索引 [!DNL MongoMK] 對執行中的AEM叢集執行是安全的，因為存放庫周遊只會發生在單一 [!DNL MongoDB] 節點，允許其他用戶繼續提供請求，而不會對效能產生重大影響。

此 [!DNL oak-run.jar] index命令，用於執行 [!DNL MongoMK] 是 [與 [!DNL TarMK] 使用 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 區段儲存參數指向 [!DNL MongoDB] 包含Node儲存區的例項。

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
