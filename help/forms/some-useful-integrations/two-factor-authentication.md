---
title: SMS雙因素驗證
description: 新增額外的安全層級，可在使用者想要執行特定活動時協助確認使用者的身份
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# 使用行動電話號碼驗證使用者

SMS雙因素驗證（雙因素驗證）是一種安全性驗證程式，透過使用者登入網站、軟體或應用程式來觸發。 在登入程式中，使用者會自動傳送SMS至包含唯一數字代碼的手機號碼。

有許多組織提供此服務，只要他們妥善記錄REST API，您就可以使用AEM Forms的資料整合功能，輕鬆整合AEM Forms。 出於本教學課程的目的，我已使用[Nexmo](https://developer.nexmo.com/verify/overview)示範SMS 2FA使用案例。

依下列步驟，使用Nexmo Verify服務透過AEM Forms實作SMS 2FA。

## 建立開發人員帳戶

使用[Nexmo](https://dashboard.nexmo.com/sign-in)建立開發人員帳戶。 記下API金鑰和API秘密金鑰。 呼叫Nexmo服務的REST API時需要這些金鑰。

## 建立Swagger/OpenAPI檔案

OpenAPI Specification （前身為Swagger Specification）是REST API的API說明格式。 OpenAPI檔案可讓您說明整個API，包括：

* 每個端點的可用端點(/users)和操作(GET /users、POST /users)
* 作業引數每個作業的輸入和輸出
驗證方法
* 聯絡資訊、授權、使用條款及其他資訊。
* API規格可以用YAML或JSON撰寫。 該格式簡單易學，且可供人類和機器讀取。

若要建立您的第一個swagger/OpenAPI檔案，請依照[OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)操作

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用[swagger編輯器](https://editor.swagger.io/)建立您的swagger檔案，以說明傳送及驗證使用簡訊傳送之OTP代碼的作業。 swagger檔案可以建立JSON或YAML格式。 可以從[這裡](assets/two-factore-authentication-swagger.zip)下載已完成的swagger檔案

## 建立資料Source

若要將AEM/AEM Forms與協力廠商應用程式整合，我們必須在雲端服務設定中[建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=zh-Hant)。

## 建立表單資料模型

AEM Forms資料整合提供直覺式使用者介面，可建立和使用[表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=zh-Hant)。 表單資料模型仰賴資料來源交換資料。
完成的表單資料模型可從這裡[&#128279;](assets/sms-2fa-fdm.zip)下載

![fdm](assets/2FA-fdm.PNG)

## 建立最適化表單

將表單資料模型的POST呼叫與您的自適應表單整合，以驗證使用者在表單中輸入的行動電話號碼。 您可以自由建立自己的最適化表單，並根據需求使用表單資料模型的POST引動來傳送和驗證OTP代碼。

如果您想要搭配API金鑰使用範例資產，請遵循下列步驟：

* [下載表單資料模型](assets/sms-2fa-fdm.zip)並使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入至AEM
* 可從這裡[&#128279;](assets/sms-2fa-verification-af.zip)下載最適化表單範例。 此範例表單使用本文中提供的表單資料模型的服務引動。
* 從[Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)將表單匯入AEM
* 在編輯模式中開啟表單。 開啟下列欄位的規則編輯器

![簡訊傳送](assets/check-sms.PNG)

* 編輯與欄位關聯的規則。 提供適當的API金鑰
* 儲存表單
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled)並測試功能
