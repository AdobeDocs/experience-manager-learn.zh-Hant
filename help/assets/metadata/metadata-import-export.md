---
title: 在AEM Assets使用元資料導入和導出
description: 瞭解如何使用Adobe Experience Manager資產的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。
version: 6.3, 6.4, 6.5, cloud-service
topic: 內容管理
feature: 中繼資料
role: 管理員
level: 中級
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 3%

---


# 在AEM Assets使用元資料導入和導出{#metadata-import-and-export}

瞭解如何使用Adobe Experience Manager資產的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。

## 中繼資料匯出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## 中繼資料匯入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 在準備要匯入的CSV檔案時，使用「中繼資料匯出」功能更容易產生包含資產清單的CSV。 然後，您可以修改產生的CSV檔案，並使用「匯入」功能匯入它。

## 中繼資料CSV檔案格式{#metadata-file-format}

### 第一列

* CSV檔案的第一列定義中繼資料結構。
* 「第一列」預設為`assetPath`，其中包含資產的絕對JCR路徑。

* 第一行中的後續欄會指向資產的其他中繼資料屬性。
   * 例如：`dc:title, dc:description, jcr:title`

* 單值屬性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定屬性類型，則預設為字串。
   * 例如：`dc:title {{String}}`

* 屬性名稱區分大小寫
   * 正確：`dc:title {{String}}`
   * 錯誤：`Dc:Title {{String}}`

* 屬性類型不區分大小寫
* 所有有效的[JCR屬性類型](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)都受支援

* 多值屬性格式- `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列保存資產的絕對JCR路徑。 例如：/content/dam/asset1.jpg
* 資產的中繼資料屬性在CSV檔案中可能遺失值。 不會更新該特定資產遺失的中繼資料屬性。
