---
title: 建立結構描述
description: 根據需要匯入最適化表單的資料建立結構描述
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# 簡介

第一步是根據將用來填入調適型表單的資料建立結構描述。

## XFA是以結構描述為基礎

使用結構描述建立最適化表單

## XFA並非以結構描述為基礎

* 在AEM Forms設計工具中開啟XDP。
* 按一下檔案 | 表單屬性 | 預覽。
* 按一下「產生預覽資料」。
* 按一下「產生」。
* 提供有意義的檔案名稱，例如`form-data.xml`

您可以使用任何免費的線上工具，從上個步驟產生的xml資料中[產生XSD](https://www.freeformatter.com/xsd-generator.html)。

根據上一步的結構描述建立最適化表單。

>[!NOTE]
>建議您一律檢查在提交最適化表單時產生的資料。 這可讓您非常瞭解需要與最適化表單合併之資料的XML格式。

從最適化表單提交的資料
![提交的資料](./assets/af-submitted-data.png)

從PDF匯出的資料
![匯出的資料](./assets/exported-data.png)

從匯出的資料中，您必須擷取&#x200B;**_topmostSubform_**&#x200B;節點，並保留適當的名稱空間，才能順利將資料與最適化表單合併。

## 後續步驟

[建立OSGi服務](./create-osgi-service.md)
