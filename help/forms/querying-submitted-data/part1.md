---
title: AEM Forms搭配JSON結構描述和資料[第1部分]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: 多部分教學課程，逐步引導您完成使用JSON結構描述建立適用性表單和查詢提交資料的相關步驟。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 根據JSON結構建立最適化表單


AEM Forms 6.3版推出以JSON結構描述為基礎建立適用性Forms的功能。 有關使用JSON結構描述建立適用性Forms的詳細資訊，請參閱本節 [文章](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

根據JSON結構建立適用性表單後，下一步就是將提交的資料儲存在資料庫中。 為此，我們將使用各種資料庫廠商推出的新JSON資料類型。 為了本文的目的，我們將使用MySql 8資料庫來儲存提交的資料。

MySql 8資料庫用於本文。 MySQL引入了一種新的資料類型，稱為 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). 這可讓您更輕鬆儲存和查詢JSON物件。 我們會將提交的資料儲存在資料庫的JSON類型欄中。

下列螢幕擷取畫面顯示已提交的表單資料，這些資料儲存在JSON資料類型中。 「formdata」欄的類型為JSON。 我們也會將與資料相關聯的表單名稱儲存在欄表單名稱中

>[!NOTE]
>
>請確定您的json結構描述檔案已正確命名。 例如，需以下列格式命名 &lt;name>schema.json。 因此，您的結構檔案可以是mortgage.schema.json或credit.schema.json。


![資料儲存](assets/datastored.gif)


[可用來建立最適化Forms的JSON結構範例。](assets/samplejsonschemas.zip). 下載並解壓縮zip檔案以取得JSON結構描述
