---
title: 測試Asset Compute背景工作
description: Asset Compute專案定義了一種模式，用於輕鬆建立及執行Asset Compute背景工作的測試。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 測試Asset Compute背景工作

Asset Compute專案定義了一種模式，用於輕鬆建立及執行Asset Compute背景工作[測試](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html)。

## 背景工作測試的剖析

Asset Compute Worker的測試會分成多個測試套裝，而每個測試套裝中，都會有一或多個測試案例宣告要測試的條件。

Asset Compute專案中的測試結構如下：

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

每個測試轉換可以有下列檔案：

+ `file.<extension>`
   + 要測試的Source檔案（副檔名可以是`.link`以外的任何專案）
   + 必填
+ `rendition.<extension>`
   + 預期的轉譯
   + 必要，但錯誤測試除外
+ `params.json`
   + 單一轉譯JSON指示
   + 選用
+ `validate`
   + 此指令碼會以引數取得預期的和實際的轉譯檔案路徑，如果結果為良好，則必須傳回退出代碼0；如果驗證或比較失敗，則必須傳回非零的退出代碼。
   + 選擇性，預設為`diff`命令
   + 使用包住Docker run命令的外殼指令碼，以使用不同的驗證工具
+ `mock-<host-name>.json`
   + [模擬外部服務呼叫](https://www.mock-server.com/mock_server/creating_expectations.html)的JSON格式化HTTP回應。
   + 選用，僅在背景工作程式碼提出自己的HTTP請求時使用

## 撰寫測試案例

此測試案例會判斷輸入檔案(`file.jpg`)的引數化輸入(`params.json`)會產生預期的PNG轉譯(`rendition.png`)。

1. 首先，刪除位於`/test/asset-compute/simple-worker`的自動產生`simple-worker`測試案例，因為這是無效的，因為我們的背景工作不再只是將來源複製到轉譯。
1. 在`/test/asset-compute/worker/success-parameterized`建立新的測試案例資料夾，以測試產生PNG轉譯的背景工作程式是否成功執行。
1. 在`success-parameterized`資料夾中，新增此測試案例的測試[輸入檔案](./assets/test/success-parameterized/file.jpg)，並將其命名為`file.jpg`。
1. 在`success-parameterized`資料夾中，新增名稱為`params.json`的新檔案，以定義背景工作程式的輸入引數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   這些是傳遞至[開發工具的Asset Compute設定檔定義](../develop/development-tool.md)的相同機碼/值，減去`worker`機碼。

1. 將預期的[轉譯檔案](./assets/test/success-parameterized/rendition.png)新增到此測試案例，並將其命名為`rendition.png`。 此檔案代表指定輸入`file.jpg`之背景工作程式的預期輸出。
1. 從命令列，執行`aio app test`以執行專案根目錄測試
   + 確保已安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援的Docker映像
   + 終止任何執行中的開發工具例項

![測試 — 成功](./assets/test/success-parameterized/result.png)

## 寫入錯誤檢查測試案例

此測試案例會進行測試，以確保Worker在`contrast`引數設為無效值時擲回適當的錯誤。

1. 在`/test/asset-compute/worker/error-contrast`建立新的測試案例資料夾，以測試背景工作程式因無效`contrast`引數值而執行的錯誤。
1. 在`error-contrast`資料夾中，新增此測試案例的測試[輸入檔案](./assets/test/error-contrast/file.jpg)，並將其命名為`file.jpg`。 此檔案的內容對此測試而言並不重要，它只是需要存在才能通過「損毀來源」檢查，以達成此測試案例驗證的`rendition.instructions`有效性檢查。
1. 在`error-contrast`資料夾中，新增名稱為`params.json`的新檔案，以定義具有下列內容之背景工作程式的輸入引數：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 將`contrast`引數設定為`10` （無效值，因為對比必須介於–1和1之間）以擲回`RenditionInstructionsError`。
   + 將`errorReason`機碼設定為與預期錯誤相關的「原因」，藉此判斷測試中擲回的錯誤是否正確。 此無效的對比引數擲回[自訂錯誤](../develop/worker.md#errors)、`RenditionInstructionsError`，因此將`errorReason`設定為此錯誤的原因，或`rendition_instructions_error`宣告擲回它。

1. 由於在錯誤執行期間不應產生轉譯，因此不需要`rendition.<extension>`檔案。
1. 執行命令`aio app test`，從專案的根目錄執行測試套件
   + 確保已安裝並啟動[Docker Desktop](../set-up/development-environment.md#docker)和支援的Docker映像
   + 終止任何執行中的開發工具例項

![測試 — 錯誤對比](./assets/test/error-contrast/result.png)

## Github上的測試案例

Github提供最終測試案例，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

+ [測試執行期間未產生轉譯](../troubleshooting.md#test-no-rendition-generated)
+ [測試產生不正確的轉譯](../troubleshooting.md#tests-generates-incorrect-rendition)
