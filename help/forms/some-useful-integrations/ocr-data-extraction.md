---
title: OCR資料擷取
description: 從政府核發的檔案擷取資料以填入表單。
feature: 巴科德Forms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---



# OCR資料擷取

自動從各種政府發佈的檔案擷取資料，以填入您的調適性表單。

有許多組織提供此項服務，只要他們有詳細記載的REST API，您就可使用資料整合功能輕鬆與AEM Forms整合。 在本教學課程中，我使用[ID Analyzer](https://www.idanalyzer.com/)來示範已上傳檔案的OCR資料擷取。

依照下列步驟，使用ID Analyzer服務使用AEM Forms實作OCR資料擷取。

## 建立開發人員帳戶

使用[ID Analyzer](https://portal.idanalyzer.com/signin.html)建立開發人員帳戶。 記下API金鑰。 需要此密鑰才能調用ID Analyzer服務的REST API。

## 建立Swagger/OpenAPI檔案

OpenAPI規格（先前稱為Swagger規格）是REST API的API說明格式。 OpenAPI檔案可讓您描述整個API，包括：

* 每個端點上的可用端點（/用戶）和操作(GET/用戶、POST/用戶)
* 工序參數每個工序的輸入和輸出
驗證方法
* 聯絡資訊、授權、使用條款及其他資訊。
* API規格可以使用YAML或JSON編寫。 該格式易於學習，對人和機器都可讀。

若要建立您的第一個Swagger/OpenAPI檔案，請依照[OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用[swagger編輯器](https://editor.swagger.io/)建立Swagger檔案，以說明使用SMS發送並驗證OTP代碼的操作。 Swagger檔案可建立為JSON或YAML格式。 您可從[這裡](assets/drivers-license-swagger.zip)下載完成的Swagger檔案

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要在雲端服務組態中建立資料來源](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)。 [請使用[swagger檔案](assets/drivers-license-swagger.zip)來建立您的資料來源。

## 建立表單資料模型

AEM Forms資料整合提供直觀的用戶介面，用於建立和使用[表單資料模型](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 以先前步驟中建立的資料來源為基礎的表單資料模型。

![fdm](assets/test-dl-fdm.PNG)

## 建立客戶端庫

我們需要取得已上傳檔案的base64編碼字串。 然後，此base64編碼字串會作為REST呼叫的參數之一傳遞。
可從此處下載客戶端庫[。](assets/drivers-license-client-lib.zip)

## 建立最適化表單

將表單資料模型的POST調用與最適化表單整合，以從表單中由使用者上傳的檔案擷取資料。 您可以自由建立自己的最適化表單，並使用表單資料模型的POST呼叫來傳送已上載檔案的base64編碼字串。

## 部署在您的伺服器上

如果您想要搭配API金鑰使用範例資產，請遵循下列步驟：

* [下載資料來](assets/drivers-license-source.zip) 源，並使用套件管AEM理器匯 [入至](http://localhost:4502/crx/packmgr/index.jsp)
* [使用套件管理器](assets/drivers-license-fdm.zip) 將表單資料模AEM型匯入 [下載](http://localhost:4502/crx/packmgr/index.jsp)
* [下載用戶端程式庫](assets/drivers-license-client-lib.zip)
* 從此處下載樣例最適化表單[。 ](assets/adaptive-form-dl.zip)此示例表單使用作為本文一部分提供的表單資料模型的服務調用。
* 將表單從AEM[Forms和文檔UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)導入
* 在[編輯模式下開啟表單。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* 在apikey欄位中指定您的API金鑰作為預設值，並儲存您所做的變更
* 開啟「64字串基本」欄位的規則編輯器。 當此欄位的值更改時，請注意服務調用。
* 儲存表格
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，上傳駕照的正面圖片


