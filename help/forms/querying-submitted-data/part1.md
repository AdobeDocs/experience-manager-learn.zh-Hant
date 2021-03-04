---
title: AEM Forms與JSON結構描述與資料[第1部分]
seo-title: AEM Forms與JSON結構描述與資料[Part1]
description: 多部分教學課程，可引導您逐步瞭解使用JSON結構描述建立最適化表單及查詢提交資料的相關步驟。
seo-description: 多部分教學課程，可引導您逐步瞭解使用JSON結構描述建立最適化表單及查詢提交資料的相關步驟。
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 1%

---


# 根據JSON結構描述建立最適化表單


AEM Forms6.3版推出以JSON結構描述為基礎的最適化Forms。 有關使用JSON結構描述建立最適化Forms的詳細資訊，請參閱本[文章](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)。

根據JSON結構描述建立最適化表單後，下一步是將提交的資料儲存在資料庫中。 為此，我們將使用各種資料庫廠商引進的新JSON資料類型。 為了本文的目的，我們將使用MySql 8資料庫來儲存提交的資料。

本文使用MySql 8資料庫。 MySQL引入了名為[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)的新資料類型。 這可讓儲存和查詢JSON物件更輕鬆。 我們將將提交的資料儲存在我們資料庫的JSON類型欄中。

下列螢幕擷取畫面顯示儲存在JSON資料類型中的已提交表單資料。 欄&quot;formdata&quot;是JSON類型。 我們還將與資料關聯的表單的名稱儲存在清單單名稱中

>[!NOTE]
>
>請確定您的json結構描述檔已正確命名。 例如，它需要以下列格式命名：&lt;name>schema.json。 因此，您的結構描述檔可以是mortgage.schema.json或credit.schema.json。


![資料儲存](assets/datastored.gif)


[可用來建立最適化Forms的範例JSON結構描述。](assets/samplejsonschemas.zip). 下載並解壓縮zip檔案，以取得JSON結構描述

