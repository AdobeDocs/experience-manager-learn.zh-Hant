---
title: OCR資料提取
description: 從政府發佈的文檔中提取資料以填充表單。
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

# OCR資料提取

自動從各種政府發佈的文檔中提取資料以填充自適應表單。

有許多組織提供此服務，只要他們有詳細的REST API文檔，您就可以使用資料整合功能輕鬆與AEM Forms整合。 在本教程中，我使用 [ID分析器](https://www.idanalyzer.com/) 演示上載文檔的OCR資料提取。

接下來是使用ID Analyzer服務使用AEM Forms實現OCR資料提取。

## 建立開發人員帳戶

建立開發人員帳戶 [ID分析器](https://portal.idanalyzer.com/signin.html)。 記下API密鑰。 調用ID Analyzer服務的REST API時需要此鍵。

## 建立Swagger/OpenAPI檔案

OpenAPI規範（以前稱為Swagger規範）是REST API的API說明格式。 OpenAPI檔案允許您描述整個API，包括：

* 可用端點(/users)和每個端點上的操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯繫資訊、許可證、使用條款和其他資訊。
* API規範可以用YAML或JSON編寫。 該格式易於學習，對人和機器都易讀。

要建立第一個swagger/OpenAPI檔案，請按照 [OpenAPI文檔](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規範2.0版(fka Swagger)。

使用 [斯瓦格編輯器](https://editor.swagger.io/) 建立swagger檔案，以描述使用SMS發送和驗證OTP代碼的操作。 可以使用JSON或YAML格式建立swagger檔案。 已完成的交換器檔案可從 [這裡](assets/drivers-license-swagger.zip)

## 定義swagger檔案時的注意事項

* 需要定義
* $ref需要用於方法定義
* 希望定義消耗和生成節
* 不定義內聯請求正文參數或響應參數。 盡量模組化。 例如，不支援以下定義

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

對requestBody定義的引用支援以下內容

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [示例Swagger檔案供參考](assets/sample-swagger.json)

## 建立資料源

要將AEM/AEM Forms與第三方應用程式整合，我們需要 [建立資料源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲服務配置中。 請使用 [swagger檔案](assets/drivers-license-swagger.zip) 建立資料源。

## 建立表單資料模型

AEM Forms資料整合提供直觀的用戶介面，用於建立和使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 將表單資料模型建立在前面步驟中建立的資料源上。

![fd](assets/test-dl-fdm.PNG)

## 建立客戶端庫

我們需要獲取上載文檔的base64編碼字串。 然後，此base64編碼字串作為REST調用的參數之一傳遞。
可以下載客戶端庫 [從這裡。](assets/drivers-license-client-lib.zip)

## 建立自適應窗體

將表單資料模型的POST調用與自適應表單整合，以從表單中由用戶上載的文檔中提取資料。 您可以建立自己的自適應表單並使用表單資料模型的POST調用來發送上載文檔的base64編碼字串。

## 在伺服器上部署

如果要將示例資產與API密鑰一起使用，請執行以下步驟：

* [下載資料源](assets/drivers-license-source.zip) 導入到使AEM用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [下載表單資料模型](assets/drivers-license-fdm.zip) 導入到使AEM用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [下載客戶端庫](assets/drivers-license-client-lib.zip)
* 下載示例自適應表單可以 [從此處下載](assets/adaptive-form-dl.zip)。 此示例表單使用作為本文一部分提供的表單資料模型的服務調用。
* 將窗體從 [Forms和文檔用戶介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在中開啟窗體 [編輯模式。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* 在apikey欄位中指定API密鑰作為預設值並保存更改
* 開啟「基64字串」欄位的規則編輯器。 當此欄位的值更改時，請注意服務調用。
* 保存窗體
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，上載您的駕照前面的圖片
