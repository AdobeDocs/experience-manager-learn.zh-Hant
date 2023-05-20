---
title: TestAsset compute工作人員
description: asset compute項目定義了一種模式，用於輕鬆建立和執行Asset compute工作程式的test。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 0%

---

# TestAsset compute工作人員

asset compute項目定義了用於容易建立和執行的模式 [testAsset compute工人](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html)。

## 工人test解剖

asset compute工人的test被分成test套間，在每個test套間內，一個或多個test案件對test提出條件。

asset compute項目test結構如下：

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

每個test轉換都可以具有以下檔案：

+ `file.<extension>`
   + 源檔案到test(副檔名可以是除 `.link`)
   + 必要
+ `rendition.<extension>`
   + 預期格式副本
   + 必需，但錯誤測試除外
+ `params.json`
   + 單個格式副本JSON說明
   + 選用
+ `validate`
   + 將預期和實際格式副本檔案路徑作為參數的指令碼，如果結果正常，則必須返回退出代碼0；如果驗證或比較失敗，則返回非零退出代碼。
   + 可選，預設為 `diff` 命令
   + 使用包含docker run命令的shell指令碼來使用不同的驗證工具
+ `mock-<host-name>.json`
   + JSON格式的HTTP響應 [嘲弄外部服務呼叫](https://www.mock-server.com/mock_server/creating_expectations.html)。
   + 可選，僅在工作代碼發出其自己的HTTP請求時使用

## 編寫test案

此test案例斷言參數化輸入(`params.json`)，用於輸入檔案(`file.jpg`)生成預期的PNG格式副本(`rendition.png`)。

1. 首先刪除自動生成的 `simple-worker` test案例 `/test/asset-compute/simple-worker` 因為這是無效的，因為我們的工作人員不再只是將源複製到格式副本。
1. 在下面建立新test案例資料夾 `/test/asset-compute/worker/success-parameterized` test生成PNG格式副本的工作程式的成功執行。
1. 在 `success-parameterized` 資料夾，添加test [輸入檔案](./assets/test/success-parameterized/file.jpg) 為這個test案例命名 `file.jpg`。
1. 在 `success-parameterized` 資料夾，添加名為 `params.json` 定義工作人員的輸入參數：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   這些是傳入到 [開發工具的Asset compute配置檔案定義](../develop/development-tool.md)，減 `worker` 按鈕

1. 添加預期 [格式檔案](./assets/test/success-parameterized/rendition.png) 給這個test案起名 `rendition.png`。 此檔案表示給定輸入的工作程式的預期輸出 `file.jpg`。
1. 從命令行中，通過執行項目根目錄的test `aio app test`
   + 確保 [Docker案頭](../set-up/development-environment.md#docker) 支援Docker映像的安裝和啟動
   + 終止任何正在運行的開發工具實例

![Test — 成功 ](./assets/test/success-parameterized/result.png)

## 寫入錯誤檢查test案例

此test案例test，以確保工作人員在 `contrast` 參數設定為無效值。

1. 在下面建立新test案例資料夾 `/test/asset-compute/worker/error-contrast` test因無效而重新執行工作程式 `contrast` 參數值。
1. 在 `error-contrast` 資料夾，添加test [輸入檔案](./assets/test/error-contrast/file.jpg) 為這個test案例命名 `file.jpg`。 此檔案的內容對此test不重要，它只需存在即可通過「損壞的源」檢查，以便到達 `rendition.instructions` 有效性檢查，此test案例驗證。
1. 在 `error-contrast` 資料夾，添加名為 `params.json` 定義具有以下內容的工作人員的輸入參數：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 設定 `contrast` 參數 `10`，無效值，因為對比度必須介於–1和1之間，才能拋出 `RenditionInstructionsError`。
   + 通過設定test `errorReason` 與預期錯誤關聯的「reason」的鍵。 此無效的對比度參數將引發 [自定義錯誤](../develop/worker.md#errors)。 `RenditionInstructionsError`，因此設定 `errorReason` 錯誤的原因，或`rendition_instructions_error` 來斷言它被拋出。

1. 由於執行錯誤時不應生成任何格式副本，因此不 `rendition.<extension>` 檔案。
1. 通過執行命令從項目的根目錄運行test套件 `aio app test`
   + 確保 [Docker案頭](../set-up/development-environment.md#docker) 支援Docker映像的安裝和啟動
   + 終止任何正在運行的開發工具實例

![Test — 錯誤對比度](./assets/test/error-contrast/result.png)

## TestGithub案

Github的最終test案可查閱：

+ [aem指南 — wknd資產計算/test/資產計算/工作人員](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑難排解

+ [執行test期間未生成格式副本](../troubleshooting.md#test-no-rendition-generated)
+ [Test生成不正確的格式副本](../troubleshooting.md#tests-generates-incorrect-rendition)
