---
title: AEM FormsJSON架構和資料[第1部分]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: 多部分教程，引導您完成建立帶JSON架構的自適應表單和查詢提交資料所涉及的步驟。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# 基於JSON架構建立自適應表單


在AEM Forms6.3版中引入了基於JSON架構建立自適應Forms的功能。 有關使用JSON架構建立自適應Forms的詳細資訊將在本章中詳細說明 [文章](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html)。

在基於JSON架構建立自適應表單後，下一步是將提交的資料儲存在資料庫中。 為此，我們將使用各種資料庫供應商引入的新JSON資料類型。 為了本文的目的，我們將使用MySql 8資料庫來儲存提交的資料。

MySql 8資料庫用於本文。 MySQL引入了一種名為 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)。 這使儲存和查詢JSON對象變得更容易。 我們將將提交的資料儲存在資料庫中JSON類型的列中。

以下螢幕抓圖顯示以JSON資料類型儲存的已提交表單資料。 列&quot;formdata&quot;的類型為JSON。 我們還將與資料關聯的表單的名稱儲存在清單單名稱中

>[!NOTE]
>
>請確保您的json架構檔案已正確命名。 例如，需要以下格式命名 &lt;name>schema.json。 因此，您的架構檔案可以是mortgage.schema.json或credit.schema.json。


![資料儲存](assets/datastored.gif)


[可用於建立自適應Forms的示例JSON架構。](assets/samplejsonschemas.zip). 下載並解壓縮zip檔案以獲取JSON架構
