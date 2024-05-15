---
title: 測試Asset compute背景工作
description: asset compute專案定義了一種模式，用於輕鬆建立及執行Asset compute背景工作的測試。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 測試Asset compute背景工作

asset compute專案定義了一種模式，用於輕鬆建立及執行 [asset compute背景工作者的測試](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## 背景工作測試的剖析

asset compute工作者的測試會分成多個測試套裝，而在每個測試套裝中，會有一或多個測試案例宣告要測試的條件。

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

每個測試轉換可以有下列檔案：

+ `file.<extension>`
   + 要測試的來源檔案(副檔名可以是 `.link`)
   + 必填
+ `rendition.<extension>`
   + 預期的轉譯
   + 必要，但錯誤測試除外
+ `params.json`
   + 單一轉譯JSON指示
   + 選用
+ `validate`
   + 此指令碼會以引數取得預期的和實際的轉譯檔案路徑，如果結果為良好，則必須傳回退出代碼0；如果驗證或比較失敗，則必須傳回非零的退出代碼。
   + 選擇性，預設為 `diff` 命令
   + 使用包住Docker run命令的外殼指令碼，以使用不同的驗證工具
+ `mock-<host-name>.json`
   + JSON格式的HTTP回應，用於 [嘲弄外部服務呼叫](https://www.mock-server.com/mock_server/creating_expectations.html).
   + 選用，僅在背景工作程式碼提出自己的HTTP請求時使用

## 撰寫測試案例

此測試案例會確認引數化的輸入(`params.json`)作為輸入檔案(`file.jpg`)產生預期的PNG轉譯(`rendition.png`)。

1. 首先刪除自動產生的 `simple-worker` 測試案例位於 `/test/asset-compute/simple-worker` 由於這是無效的，因為我們的背景工作不再只是將來源複製到轉譯。
1. 於建立新的測試案例資料夾 `/test/asset-compute/worker/success-parameterized` 測試產生PNG轉譯之背景工作程式的成功執行。
1. 在 `success-parameterized` 資料夾，新增測試 [輸入檔案](./assets/test/success-parameterized/file.jpg) 用於此測試案例並命名它 `file.jpg`.
1. 在 `success-parameterized` 資料夾，新增名為的新檔案 `params.json` 會定義背景工作程式的輸入引數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   這些是傳遞至 [開發工具的Asset compute設定檔定義](../develop/development-tool.md)，減去 `worker` 機碼。

1. 新增預期的 [轉譯檔案](./assets/test/success-parameterized/rendition.png) 到此測試案例並命名它 `rendition.png`. 此檔案代表指定輸入之背景工作程式的預期輸出 `file.jpg`.
1. 從命令列，執行以測試專案根目錄 `aio app test`
   + 確定 [Docker案頭](../set-up/development-environment.md#docker) 和支援的Docker映像已安裝並啟動
   + 終止任何執行中的開發工具例項

![測試 — 成功 ](./assets/test/success-parameterized/result.png)

## 寫入錯誤檢查測試案例

此測試案例會進行測試以確保工作者在 `contrast` 引數已設定為無效值。

1. 於建立新的測試案例資料夾 `/test/asset-compute/worker/error-contrast` 測試由於無效背景工作程式的錯誤執行 `contrast` 引數值。
1. 在 `error-contrast` 資料夾，新增測試 [輸入檔案](./assets/test/error-contrast/file.jpg) 用於此測試案例並命名它 `file.jpg`. 此檔案的內容對此測試而言並不重要，它只需存在即可通過「損壞的來源」檢查，以達到 `rendition.instructions` 有效性檢查，此測試案例可驗證。
1. 在 `error-contrast` 資料夾，新增名為的新檔案 `params.json` 定義背景工作程式的輸入引數及其內容：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 設定 `contrast` 引數至 `10`，為無效值，因為對比度必須介於–1到1之間，才能擲回 `RenditionInstructionsError`.
   + 透過設定 `errorReason` 與預期錯誤相關聯之「原因」的索引鍵。 此無效的對比引數會擲回 [自訂錯誤](../develop/worker.md#errors)， `RenditionInstructionsError`，因此請設定 `errorReason` 此錯誤的原因，或`rendition_instructions_error` 以判斷是否擲回。

1. 由於在錯誤執行期間不應產生轉譯，因此 `rendition.<extension>` 檔案為必要項。
1. 執行命令，從專案的根目錄執行測試套裝 `aio app test`
   + 確定 [Docker案頭](../set-up/development-environment.md#docker) 和支援的Docker映像已安裝並啟動
   + 終止任何執行中的開發工具例項

![測試 — 錯誤對比](./assets/test/error-contrast/result.png)

## Github上的測試案例

Github提供最終測試案例，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

+ [測試執行期間未產生轉譯](../troubleshooting.md#test-no-rendition-generated)
+ [測試產生不正確的轉譯](../troubleshooting.md#tests-generates-incorrect-rendition)
