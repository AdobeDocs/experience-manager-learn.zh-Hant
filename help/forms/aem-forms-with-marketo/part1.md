---
title: AEM Forms與Marketo（第1部分）
description: 使用AEM Forms表單資料模型整合AEM Forms與Marketo的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# AEM Forms與Marketo

Marketo是Adobe的一部分，提供行銷自動化軟體，著重於帳戶型行銷，包括電子郵件、行動裝置、社交、數位廣告、網頁管理和分析。

我們現在可以使用AEM Forms的表單資料模型，將AEM Form與Marketo緊密整合。

[深入了解表單資料模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo會公開REST API，允許遠端執行系統的許多功能。 從建立程式到大量銷售機會匯入，有許多選項可允許對Marketo例項進行微調控制。 使用表單資料模型，將AEM Forms與Marketo整合相當簡單。

本教學課程將逐步引導您完成使用表單資料模型整合AEM Forms與Marketo的相關步驟。 完成本教學課程後，您會擁有OSGi套件組合，針對Marketo進行自訂驗證。 您也將使用提供的Swagger檔案來設定資料來源。

若要開始，強烈建議您熟悉「先決條件」區段中列出的下列主題。

## 必備條件

1. [已安裝AEM Forms Add的AEM伺服器套件](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本機AEM開發環境
1. 熟悉表單資料模型
1. Swagger檔案的基本知識
1. 建立適用性Forms

**用戶端密碼ID和用戶端密碼金鑰**

Marketo與AEM Forms整合的第一步，是取得使用API進行REST呼叫所需的API憑證。 您需要下列項目

1. client_id
1. client_secret
1. identity_endpoint
1. 驗證URL

[請參閱Marketo官方檔案，取得上述屬性。](https://developers.marketo.com/rest-api/) 或者，您也可以聯絡Marketo執行個體的管理員。

**開始之前**

[下載並解壓縮與本文相關的資產。](assets/aemformsandmarketo.zip) zip檔案包含下列項目：

1. BlankTemplatePackage.zip — 此為最適化表單範本。 使用套件管理器匯入此項目。
1. marketo.json — 此為用來設定資料來源的Swagger檔案。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — 此為執行自訂驗證的套件組合。 如果您無法完成教學課程，或您的套件無法如預期般運作，歡迎使用此功能。

## 後續步驟

[建立自訂驗證](./part2.md)
