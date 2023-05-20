---
title: SMS雙因素身份驗證
description: 添加額外的安全層，以幫助確認用戶在想要執行某些活動時的身份
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

# 驗證用戶是否使用其手機號碼

SMS雙因素驗證（雙因素驗證）是一種安全驗證過程，通過用戶登錄網站、軟體或應用程式來觸發該過程。 在登錄過程中，用戶被自動地發送SMS到其包含唯一數字代碼的移動號碼。

有許多組織提供此服務，只要他們有詳細的REST API文檔，您就可以使用AEM Forms的資料整合功能輕鬆整合AEM Forms。 在本教程中，我使用 [內克莫](https://developer.nexmo.com/verify/overview) 演示SMS 2FA使用案例。

按照以下步驟使用Nexmo Verify服務與AEM Forms一起實施SMS 2FA。

## 建立開發人員帳戶

建立開發人員帳戶 [內克莫](https://dashboard.nexmo.com/sign-in)。 記下API密鑰和API密鑰。 調用Nexmo服務的REST API時需要這些密鑰。

## 建立Swagger/OpenAPI檔案

OpenAPI規範（以前稱為Swagger規範）是REST API的API說明格式。 OpenAPI檔案允許您描述整個API，包括：

* 可用端點(/users)和每個端點上的操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯繫資訊、許可證、使用條款和其他資訊。
* API規範可以用YAML或JSON編寫。 該格式易於學習，對人和機器都易讀。

要建立第一個swagger/OpenAPI檔案，請按照 [OpenAPI文檔](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規範2.0版(fka Swagger)。

使用 [斯瓦格編輯器](https://editor.swagger.io/) 建立swagger檔案，以描述使用SMS發送和驗證OTP代碼的操作。 可以使用JSON或YAML格式建立swagger檔案。 已完成的交換器檔案可從 [這裡](assets/two-factore-authentication-swagger.zip)

## 建立資料源

要將AEM/AEM Forms與第三方應用程式整合，我們需要 [建立資料源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲服務配置中。

## 建立表單資料模型

AEM Forms資料整合提供直觀的用戶介面，用於建立和使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表單資料模型依賴於資料源來交換資料。
完成的表單資料模型可以 [從此處下載](assets/sms-2fa-fdm.zip)

![fd](assets/2FA-fdm.PNG)

## 建立自適應窗體

將表單資料模型的POST調用與自適應表單整合，以驗證用戶在表單中輸入的行動電話號碼。 您可以自由地建立自己的自適應表單，並使用表單資料模型的POST調用根據您的要求發送和驗證OTP代碼。

如果要將示例資產與API密鑰一起使用，請執行以下步驟：

* [下載表單資料模型](assets/sms-2fa-fdm.zip) 導入到使AEM用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 下載示例自適應表單可以 [從此處下載](assets/sms-2fa-verification-af.zip)。 此示例表單使用作為本文一部分提供的表單資料模型的服務調用。
* 將窗體從 [Forms和文檔用戶介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在編輯模式下開啟窗體。 開啟以下欄位的規則編輯器

![簡訊](assets/check-sms.PNG)

* 編輯與欄位關聯的規則。 提供適當的API密鑰
* 保存窗體
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 和test功能
