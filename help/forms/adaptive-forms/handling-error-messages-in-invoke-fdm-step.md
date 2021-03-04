---
title: 在工作流中捕獲表單資料模型服務中的錯誤消息
seo-title: 在工作流中捕獲表單資料模型服務中的錯誤消息
description: 從AEM Forms6.5.1開始，我們現在可以擷取使用叫用表單資料模型服務(Form Data Model Service)所產生的錯誤訊息，做為工作流程中的AEM步驟。 工作流程.
seo-description: 從AEM Forms6.5.1開始，我們現在可以擷取使用叫用表單資料模型服務(Form Data Model Service)所產生的錯誤訊息，做為工作流程中的AEM步驟。 工作流程.
feature: 工作流程
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 1%

---


# 在調用表單資料模型服務步驟中捕獲錯誤消息

從AEM Forms6.5.1開始，我們現在可以選擇擷取錯誤訊息並指定驗證選項。 已增強「叫用表單資料模型服務」步驟，以提供下列功能。

* 提供3層驗證（「OFF」、「BASIC」和「FULL」）選項，以處理調用表單資料模型服務時遇到的異常。 這3個選項依次表示檢查特定資料庫要求的更嚴格版本。
   ![驗證級別](assets/validation-level.PNG)

* 提供用於自定義工作流執行的複選框。 因此，即使「叫用表單資料模型」步驟拋出「例外」，使用者現在仍可以靈活執行「工作流執行」。

* 儲存由於驗證異常而引起的錯誤的重要資訊。 已整合三個自動完成類型的變數選擇器，以選取相關的變數來儲存ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果例外不是DermisValidationException，則ErrorDetails將設定為null。
   ![捕獲錯誤消息](assets/fdm-error-details.PNG)

透過這些變更，「叫用表單資料模型服務」步驟將確保輸入值符合Swagger檔案中提供的資料限制。 例如，當accountId和balance值與Swagger檔案中指定的資料限制不相容時，會擲回下列錯誤訊息。

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


