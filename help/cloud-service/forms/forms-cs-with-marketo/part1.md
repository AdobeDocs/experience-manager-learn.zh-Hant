---
title: 整合AEM Formsas a Cloud Service和Marketo
description: 瞭解如何使用AEM Forms表單資料模型整合AEM Forms和Marketo。
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 1%

---

# 整合AEM Forms和Marketo

Marketo是Adobe的一部分，提供行銷自動化軟體，其著重於以帳戶為基礎的行銷，包括電子郵件、行動裝置、社交、數位廣告、網頁管理和分析。

現在，我們可以運用AEM Forms的表單資料模型，將AEM表單與Marketo緊密整合。

[進一步瞭解表單資料模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公開REST API，允許從遠端執行系統的許多功能。 從建立程式到大量潛在客戶匯入，有許多選項可讓您對Marketo執行個體進行微調控制。 使用表單資料模型時，可輕鬆將AEM Forms與Marketo整合。

本教學課程將逐步引導您完成使用表單資料模型將AEM Forms與Marketo整合的步驟。 完成本教學課程後，您將會擁有OSGi套件組合，可針對Marketo執行自訂驗證。 您也將已使用提供的swagger檔案設定資料來源。

若要開始使用，強烈建議您熟悉先決條件區段中所列的下列主題。

## 必備條件

1. 存取AEM Formsas a Cloud Service執行個體
1. 熟悉表單資料模型
1. Swagger檔案的基本知識
1. 建立最適化Forms

**使用者端密碼識別碼與使用者端密碼金鑰**

Marketo與AEM Forms整合的第一步是取得使用API執行REST呼叫所需的API認證。 您將需要下列專案

1. client_id
1. client_secret
1. identity_endpoint

[請依照官方Marketo檔案操作，以取得上述屬性。](https://developers.marketo.com/rest-api/)或者，您也可以連絡Marketo執行個體的管理員。

**在您開始之前**

* [下載並解壓縮與本教學課程相關的資產](assets/marketo.zip)

zip檔案包含以下內容：

1. marketo.json — 這是用來設定資料來源的Swagger檔案。
1. 請務必變更marketo.json中的主機屬性，以指向您的marketo例項

## 後續步驟

[建立資料Source](./part2.md)
