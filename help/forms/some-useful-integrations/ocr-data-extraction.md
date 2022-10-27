---
title: OCR資料擷取
description: 從政府核發的檔案中擷取資料以填入表單。
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 1%

---

# OCR資料擷取

自動從各種政府核發的檔案中擷取資料，以填入最適化表單。

有許多組織提供此服務，只要他們有記錄完善的REST API，您就可以使用資料整合功能，輕鬆與AEM Forms整合。 在本教學課程中，我已使用 [ID分析器](https://www.idanalyzer.com/) 演示已上載文檔的OCR資料提取。

請依照下列步驟，使用ID Analyzer服務，透過AEM Forms實作OCR資料擷取。

## 建立開發人員帳戶

使用建立開發人員帳戶 [ID分析器](https://portal.idanalyzer.com/signin.html). 記下API金鑰。 需要此金鑰才能叫用ID Analyzer服務的REST API。

## 建立Swagger/OpenAPI檔案

OpenAPI規格（原稱Swagger規格）是REST API的API說明格式。 OpenAPI檔案可讓您描述整個API，包括：

* 每個端點上的可用端點(/users)和操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯絡資訊、授權、使用條款和其他資訊。
* API規格可以用YAML或JSON編寫。 該格式便於人和機器學習和閱讀。

若要建立您的第一個swagger/OpenAPI檔案，請遵循 [OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用 [swagger編輯器](https://editor.swagger.io/) 建立swagger檔案，以說明使用簡訊傳送及驗證OTP程式碼的操作。 可以使用JSON或YAML格式建立Swagger檔案。 已完成的Swagger檔案可從 [此處](assets/drivers-license-swagger.zip)

## 定義Swagger檔案時的考量事項

* 需要定義
* $ref需要用於方法定義
* 偏好使用並生成已定義的節
* 請勿定義內嵌要求內文參數或回應參數。 盡量模組化。 例如，不支援下列定義

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

參考requestBody定義支援下列項目

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [參考的Swagger檔案範例](assets/sample-swagger.json)

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要 [建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲端服務設定中。 請使用 [swagger檔案](assets/drivers-license-swagger.zip) 來建立您的資料來源。

## 建立表單資料模型

AEM Forms資料整合提供直覺式的使用者介面，讓您建立及使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 將表單資料模型建立在先前步驟中建立的資料來源上。

![fdm](assets/test-dl-fdm.PNG)

## 建立客戶端庫

我們需要上傳檔案的base64編碼字串。 接著，系統會將此base64編碼字串作為REST呼叫的參數之一傳遞。
可下載用戶端程式庫 [從這裡。](assets/drivers-license-client-lib.zip)

## 建立最適化表單

將表單資料模型的POST調用與最適化表單整合，以從表單中的用戶上載的文檔中提取資料。 您可以自由建立自己的最適化表單，並使用表單資料模型的POST叫用，傳送已上傳檔案的base64編碼字串。

## 在伺服器上部署

如果您想搭配API金鑰使用範例資產，請遵循下列步驟：

* [下載資料來源](assets/drivers-license-source.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [下載表單資料模型](assets/drivers-license-fdm.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [下載客戶端庫](assets/drivers-license-client-lib.zip)
* 下載範例最適化表單可以 [從此處下載](assets/adaptive-form-dl.zip). 此示例表單使用本文中提供的表單資料模型的服務調用。
* 將表單從匯入AEM [Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在中開啟表單 [編輯模式。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* 在apikey欄位中指定您的API金鑰為預設值，並儲存您的變更
* 開啟「Base 64 String」欄位的規則編輯器。 更改此欄位的值時，請注意服務調用。
* 儲存表單
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，上傳您的駕照前面的圖片
