---
title: 在AEM資產中使用中繼資料匯入和匯出
seo-title: 在AEM資產中使用中繼資料匯入和匯出
description: AEM Assets中繼資料匯入和匯出功能可讓內容作者輕鬆將資產中繼資料移入和移出AEM，並運用Microsoft Excel的強大功能來大規模控制中繼資料，以利AEM中現有資產的大量更新中繼資料。
seo-description: AEM Assets中繼資料匯入和匯出功能可讓內容作者輕鬆將資產中繼資料移入和移出AEM，並運用Microsoft Excel的強大功能來大規模控制中繼資料，以利AEM中現有資產的大量更新中繼資料。
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# 在AEM Assets中使用中繼資料匯入和匯出{#using-metadata-import-and-export-in-aem-assets}

AEM Assets中繼資料匯入和匯出功能可讓內容作者輕鬆將資產中繼資料移入和移出AEM，並運用Microsoft Excel的強大功能來大規模控制中繼資料，以利AEM中現有資產的大量更新中繼資料。

## 中繼資料匯出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## 中繼資料匯入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

下載[WeRetail sports資料夾](assets/we-retail-sports.zip)

下載[資產中繼資料套件](assets/we-retail-sports-asset-metadata.zip)

## 中繼資料檔案格式{#metadata-file-format}

### CSV檔案格式

#### 第一列

* CSV檔案的第一列定義中繼資料結構。
* 「第一列」預設為`assetPath`，其中包含資產的絕對JCR路徑。

* 第一行中的後續欄會指向資產的其他中繼資料屬性。

   * 例如：`dc:title, dc:description, jcr:title`

* 單值屬性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定屬性類型，則預設為「字串」。
   * 例如：`dc:title {{String}}`

* 屬性名稱區分大小寫
   * 正確：`dc:title {{String}}`
   * 錯誤：`Dc:Ttle {{String}}`

* 屬性類型不區分大小寫
* 所有有效的[JCR屬性類型](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)都受支援

* 多值屬性格式- `<metadata property name> {{<property type : MULTI }}`

#### 第二行到N行

* 第一列保存資產的絕對JCR路徑。 例如：/content/dam/asset1.jpg
* 資產的中繼資料屬性在CSV檔案中可能遺失值。 不會更新該特定資產遺失的中繼資料屬性。