---
title: 在AEM Assets中使用中繼資料匯入和匯出
description: 了解如何使用Adobe Experience Manager Assets的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 2%

---

# 在AEM Assets中使用中繼資料匯入和匯出 {#metadata-import-and-export}

了解如何使用Adobe Experience Manager Assets的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。

## 中繼資料匯出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## 中繼資料匯入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 準備要匯入的CSV檔案時，使用「中繼資料匯出」功能，可更輕鬆地產生包含資產清單的CSV。 然後，您可以修改產生的CSV檔案，並使用「匯入」功能匯入。

## 中繼資料CSV檔案格式 {#metadata-file-format}

### 第一列

* CSV檔案的第一列定義中繼資料結構。
* 第一欄預設為 `assetPath`，其中包含資產的絕對JCR路徑。

* 第一列中的後續欄指向資產的其他中繼資料屬性。
   * 例如： `dc:title, dc:description, jcr:title`

* 單值屬性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定屬性類型，其預設值為字串。
   * 例如：`dc:title {{String}}`

* 屬性名稱區分大小寫
   * 正確： `dc:title {{String}}`
   * 錯誤： `Dc:Title {{String}}`

* 屬性類型不區分大小寫
* 全部有效 [JCR屬性類型](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 支援

* 多值屬性格式 —  `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一欄保留資產的絕對JCR路徑。 例如：/content/dam/asset1.jpg
* 資產的中繼資料屬性在CSV檔案中可能缺少值。 不會更新該特定資產的遺失中繼資料屬性。
