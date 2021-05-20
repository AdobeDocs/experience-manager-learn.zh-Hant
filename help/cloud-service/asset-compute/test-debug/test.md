---
title: 測試Asset compute工作
description: asset compute項目定義了一種模式，以便輕鬆建立和執行Asset compute工作的測試。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 0%

---


# 測試Asset compute工作

asset compute項目定義了一種模式，用於輕鬆建立和執行Asset compute工作者](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)的測試。[

## 工人測試解剖

asset compute背景工作的測試會分為測試套裝，而在每個測試套裝中，有一或多個測試案例主張要測試的條件。

asset compute專案中的測試結構如下：

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
   + 要測試的源檔案（副檔名可以是`.link`以外的任何值）
   + 必要
+ `rendition.<extension>`
   + 預期的轉譯
   + 必要，錯誤測試除外
+ `params.json`
   + 單一轉譯JSON指示
   + 可選
+ `validate`
   + 指令碼將預期和實際的轉譯檔案路徑作為參數，如果結果正常，則必須返回退出代碼0；如果驗證或比較失敗，則返回非零退出代碼。
   + 可選，預設為`diff`命令
   + 使用包裝Docker運行命令的Shell指令碼，以使用不同的驗證工具
+ `mock-<host-name>.json`
   + [模仿外部服務呼叫](https://www.mock-server.com/mock_server/creating_expectations.html)的JSON格式化HTTP回應。
   + 選用，僅在背景程式碼自行提出HTTP要求時使用

## 編寫測試用例

此測試實例聲明輸入檔案(`file.jpg`)的參數化輸入(`params.json`)生成預期的PNG格式副本(`rendition.png`)。

1. 首先，請刪除`/test/asset-compute/simple-worker`處自動生成的`simple-worker`測試案例，因為這無效，因為我們的工作人員不再只是將源複製到格式副本。
1. 在`/test/asset-compute/worker/success-parameterized`處建立新的測試案例資料夾，以測試是否成功執行產生PNG轉譯的工作器。
1. 在`success-parameterized`資料夾中，添加此測試案例的測試[輸入檔案](./assets/test/success-parameterized/file.jpg)，並將其命名為`file.jpg`。
1. 在`success-parameterized`資料夾中，添加一個名為`params.json`的新檔案，該檔案定義工作器的輸入參數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   這些是傳入[開發工具的Asset compute設定檔定義](../develop/development-tool.md)的相同索引鍵/值，減去`worker`索引鍵。

1. 將預期的[轉譯檔案](./assets/test/success-parameterized/rendition.png)新增至此測試案例，並將其命名為`rendition.png`。 此檔案表示給定輸入`file.jpg`的工作器的預期輸出。
1. 從命令行中，通過執行`aio app test`來運行項目根
   + 確保安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援的Docker映像
   + 終止任何正在運行的開發工具實例

![測試 — 成功  ](./assets/test/success-parameterized/result.png)

## 編寫錯誤檢查測試用例

此測試案例旨在確保當`contrast`參數設定為無效值時，工作者擲回適當錯誤。

1. 在`/test/asset-compute/worker/error-contrast`處建立新的測試用例資料夾，以測試由於無效`contrast`參數值導致的工作器的重新執行。
1. 在`error-contrast`資料夾中，添加此測試案例的測試[輸入檔案](./assets/test/error-contrast/file.jpg)，並將其命名為`file.jpg`。 此檔案的內容對此測試不重要，它只需要存在才能通過「損壞源」檢查，才能達到`rendition.instructions`有效性檢查，該測試案例才能驗證。
1. 在`error-contrast`資料夾中，添加一個名為`params.json`的新檔案，該檔案定義了包含內容的工作器的輸入參數：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 將`contrast`參數設定為`10`，這是無效值，因為對比度必須介於–1和1之間，才會引發`RenditionInstructionsError`。
   + 將`errorReason`鍵設定為與預期錯誤相關聯的「reason」，以在測試中擲回適當錯誤。 此無效的對比度參數會擲回[自訂錯誤](../develop/worker.md#errors)、`RenditionInstructionsError`，因此將`errorReason`設為此錯誤的原因，或將`rendition_instructions_error`設為斷言擲回。

1. 由於執行錯誤期間不應產生任何轉譯，因此不需要`rendition.<extension>`檔案。
1. 通過執行命令`aio app test`從項目的根目錄運行測試套件
   + 確保安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援的Docker映像
   + 終止任何正在運行的開發工具實例

![測試 — 錯誤對比](./assets/test/error-contrast/result.png)

## Github的測試案例

Github提供最終測試案例，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

+ [測試執行期間未產生任何轉譯](../troubleshooting.md#test-no-rendition-generated)
+ [測試會產生錯誤的轉譯](../troubleshooting.md#tests-generates-incorrect-rendition)
