---
title: 在AEM Assets使用元資料導入和導出
description: 瞭解如何使用Adobe Experience Manager資產的導入和導出元資料功能。 導入和導出功能允許內容作者批量更新現有資產的元資料。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

---

# 在AEM Assets使用元資料導入和導出 {#metadata-import-and-export}

瞭解如何使用Adobe Experience Manager資產的導入和導出元資料功能。 導入和導出功能允許內容作者批量更新現有資產的元資料。

## 中繼資料匯出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## 中繼資料匯入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 在準備要導入的CSV檔案時，使用元資料導出功能更容易生成包含資產清單的CSV。 然後，可以修改生成的CSV檔案，並使用「導入」功能導入它。

## 元資料CSV檔案格式 {#metadata-file-format}

### 第一行

* CSV檔案的第一行定義元資料架構。
* 第一列預設為 `assetPath`，它包含資產的絕對JCR路徑。

* 第一行中的後續列指向資產的其他元資料屬性。
   * 例如：`dc:title, dc:description, jcr:title`

* 單值屬性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定屬性類型，則預設為String。
   * 例如：`dc:title {{String}}`

* 屬性名稱區分大小寫
   * 正確： `dc:title {{String}}`
   * 錯誤： `Dc:Title {{String}}`

* 屬性類型不區分大小寫
* 全部有效 [JCR屬性類型](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 支援

* 多值屬性格式 —  `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列保存資產的絕對JCR路徑。 例如：/content/dam/asset1.jpg
* 資產的元資料屬性在CSV檔案中可能缺少值。 不更新該特定資產缺少的元資料屬性。
