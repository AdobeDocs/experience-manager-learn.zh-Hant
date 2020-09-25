---
title: 建立資料庫表
description: 建立表單資料模型要使用的資料庫
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# 建立資料庫表

表單資料模型可以以RDBMS、RESTfull、SOAP或OData來源為基礎。 本課程的重點是使用由RDBMS資料源支援的表單資料模型預先歸檔自適應表單。 本教學課程使用MYSQL資料庫。 我們建立了下列兩個表格來示範使用案例

* **newhire表** -此表儲存了新的資訊

   ![newhire](assets/newhire-table.png)


* **受益人** 表——這家商店的受益人

   ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL工 [作台將sql檔案導入](assets/db-schema.sql) ，以便建立到具有某些示例資料的表。