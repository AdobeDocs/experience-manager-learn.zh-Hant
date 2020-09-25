---
title: AEM Forms with Marketo(Part 1)
seo-title: AEM Forms with Marketo(Part 1)
description: 使用AEM Forms Data Model將AEM Forms與Market整合的教學課程。
seo-description: 使用AEM Forms Data Model將AEM Forms與Market整合的教學課程。
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# AEM Forms With Marketo

Marketo是Adobe的一部分，提供行銷自動化軟體，主要針對以帳戶為基礎的行銷，包括電子郵件、行動裝置、社交、數位廣告、網頁管理和分析。

使用AEM Forms的「表單資料模型」，我們現在可以將AEM Forms與Market完美整合。

[進一步瞭解表單資料模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公開REST API，可讓遠端執行系統的許多功能。 從建立程式到大量匯入銷售機會，有許多選項可讓您精細控制Marketo例項。 使用表單資料模型，將AEM Forms與Marketo整合相當簡單。

本教學課程將引導您逐步瞭解使用表單資料模型將AEM Forms與Marketo整合的相關步驟。 完成教學課程後，您將會擁有OSGi套件，可針對Marketo進行自訂驗證。 您也可以使用提供的Swagger檔案來設定資料來源。

若要開始，強烈建議您熟悉「先決條件」區段中所列的下列主題。

## 先決條件

1. [已安裝AEM Forms Add on套件的AEM伺服器](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本機AEM開發環境
1. 熟悉表單資料模型
1. Swagger檔案的基本知識
1. 建立最適化表單

**用戶端密碼ID和用戶端密碼金鑰**

將Marketo與AEM Forms整合的第一步，就是取得使用API進行REST呼叫所需的API認證。 您將需要下列

1. client_id
1. client_secret
1. identity_endpoint
1. 驗證URL。

[請依照官方行銷檔案取得上述物業。](https://developers.marketo.com/rest-api/) 或者，您也可以連絡您Marketo例項的管理員。

**開始之前**

[下載並解壓縮與本文相關的資產。](assets/aemformsandmarketo.zip) zip檔案包含下列項目：

1. BlankTemplatePackage.zip —— 這是Adaptive Form模板。 使用套件管理器匯入此項。
1. marketo.json —— 此為將用來設定資料來源的Swagger檔案。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar —— 這是執行自訂驗證的套件。 如果您無法完成教學課程，或您的套件無法如預期般運作，請自由使用此功能。
