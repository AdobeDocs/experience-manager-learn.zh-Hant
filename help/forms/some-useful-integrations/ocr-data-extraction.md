---
title: ocr資料擷取
description: 從政府核發的檔案擷取資料以填入表單。
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# ocr資料擷取

自動擷取各種政府核發檔案的資料，以填入最適化表單。

有許多組織提供此服務，只要他們有妥善記錄的REST API，您就可以使用資料整合功能輕鬆與AEM Forms整合。 為了這個教學課程的目的，我已使用[ID分析器](https://www.idanalyzer.com/)來示範已上傳檔案的OCR資料擷取。

依照下列步驟，使用ID分析器服務透過AEM Forms實作OCR資料擷取。

## 建立開發人員帳戶

使用[ID分析器](https://portal.idanalyzer.com/signin.html)建立開發人員帳戶。 記下API金鑰。 呼叫ID分析器服務的REST API時需要此金鑰。

## 建立Swagger/OpenAPI檔案

OpenAPI Specification （前身為Swagger Specification）是REST API的API說明格式。 OpenAPI檔案可讓您說明整個API，包括：

* 每個端點的可用端點(/users)和操作(GET /users、POST /users)
* 作業引數每個作業的輸入和輸出
驗證方法
* 聯絡資訊、授權、使用條款及其他資訊。
* API規格可以用YAML或JSON撰寫。 該格式簡單易學，且可供人類和機器讀取。

若要建立您的第一個swagger/OpenAPI檔案，請依照[OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)操作

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用[swagger編輯器](https://editor.swagger.io/)建立您的swagger檔案，以說明傳送及驗證使用簡訊傳送之OTP代碼的作業。 swagger檔案可以建立JSON或YAML格式。 可以從[這裡](assets/drivers-license-swagger.zip)下載已完成的swagger檔案

## 定義swagger檔案時的考量事項

* 需要定義
* 方法定義必須使用$ref
* 偏好定義消耗與產生區段
* 請勿定義內嵌要求內文引數或回應引數。 儘可能模組化。 例如，不支援下列定義

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

requestBody定義的參考支援下列專案

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Swagger檔案範例以供參考](assets/sample-swagger.json)

## 建立資料Source

若要將AEM/AEM Forms與協力廠商應用程式整合，我們必須在雲端服務設定中[建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)。 請使用[swagger檔案](assets/drivers-license-swagger.zip)建立您的資料來源。

## 建立表單資料模型

AEM Forms資料整合提供直覺式使用者介面，可建立和使用[表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 讓表單資料模型以先前步驟建立的資料來源為基礎。

![fdm](assets/test-dl-fdm.PNG)

## 建立使用者端程式庫

我們需要取得已上傳檔案的base64編碼字串。 然後，會將此base64編碼字串作為REST叫用的其中一個引數傳遞。
可以從這裡[下載使用者端資料庫。](assets/drivers-license-client-lib.zip)

## 建立最適化表單

將表單資料模型的POST引動與您的調適型表單整合，以從表單中的使用者上傳的檔案中擷取資料。 您可以自由建立自己的調適型表單，並使用表單資料模型的POST引動來傳送上傳檔案的base64編碼字串。

## 在您的伺服器上部署

如果您想要搭配API金鑰使用範例資產，請遵循下列步驟：

* [下載資料來源](assets/drivers-license-source.zip)並使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入至AEM
* [下載表單資料模型](assets/drivers-license-fdm.zip)並使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入至AEM
* [下載使用者端程式庫](assets/drivers-license-client-lib.zip)
* 可從這裡](assets/adaptive-form-dl.zip)下載最適化表單範例[。 此範例表單使用本文中提供的表單資料模型的服務引動。
* 從[Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)將表單匯入AEM
* 以[編輯模式開啟表單。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* 在apikey欄位中將API金鑰指定為預設值，並儲存變更
* 開啟Base 64字串欄位的規則編輯器。 當此欄位的值變更時，請注意服務引動過程。
* 儲存表單
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，上傳您的駕駛執照正面圖片
