---
title: 建立資料庫表
description: 建立要由表單資料模型使用的資料庫
feature: 適用性表單
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# 建立資料庫表

表單資料模型可以以RDBMS、RESTfull、SOAP或OData來源為基礎。 本課程的重點在於使用RDBMS資料來源支援的表單資料模型預先填入最適化表單。 本教程使用了MYSQL資料庫。 我們建立了以下兩個表格來演示使用案例

* **** newhiretable — 此表儲存了newhire資訊

   ![內惠爾](assets/newhire-table.png)


* **** 受益者 — 此商店接近受益者

   ![受益人](assets/beneficiaries-table.png)

您可以使用MySQL Workbench匯入[sql檔案](assets/db-schema.sql)，以建立包含某些範例資料的表格。