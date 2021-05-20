---
title: AEM Forms搭配JSON結構描述和資料[第1部分]
seo-title: AEM Forms搭配JSON結構描述和資料[Part1]
description: 多部分教學課程，逐步引導您完成使用JSON結構描述建立適用性表單和查詢提交資料的相關步驟。
seo-description: 多部分教學課程，逐步引導您完成使用JSON結構描述建立適用性表單和查詢提交資料的相關步驟。
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 1%

---


# 根據JSON結構建立最適化表單


AEM Forms 6.3版推出以JSON結構描述為基礎建立適用性Forms的功能。 有關使用JSON結構描述建立適用性Forms的詳細資訊，請參閱本[文章](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)。

根據JSON結構建立適用性表單後，下一步就是將提交的資料儲存在資料庫中。 為此，我們將使用各種資料庫廠商推出的新JSON資料類型。 為了本文的目的，我們將使用MySql 8資料庫來儲存提交的資料。

MySql 8資料庫用於本文。 MySQL引入了名為[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)的新資料類型。 這可讓您更輕鬆儲存和查詢JSON物件。 我們會將提交的資料儲存在資料庫中JSON類型的欄中。

下列螢幕擷取畫面顯示已提交的表單資料，這些資料儲存在JSON資料類型中。 「formdata」欄的類型為JSON。 我們也會將與資料相關聯的表單名稱儲存在欄表單名稱中

>[!NOTE]
>
>請確定您的json結構描述檔案已正確命名。 例如，需要以下列格式命名。 因此，您的結構檔案可以是mortgage.schema.json或credit.schema.json。


![資料儲存](assets/datastored.gif)


[可用來建立最適化Forms的JSON結構範例。](assets/samplejsonschemas.zip). 下載並解壓縮zip檔案以取得JSON結構描述

