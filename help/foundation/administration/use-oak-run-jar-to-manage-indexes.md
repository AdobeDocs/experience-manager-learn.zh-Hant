---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了多項功能，用於管理中的Oak索引AEM，包括收集索引統計資訊、運行索引一致性檢查以及索引本身的重新/索引。
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

[!DNL oak-run.jar]&#39;s index命令合併了要管理的多個功能 [!DNL Oak]中的200個索AEM引，包括收集索引統計資訊、運行索引一致性檢查和索引本身的重新/索引。

>[!NOTE]
>
>在本文和視頻中，術語索引和重新索引可互換使用，並且被視為相同的操作。

## [!DNL oak-run.jar] 索引命令基礎

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 版本 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 使用的版本必須與實例上使用的Oak版本AEM匹配。
* 使用 [!DNL oak-run.jar] 利用 **[!DNL index]** 命令，可支援不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引統計資訊

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 轉儲離線分析的所有索引定義、重要索引統計資訊和索引內容。
* 索引統計資訊收集對於在用實例是安全的AEM。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` 快速確定lucene Oak索引是否已損壞。
* 一致性檢查在一致性檢查級別1和2的使AEM用中實例上運行是安全的。

## TarMK聯機索引 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 聯機索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 比設定快 `reindex=true` 的 `oak:queryIndexDefinition` 的下界。 儘管效能提高，但使用 [!DNL oak-run.jar] 仍需要維護窗口才能執行索引。

* 聯機索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 應該 **不** 在實例維AEM護窗口AEM外執行。

## 使用oak-run.jar的TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 離線索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 最簡單 [!DNL oak-run.jar] 基於索引的方法 [!DNL TarMK] 因為它需要 [!DNL oak-run.jar] 命令，但需要關AEM閉實例。

## TarMK帶外索引與oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 帶外索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 最大限度地減少索引對使用中實例的AEM影響。
* 帶外索引是建議的索引方法，用於AEM在重新/索引時間超過可用維護窗口的安裝。

## MongoMK使用oak-run.jar的聯機索引

* 聯機索引 [!DNL oak-run.jar] 上 [!DNL MongoMK] 和 [!DNL RDBMK] 是推薦的重新索引方法 [!DNL MongoMK] (和 [!DNL RDBMK])安AEM裝。 **不應使用其他方法 [!DNL MongoMK] 或 [!DNL RDBMK]。**
* 此索引只需針對群集中的AEM單個實例執行。
* 聯機索引 [!DNL MongoMK] 對正在運行的群集執行AEM是安全的，因為儲存庫遍歷只發生在一個 [!DNL MongoDB] 節點，允許其他節點繼續服務請求，而不會對效能產生重大影響。

的 [!DNL oak-run.jar] index命令執行聯機索引 [!DNL MongoMK] 是 [與 [!DNL TarMK] 使用 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 與段儲存參數指向 [!DNL MongoDB] 包含節點儲存的實例。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支撐材料

* [下載 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *確保下載的版本與上述安裝的AEMOak版本匹配*
* [Apache Jackrabbit Oak-run.jar索引命令文檔](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
