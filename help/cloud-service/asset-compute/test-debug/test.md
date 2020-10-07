---
title: 測試資產計算工作者
description: 「資產計算」項目定義了一種模式，以便輕鬆建立和執行資產計算工作者的測試。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---


# 測試資產計算工作者

「資產計算」項目定義了一種模式，以便輕鬆建立和執 [行資產計算工作者的測試](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)。

## 工人測驗的剖析

「資產計算」工作者的測試會分割為測試套裝，而在每個測試套裝中，有一或多個測試案例會提出要測試的條件。

「資產計算」項目中的測試結構如下：

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

每個測試轉播都可以有下列檔案：

+ `file.<extension>`
   + 要測試的來源檔案(副檔名可以是除外的任何 `.link`)
   + 必要
+ `rendition.<extension>`
   + 預期的轉譯
   + 必要，錯誤測試除外
+ `params.json`
   + 單一轉譯JSON指示
   + 可選
+ `validate`
   + 一種指令碼，其將預期和實際的轉譯檔案路徑視為引數，若結果正常，則必須傳回退出代碼0；若驗證或比較失敗，則必須傳回非零退出代碼。
   + 可選，預設為命 `diff` 令
   + 使用外殼指令碼，該指令碼將docker運行命令包住，以使用不同的驗證工具
+ `mock-<host-name>.json`
   + 模擬外部服務呼叫的JSON [格式化HTTP回應](https://www.mock-server.com/mock_server/creating_expectations.html)。
   + 可選，僅在工作程式碼發出HTTP要求時使用

## 編寫測試案例

此測試案例斷言輸入檔案(`params.json`)的參數化輸入(`file.jpg`)會產生預期的PNG轉譯(`rendition.png`)。

1. 請先刪除自動產生的測 `simple-worker` 試案例，因 `/test/asset-compute/simple-worker` 為這是無效的，因為我們的員工不再只是將來源複製到轉譯。
1. 在上建立新的測試案例資料夾， `/test/asset-compute/worker/success-parameterized` 以測試生成PNG格式副本的工作器是否成功執行。
1. 在資料 `success-parameterized` 夾中，新增此測 [試案例的測試輸入檔](./assets/test/success-parameterized/file.jpg) ，並為其命名 `file.jpg`。
1. 在資料夾 `success-parameterized` 中，添加一個名為的新檔案， `params.json` 該檔案定義了工作器的輸入參數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   這些是傳入「開發工具」的「資產計算」 [描述檔定義中的相同鍵／值](../develop/development-tool.md)，減去 `worker` 鍵。
1. 將預期的轉 [譯檔案新增至](./assets/test/success-parameterized/rendition.png) 此測試案例，並為其命名 `rendition.png`。 此檔案表示工作器對給定輸入的預期輸出 `file.jpg`。
1. 在命令行中，通過執行 `aio app test`
   + 確 [保已安裝並啟動Docker Desktop](../set-up/development-environment.md#docker) ，並支援Docker映像
   + 終止任何正在運行的開發工具實例

![測試——成功 ](./assets/test/success-parameterized/result.png)

## 編寫錯誤檢查測試案例

此測試案例測試可確保當參數設為無效值時，工 `contrast` 作者會擲出適當的錯誤。

1. 在上建立新的測試案例資料夾 `/test/asset-compute/worker/error-contrast` ，以測試由於參數值無效而導致的工作器的重 `contrast` 復執行。
1. 在資料 `error-contrast` 夾中，新增此測 [試案例的測試輸入檔](./assets/test/error-contrast/file.jpg) ，並為其命名 `file.jpg`。 該檔案的內容對本測試不重要，只需存在即可通過「損壞源」檢查，才能達到有效性檢查，該測試案例 `rendition.instructions` 驗證了該檢查。
1. 在資料夾 `error-contrast` 中，添加一個名為的新檔案，該文 `params.json` 件定義了具有以下內容的工作器的輸入參數：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 將參 `contrast` 數設為 `10`無效值，因為對比必須介於-1和1之間，才能擲出 `RenditionInstructionsError`。
   + 在測試中，借由將索引鍵設定為與預期錯誤 `errorReason` 相關聯的「原因」，以宣告適當的錯誤。 此無效的對比參數會 [引發自訂錯誤](../develop/worker.md#errors), `RenditionInstructionsError`因此請將此 `errorReason` 錯誤設為原因`rendition_instructions_error` ，或是斷送。

1. 由於在執行錯誤期間不應產生任何轉譯，因此不 `rendition.<extension>` 需要任何檔案。
1. 執行命令，從專案的根目錄執行測試套裝 `aio app test`
   + 確 [保已安裝並啟動Docker Desktop](../set-up/development-environment.md#docker) ，並支援Docker映像
   + 終止任何正在運行的開發工具實例

![測試——錯誤對比](./assets/test/error-contrast/result.png)

## Github測試案例

Github上提供最終測試案例，網址為：

+ [aem-guides-wknd-asset-compute/test/asset-compute-worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

### 未產生轉譯

測試案例在不產生轉譯的情況下失敗。

+ __錯誤：__ 失敗：未產生轉譯。
+ __原因：__ 由於意外錯誤（如JavaScript語法錯誤），工作者無法產生轉譯。
+ __解析度：__ 查看測試執行的位 `test.log` 置 `/build/test-results/test-worker/test.log`。 在此檔案中找到與失敗測試案例對應的部分，並查看錯誤。

   ![疑難排解——未產生轉譯](./assets/test/troubleshooting__no-rendition-generated.png)

### 測試會產生錯誤的轉譯

測試案例無法產生錯誤的轉譯。

+ __錯誤：__ 失敗：轉譯&#39;rendition.xxx&#39;不如預期。
+ __原因：__ 工作者輸出與測試案例中提供的格 `rendition.<extension>` 式副本不同的格式副本。
   + 如果在測 `rendition.<extension>` 試案例中，未以與本機產生的轉譯完全相同的方式建立預期的檔案，則測試可能會失敗，因為位可能會有一些差異。 如果測試案例中的預期轉譯是從「開發工具」儲存（即在Adobe I/O Runtime中產生），位元在技術上可能會有所不同，導致測試失敗，即使從人類角度看，預期和實際的轉譯檔案完全相同。
+ __解析度：__ 導覽至測試以檢視轉譯輸出，並 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`將它與測試案例中預期的轉譯檔案比較。
