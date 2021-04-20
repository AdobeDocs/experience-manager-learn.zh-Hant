---
title: 測試Asset compute工作者
description: asset compute項目定義了一種模式，以便輕鬆建立和執行Asset compute工作者的測試。
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '639'
ht-degree: 0%

---


# 測試Asset compute工作者

asset compute項目定義了一種模式，用於輕鬆建立和執行Asset compute工作者的[測試。](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)

## 工人測驗的剖析

asset compute工人的測試分為測試套件，而在每個測試套件中，有一或多個測試案例宣告要測試的條件。

asset compute項目中的測試結構如下：

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

每個測試轉播都可以有下列檔案：

+ `file.<extension>`
   + 要測試的源檔案（副檔名可以是`.link`以外的任何內容）
   + 必要
+ `rendition.<extension>`
   + 預期的轉譯
   + 必要，錯誤測試除外
+ `params.json`
   + 單一轉譯JSON指示
   + 可選
+ `validate`
   + 一種指令碼，其將預期和實際的轉譯檔案路徑視為引數，若結果正常，則必須傳回退出代碼0；若驗證或比較失敗，則必須傳回非零退出代碼。
   + 可選，預設為`diff`命令
   + 使用外殼指令碼，該指令碼將docker運行命令包住，以使用不同的驗證工具
+ `mock-<host-name>.json`
   + 針對[模擬外部服務呼叫](https://www.mock-server.com/mock_server/creating_expectations.html)的JSON格式HTTP回應。
   + 可選，僅在工作程式碼發出HTTP要求時使用

## 編寫測試案例

此測試案例斷言輸入檔案(`file.jpg`)的參數化輸入(`params.json`)會產生預期的PNG轉譯(`rendition.png`)。

1. 首先刪除自動生成的`simple-worker`測試案例（位於`/test/asset-compute/simple-worker`），因為該案例無效，因為我們的員工不再只是將源複製到格式副本。
1. 在`/test/asset-compute/worker/success-parameterized`建立新的測試案例資料夾，以測試產生PNG轉譯的工作者的成功執行。
1. 在`success-parameterized`資料夾中，新增此測試案例的測試[輸入檔案](./assets/test/success-parameterized/file.jpg)，並將它命名為`file.jpg`。
1. 在`success-parameterized`資料夾中，添加一個名為`params.json`的新檔案，該檔案定義工作器的輸入參數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   這些是傳入[開發工具的Asset compute配置檔案定義](../develop/development-tool.md)中的相同鍵／值，減去`worker`鍵。
1. 將預期的[轉譯檔案](./assets/test/success-parameterized/rendition.png)新增至此測試案例，並將它命名為`rendition.png`。 此檔案表示給定輸入`file.jpg`的工作器預期輸出。
1. 在命令行中，通過執行`aio app test`來運行項目根
   + 確保已安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援Docker映像
   + 終止任何正在運行的開發工具實例

![測試——成功  ](./assets/test/success-parameterized/result.png)

## 編寫錯誤檢查測試案例

此測試案例測試可確保當`contrast`參數設為無效值時，工作者拋出適當的錯誤。

1. 在`/test/asset-compute/worker/error-contrast`建立新的測試案例資料夾，以測試因`contrast`參數值無效而導致的工作器重複執行。
1. 在`error-contrast`資料夾中，新增此測試案例的測試[輸入檔案](./assets/test/error-contrast/file.jpg)，並將它命名為`file.jpg`。 此檔案的內容對本測試並不重要，只需存在即可通過「Corrupt source」（損壞來源）檢查，才能達到`rendition.instructions`有效性檢查，此測試案例即可驗證。
1. 在`error-contrast`資料夾中，添加一個名為`params.json`的新檔案，該檔案定義了具有以下內容的工作器的輸入參數：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 將`contrast`參數設為`10`，表示一個無效值，因為對比度必須介於-1和1之間，以拋出`RenditionInstructionsError`。
   + 將`errorReason`鍵設為與預期錯誤相關的&quot;reason&quot;，以在測試中提出正確的錯誤。 此無效的對比參數會拋出[自訂錯誤](../develop/worker.md#errors)、`RenditionInstructionsError`，因此請將`errorReason`設為此錯誤的原因，或將`rendition_instructions_error`設為斷言它已拋出。

1. 由於在執行錯誤時不應產生任何轉譯，因此不需要`rendition.<extension>`檔案。
1. 通過執行命令`aio app test`從項目的根目錄運行測試套件
   + 確保已安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援Docker映像
   + 終止任何正在運行的開發工具實例

![測試——錯誤對比](./assets/test/error-contrast/result.png)

## Github測試案例

Github上提供最終測試案例，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute-worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

+ [測試執行期間未產生轉譯](../troubleshooting.md#test-no-rendition-generated)
+ [測試會產生錯誤的轉譯](../troubleshooting.md#tests-generates-incorrect-rendition)
