---
title: 在工作流程中擷取表單資料模型服務中的錯誤訊息
description: 從AEM Forms 6.5.1開始，現在起，我們能夠擷取使用叫用表單資料模型服務(作為AEM工作流程中的步驟)時產生的錯誤訊息。 工作流程.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 在調用表單資料模型服務步驟中捕獲錯誤消息

從AEM Forms 6.5.1開始，我們現在可以選擇擷取錯誤訊息並指定驗證選項。 已增強「叫用表單資料模型服務」步驟，以提供下列功能。

* 提供3層驗證（「OFF」、「BASIC」和「FULL」）的選項，以處理調用表單資料模型服務時遇到的異常。 這3個選項依次表示檢查資料庫特定要求的更嚴格版本。
   ![驗證層級](assets/validation-level.PNG)

* 提供用於自定義工作流執行的複選框。 因此，即使「叫用表單資料模型」步驟擲回例外狀況，使用者現在仍可彈性地繼續執行工作流程執行。

* 儲存因驗證異常而出現的錯誤的重要資訊。 已整合三個自動完成類型變數選取器，以選取相關變數以儲存ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果異常不是DermisValidationException，則ErrorDetails將設定為Null。
   ![捕獲錯誤消息](assets/fdm-error-details.PNG)

經過這些變更後，「叫用表單資料模型服務」步驟可確保輸入值符合Swagger檔案中提供的資料限制。 例如，當accountId和餘額值不符合swagger檔案中指定的資料限制時，會擲回下列錯誤訊息。

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
