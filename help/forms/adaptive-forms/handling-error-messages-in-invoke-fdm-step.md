---
title: 在工作流中捕獲表單資料模型服務中的錯誤消息
description: 從AEM Forms6.5.1開始，我們現在能夠捕獲在使用調用表單資料模型服務作為工作流中的步驟時生成的錯誤AEM消息。 工作流程.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# 調用表單資料模型服務步驟中捕獲錯誤消息

從AEM Forms6.5.1開始，我們現在可以選擇捕獲錯誤消息並指定驗證選項。 調用表單資料模型服務步驟已增強，以提供以下功能。

* 提供用於3層驗證（「OFF」、「BASIC」和「FULL」）的選項，以處理調用表單資料模型服務時遇到的異常。 3個選項依次表示檢查特定於資料庫的要求的更嚴格版本。
   ![驗證級別](assets/validation-level.PNG)

* 提供用於自定義工作流執行的複選框。 因此，即使「調用表單資料模型」步驟引發異常，用戶現在也可以靈活地繼續執行工作流執行。

* 儲存由驗證異常引起的錯誤的重要資訊。 已合併了三個自動完成類型變數選擇器，以選擇相關變數以儲存ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果異常不是DermisValidationException，則ErrorDetails將設定為null。
   ![捕獲錯誤消息](assets/fdm-error-details.PNG)

通過這些更改，「調用表單資料模型服務」步驟將確保輸入值符合交換器檔案中提供的資料約束。 例如，當accountId和balance值與swagger檔案中指定的資料約束不相容時，將引發以下錯誤消息。

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
