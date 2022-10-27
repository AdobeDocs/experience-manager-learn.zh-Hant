---
title: SMS雙因素驗證
description: 新增額外的安全層，以協助在使用者想執行特定活動時確認其身分
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 使用行動電話號碼驗證使用者

SMS雙因素驗證（Dual Factor Authentication，雙因素驗證）是一種安全驗證過程，通過登錄到網站、軟體或應用程式的用戶觸發。 在登入程式中，系統會自動將簡訊傳送至包含唯一數值代碼的行動號碼。

有許多組織提供此服務，只要他們有記錄完善的REST API，您就能使用AEM Forms的資料整合功能，輕鬆整合AEM Forms。 在本教學課程中，我已使用 [Nexmo](https://developer.nexmo.com/verify/overview) 以展示SMS 2FA使用案例。

依照下列步驟，使用Nexmo Verify服務，透過AEM Forms實作SMS 2FA。

## 建立開發人員帳戶

使用建立開發人員帳戶 [Nexmo](https://dashboard.nexmo.com/sign-in). 記下API金鑰和API密鑰。 需要這些金鑰才能叫用Nexmo服務的REST API。

## 建立Swagger/OpenAPI檔案

OpenAPI規格（原稱Swagger規格）是REST API的API說明格式。 OpenAPI檔案可讓您描述整個API，包括：

* 每個端點上的可用端點(/users)和操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯絡資訊、授權、使用條款和其他資訊。
* API規格可以用YAML或JSON編寫。 該格式便於人和機器學習和閱讀。

若要建立您的第一個swagger/OpenAPI檔案，請遵循 [OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用 [swagger編輯器](https://editor.swagger.io/) 建立swagger檔案，以說明使用簡訊傳送及驗證OTP程式碼的操作。 可以使用JSON或YAML格式建立Swagger檔案。 已完成的Swagger檔案可從 [此處](assets/two-factore-authentication-swagger.zip)

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要 [建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲端服務設定中。

## 建立表單資料模型

AEM Forms資料整合提供直覺式的使用者介面，讓您建立及使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 表單資料模型依賴資料來源來交換資料。
完成的表單資料模型可以 [從此處下載](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 建立最適化表單

將表單資料模型的POST調用與最適化表單整合，以驗證使用者在表單中輸入的行動電話號碼。 您可以自由建立自己的最適化表單，並使用表單資料模型的POST叫用，根據您的需求傳送及驗證OTP程式碼。

如果您想搭配API金鑰使用範例資產，請遵循下列步驟：

* [下載表單資料模型](assets/sms-2fa-fdm.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 下載範例最適化表單可以 [從此處下載](assets/sms-2fa-verification-af.zip). 此示例表單使用本文中提供的表單資料模型的服務調用。
* 將表單從匯入AEM [Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在編輯模式中開啟表單。 開啟下列欄位的規則編輯器

![sms傳送](assets/check-sms.PNG)

* 編輯與欄位相關聯的規則。 提供您適當的API金鑰
* 儲存表單
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 並測試功能
