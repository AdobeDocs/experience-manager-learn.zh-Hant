---
title: 建立資料庫表
description: 建立要由表單資料模型使用的資料庫
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 建立資料庫表

表單資料模型可以基於RDBMS、RESTfull、SOAP或OData源。 本課程的重點是使用RDBMS資料源支援的表單資料模型預歸檔自適應表單。 本教程使用了MYSQL資料庫。 我們建立了以下兩個表來演示使用案例

* **新白** 表 — 此表儲存新資訊

   ![新白](assets/newhire-table.png)


* **受益人** 表 — 此商店的受益人

   ![受益人](assets/beneficiaries-table.png)

可以導入 [sql檔案](assets/db-schema.sql) 使用MySQL Workbench建立包含一些示例資料的表。

## 後續步驟

[配置表單資料模型](./configuring-form-data-model.md)
