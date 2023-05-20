---
title: 使用OTP驗證用戶
description: 使用OTP驗證與應用程式號關聯的移動號碼。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# 使用OTP驗證用戶

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

要將AEM/AEM Forms與第三方應用程式整合，我們需要 [使用swagger檔案的基於REST的資料源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲服務配置中。 已完成的資料源將作為本課程資產的一部分提供給您。

## 建立表單資料模型

AEM Forms資料整合提供直觀的用戶介面，用於建立和使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表單資料模型依賴於資料源來交換資料。
完成的表單資料模型可以 [從此處下載](assets/sms-2fa-fdm.zip)

![fd](assets/2FA-fdm.PNG)

[建立主窗體](./create-the-main-adaptive-form.md)
