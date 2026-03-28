---
title: 建立IC檔案的表單資料模型
description: 瞭解如何在AEM Forms中建立表單資料模型，以動態擷取互動式通訊檔案的資料。
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# 建立IC檔案的表單資料模型

建立Forms資料模型，將外部資料來源與Adobe AEM中的互動式通訊整合。 此程式包含設定RESTful服務、上傳Swagger檔案，以及設定服務端點以動態擷取及繫結資料。 瞭解如何安全地連線到外部服務並測試模型以確保成功的資料擷取。

已實作模擬API伺服器，可模擬Orders服務以進行開發和測試。 它會公開端點，以擷取特定使用者的訂單（例如，依使用者ID），傳回與生產API相同結構描述中預先定義或動態產生的訂單資料。

建立表單資料模型時使用的Swagger檔案可從這裡[下載](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480028/?captions=chi_hant&learn=on&enablevpops)

## 後續步驟

[建立範本](./create-template.md)
