---
title: 在工作流程中擷取表單資料模型服務中的錯誤訊息
seo-title: 在工作流程中擷取表單資料模型服務中的錯誤訊息
description: 從AEM Forms 6.5.1開始，現在起，我們能夠擷取使用叫用表單資料模型服務(作為AEM工作流程中的步驟)時產生的錯誤訊息。 工作流程.
seo-description: 從AEM Forms 6.5.1開始，現在起，我們能夠擷取使用叫用表單資料模型服務(作為AEM工作流程中的步驟)時產生的錯誤訊息。 工作流程.
feature: 工作流程
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 1%

---


# 在調用表單資料模型服務步驟中捕獲錯誤消息

從AEM Forms 6.5.1開始，現在可以選擇擷取錯誤訊息並指定驗證選項。 已增強「叫用表單資料模型服務」步驟，以提供下列功能。

* 提供3層驗證（「OFF」、「BASIC」和「FULL」）的選項，以處理調用表單資料模型服務時遇到的異常。 這3個選項依次表示檢查資料庫特定要求的更嚴格版本。
   ![驗證層級](assets/validation-level.PNG)

* 提供用於自定義工作流執行的複選框。 因此，即使「叫用表單資料模型」步驟擲回例外狀況，使用者現在仍可彈性地繼續進行「工作流程執行」。

* 儲存因驗證異常而出現的錯誤的重要資訊。 已整合三個自動完成類型變數選取器，以選取相關變數以儲存ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果異常不是DermisValidationException，則ErrorDetails將設定為Null。
   ![捕獲錯誤消息](assets/fdm-error-details.PNG)

經過這些變更後，「叫用表單資料模型服務」步驟將確保輸入值符合Swagger檔案中提供的資料限制。 例如，當accountId和餘額值不符合swagger檔案中指定的資料限制時，將擲回下列錯誤訊息。

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


