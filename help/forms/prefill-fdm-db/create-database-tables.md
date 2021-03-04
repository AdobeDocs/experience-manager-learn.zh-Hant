---
title: 建立資料庫表
description: 建立表單資料模型要使用的資料庫
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 3%

---


# 建立資料庫表

表單資料模型可以以RDBMS、RESTfull、SOAP或OData來源為基礎。 本課程的重點是使用由RDBMS資料源支援的表單資料模型預先歸檔自適應表單。 本教學課程使用MYSQL資料庫。 我們建立了下列兩個表格來示範使用案例

* **** newhiretable —— 此表儲存新資訊

   ![newhire](assets/newhire-table.png)


* **受益** 者——這家商店的受益者

   ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL工作台將[sql檔案](assets/db-schema.sql)導入到具有某些示例資料的表。