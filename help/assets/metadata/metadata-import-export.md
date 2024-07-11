---
title: 在AEM Assets中使用中繼資料匯入和匯出
description: 瞭解如何使用Adobe Experience Manager Assets的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: 726715890d997ba3bb85f4833e220ac2222b3a42
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# 在AEM Assets中使用中繼資料匯入和匯出 {#metadata-import-and-export}

瞭解如何使用Adobe Experience Manager Assets的匯入和匯出中繼資料功能。 匯入和匯出功能可讓內容作者大量更新現有資產的中繼資料。

## 中繼資料匯出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> 在Excel中開啟中繼資料匯出CSV檔案時，請使用 [Excel匯入工具](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) 而不要連按兩下檔案來避免UTF-8編碼的CSV檔案發生問題。
>
> 若要在Excel中開啟中繼資料匯出CSV檔案，請執行下列步驟：
> 
> 1. 開啟Microsoft Excel
> 1. 選取 __檔案>新增__ 建立空白試算表
> 1. 在空白試算表開啟的狀態下，選取 __檔案>匯入__
> 1. 選取 __文字__ 檔案並按一下 __匯入__
> 1. 從檔案系統選取匯出的CSV檔案，然後按一下 __取得資料__
> 1. 在匯入精靈的步驟1中，選取 __已分隔__ 並設定 __檔案來源__ 至 __Unicode (UTF-8)__，然後按一下 __下一個__
> 1. 在步驟2中，設定 __分隔字元__ 至 __逗號__，然後按一下 __下一個__
> 1. 在步驟3中，將 __欄資料格式__ 原樣，然後按一下 __完成__
> 1. 選取 __匯入__ 將資料新增至試算表

## 中繼資料匯入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 在準備要匯入的CSV檔案時，使用「中繼資料匯出」功能可更輕鬆地產生包含資產清單的CSV。 然後，您可以修改產生的CSV檔案，並使用「匯入」功能將其匯入。

## 中繼資料CSV檔案格式 {#metadata-file-format}

### 第一列

* CSV檔案的第一列定義中繼資料結構。
* 第一欄預設為 `assetPath`，儲存資產的絕對JCR路徑。

* 第一列中的後續欄會指向資產的其他中繼資料屬性。
   * 例如： `dc:title, dc:description, jcr:title`

* 單值屬性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定屬性型別，其預設值為String。
   * 例如：`dc:title {{String}}`

* 屬性名稱區分大小寫
   * 正確： `dc:title {{String}}`
   * 不正確： `Dc:Title {{String}}`

* 屬性型別區分大小寫
* 全部有效 [JCR屬性型別](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 受支援

* 多值屬性格式 —  `<metadata property name> {{<property type : MULTI }}`

### 第二列為N列

* 第一欄包含資產的絕對JCR路徑。 例如： /content/dam/asset1.jpg
* 資產的中繼資料屬性在CSV檔案中可能遺漏值。 遺失該特定資產的中繼資料屬性不會更新。
