---
title: 建立資料庫表格
description: 建立表單資料模型要使用的資料庫
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 建立資料庫表格

表單資料模型可以以RDBMS、RESTfull、SOAP或OData來源為基礎。 本課程的重點是使用RDBMS資料來源支援的表單資料模型預先填寫最適化表單。 在本教學課程中，使用了MYSQL資料庫。 我們建立了以下兩個表格來示範使用案例

* **newhire** 表格 — 此表格儲存新的資訊

  ![newhire](assets/newhire-table.png)


* **受益人** 表格 — 此表格儲存新的受益人

  ![受益人](assets/beneficiaries-table.png)

您可以匯入 [sql檔案](assets/db-schema.sql) 使用MySQL Workbench建立包含一些範例資料的表格。

## 後續步驟

[設定表單資料模型](./configuring-form-data-model.md)
