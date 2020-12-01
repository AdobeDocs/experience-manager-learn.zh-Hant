---
title: SMS雙因素驗證
description: 新增額外的安全層，協助在使用者想要執行特定活動時確認其身分
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 4c08b09f59be0eb6644aaec729807b92bc339e82
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---



# 驗證使用者是否使用其行動電話號碼

SMS雙因素驗證（雙因素驗證）是一種安全驗證程式，由用戶登錄到網站、軟體或應用程式時觸發。 在登入程式中，使用者會自動傳送SMS至包含唯一數值代碼的行動電話號碼。

有許多組織提供此服務，只要他們有詳細記錄的REST API，您就可以使用AEM Forms的資料整合功能，輕鬆整合AEM Forms。 在本教學課程中，我使用[Nexmo](https://developer.nexmo.com/verify/overview)來示範SMS 2FA使用案例。

依照下列步驟，使用Nexmo Verify服務，將SMS 2FA與AEM Forms實作。

## 建立開發人員帳戶

使用[Nexmo](https://dashboard.nexmo.com/sign-in)建立開發人員帳戶。 記下API金鑰和API機密金鑰。 需要這些密鑰才能調用Nexmo服務的REST API。

## 建立Swagger/OpenAPI檔案

OpenAPI規格（先前稱為Swagger規格）是REST API的API說明格式。 OpenAPI檔案可讓您描述整個API，包括：

* 每個端點上的可用端點（/用戶）和操作(GET /users、POST /users)
* 工序參數每個工序的輸入和輸出
驗證方法
* 聯絡資訊、授權、使用條款及其他資訊。
* API規格可以使用YAML或JSON編寫。 該格式易於學習，對人和機器都可讀。

若要建立您的第一個Swagger/OpenAPI檔案，請依照[OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用[swagger編輯器](https://editor.swagger.io/)建立Swagger檔案，以說明使用SMS發送並驗證OTP代碼的操作。 Swagger檔案可建立為JSON或YAML格式。 您可從[這裡](assets/two-factore-authentication-swagger.zip)下載完成的Swagger檔案

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要在雲端服務設定中[建立資料來源](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)。

## 建立表單資料模型

AEM Forms資料整合提供直覺式使用者介面，以建立及使用[表單資料模型](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表單資料模型依賴資料來源來交換資料。
完成的表單資料模型可從此處[下載](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 建立最適化表單

將表單資料模型的POST呼叫與您的最適化表單整合，以驗證使用者在表單中輸入的行動電話號碼。 您可以自由建立自己的自適應表單，並根據您的要求使用表單資料模型的POST調用來發送和驗證OTP代碼。

如果您想要將範例資產與您的API金鑰搭配使用，請遵循下列步驟：

* [使用套件管理器將表](assets/sms-2fa-fdm.zip) 單資料模型匯入AEM [中](http://localhost:4502/crx/packmgr/index.jsp)
* 從此處下載樣例最適化表單[。 ](assets/sms-2fa-verification-af.zip)此示例表單使用作為本文一部分提供的表單資料模型的服務調用。
* 從[表單和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)將表單匯入AEM
* 在編輯模式中開啟表格。 開啟下列欄位的規則編輯器

![sms-send](assets/check-sms.PNG)

* 編輯與欄位關聯的規則。 提供您適當的API金鑰
* 儲存表格
* [預覽表](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 格並測試功能


