---
title: 將AEM Forms與SendGrid整合
description: 利用AEM Forms的SengGrid雲端型電子郵件傳遞平台。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# 將AEM Forms與SendGrid整合

歡迎使用本技術指南，我們將在此指南中探索使用AEM Forms的SendGrid動態範本傳送電子郵件的程式。 本指南旨在讓您清楚瞭解如何運用動態範本，以有效個人化您的電子郵件內容。

動態範本可讓您建立電子郵件範本，這些範本可以根據最適化表單中擷取的資料，向收件者顯示不同的內容。 透過利用個人化變數，您可以提供目標鎖定和自訂的電子郵件體驗，以引起觀眾的共鳴。

此外，我們將深入探討Swagger檔案的使用，其可讓您納入客戶名稱和電子郵件地址，並選取適當的動態電子郵件範本，進一步個人化您的電子郵件。

依照本檔案的逐步指示，利用SendGrid動態範本和AEM Forms的強大功能，將您的電子郵件通訊提升至新的參與度和關聯性等級。 讓我們開始吧！

## 先決條件

繼續使用AEM Forms的SendGrid動態範本傳送電子郵件之前，請確定您已符合下列先決條件：

1. **SendGrid帳戶**：在[https://sendgrid.com](https://sendgrid.com)註冊SendGrid帳戶，以存取其電子郵件傳遞服務。 您需要帳戶認證，才能將SendGrid與AEM Forms整合。
1. **熟悉建立資料來源**：擁有在AEM Forms中建立資料來源的工作知識。 如有需要，請參閱有關[建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=zh-Hant)的檔案以取得詳細指示。
1. **熟悉表單資料模型**：瞭解AEM Forms中表單資料模型的概念。 如有需要，請檢閱有關[建立表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=zh-Hant)的檔案，以確保您具備必要的瞭解。

只要符合這些必要條件，您就具備了必要的知識和資源，可以使用AEM Forms的SendGrid動態範本有效傳送電子郵件。

## 資產範例

本文提供的範例資產包括：

* **[Swagger檔案](assets/SendGridWithDynamicTemplate.yaml)**：此檔案可讓您使用動態電子郵件範本傳送電子郵件。 它提供必要的規格和設定，以與SendGrid和AEM Forms整合，實現順暢的電子郵件傳送。

您可以利用隨附的Swagger檔案，作為透過動態範本實作電子郵件功能的參考或起點。

## 測試指示

若要測試本指南中說明的功能，請遵循下列步驟：

1. 下載資產資料夾中提供的[swagger檔案](assets/SendGridWithDynamicTemplate.yaml)。
2. 使用下載的Swagger檔案和您的SendGrid認證建立Restful資料來源。
3. 根據Restful資料來源建立表單資料模型。
4. 根據您的需求，叫用表單資料模型的`mail/send` POST作業。 例如，您可以在按鈕點選時觸發電子郵件，或將其納入您的AEM Forms工作流程的一部分。

服務的裝載範例如下。 將預留位置值取代為您自己的資料：

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

請確定`template_id`對應至您SendGrid動態電子郵件範本的識別碼，且電子郵件地址有效且由SendGrid驗證。 `personalizations`區段中的值可讓您使用使用者從最適化表單輸入的資料來個人化電子郵件。

按照這些步驟並自訂所提供的裝載，您就能有效地測試SendGrid動態範本與AEM Forms的整合。
