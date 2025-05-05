---
title: 整合AEM Forms和Marketo
description: 瞭解如何使用AEM Forms表單資料模型整合AEM Forms和Marketo。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# 整合AEM Forms和Marketo


Marketo是Adobe的一部分，提供行銷自動化軟體，其著重於以帳戶為基礎的行銷，包括電子郵件、行動裝置、社交、數位廣告、網頁管理和分析。

現在，我們可以運用AEM Forms的表單資料模型，將AEM Form與Marketo緊密整合。

[進一步瞭解表單資料模型](https://helpx.adobe.com/tw/experience-manager/6-5/forms/using/data-integration.html)

Marketo公開REST API，允許從遠端執行系統的許多功能。 從建立程式到大量潛在客戶匯入，有許多選項可讓您對Marketo執行個體進行微調控制。 使用表單資料模型時，可輕鬆將AEM Forms與Marketo整合。

>[!NOTE]
>
>本教學課程專為AEM Forms 6.5量身打造。如果您想要整合AEM Forms as a Cloud Service與Adobe Marketo Engage，請參閱該整合的[專屬檔案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/forms/integrate/services/integrate-adaptive-form-with-market-engage/integrate-form-to-marketo-engage)。

本教學課程將逐步引導您完成使用表單資料模型將AEM Forms與Marketo整合的步驟。 完成本教學課程後，您將會擁有OSGi套件組合，可針對Marketo執行自訂驗證。 您也將已使用提供的swagger檔案設定資料來源。

若要開始使用，強烈建議您熟悉先決條件區段中所列的下列主題。

## 必備條件

1. [已安裝AEM附加套件的AEM Forms伺服器](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本機AEM開發環境
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

* [下載並解壓縮與本教學課程相關的資產](assets/marketo-integration-assets.zip)

zip檔案包含以下內容：

1. BlankTemplatePackage.zip — 這是最適化表單範本。 使用封裝管理員匯入此專案。
1. marketo.json — 這是用來設定資料來源的Swagger檔案。
1. 請務必變更marketo.json中的主機屬性，以指向您的marketo例項

## 後續步驟

[建立資料Source](./part2.md)
