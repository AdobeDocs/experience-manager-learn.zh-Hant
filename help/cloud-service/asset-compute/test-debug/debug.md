---
title: 調試Asset compute工作
description: asset compute背景工作可透過數種方式進行除錯，從簡單的除錯記錄陳述式，到附加的VS Code（作為遠端除錯工具），再到從起始自AEM as a Cloud Service的Adobe I/O Runtime中提取啟用記錄。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# 調試Asset compute工作

asset compute背景工作可透過數種方式進行除錯，從簡單的除錯記錄陳述式，到附加的VS Code（作為遠端除錯工具），再到從起始自AEM as a Cloud Service的Adobe I/O Runtime中提取啟用記錄。

## 記錄

調試Asset compute工作的最基本形式使用傳統 `console.log(..)` 語句。 此 `console` JavaScript物件是隱式的全域物件，因此不需要匯入或要求它，因為它一律存在於所有內容中。

根據Asset compute工作程式的執行方式，這些日誌陳述式可用於查看的方式不同：

+ 從 `aio app run`，記錄打印到標準輸出， [開發工具](../develop/development-tool.md) 啟動記錄
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 從 `aio app test`，將打印到 `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用 `wskdebug`，記錄陳述式會列印至VS程式碼除錯主控台（檢視>除錯主控台），標準輸出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用 `aio app logs`，日誌語句打印到激活日誌輸出

## 透過附加偵錯工具進行遠端除錯

>[!WARNING]
>
>使用Microsoft Visual Studio Code 1.48.0或更新版本，以與wskdebug相容

此 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組支援將除錯程式附加至Asset compute背景工作，包括在VS程式碼中設定中斷點，以及逐步執行程式碼的功能。

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_使用wskdebug對Asset compute工作程式進行偵錯的點進（無音訊）_

1. 確保 [wskdebug](../set-up/development-environment.md#wskdebug) 和 [恩格羅克](../set-up/development-environment.md#ngork) 已安裝npm模組
1. 確保 [Docker Desktop和支援的Docker映像](../set-up/development-environment.md#docker) 已安裝且正在執行
1. 關閉「開發工具」的任何作用中執行個體。
1. 使用部署最新程式碼 `aio app deploy`  並記錄已部署的動作名稱(名稱介於 `[...]`)。 這可用來更新 `launch.json` 中。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令啟動Asset compute開發工具的新實例 `npx adobe-asset-compute devtool`
1. 在VS程式碼中，點選左側導覽中的「除錯」圖示
   + 如果出現提示，請點選 __建立launch.json檔案> Node.js__ 建立新 `launch.json` 檔案。
   + 否則，點選 __齒輪__ 表徵圖 __啟動計畫__ 下拉式清單以開啟現有 `launch.json` 編輯里。
1. 將下列JSON物件設定新增至 `configurations` 陣列：

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. 選取新 __wskdebug__ 從下拉式清單
1. 點選綠色 __執行__ 按鈕 __wskdebug__ 下拉式清單
1. 開啟 `/actions/worker/index.js` 並點選行號左側以添加斷點1。 導覽至在步驟6中開啟的Asset compute開發工具Web瀏覽器視窗
1. 點選 __執行__ 執行工作人員的按鈕
1. 導覽回「VS程式碼」，然後導覽至 `/actions/worker/index.js` 並逐步執行程式碼
1. 若要退出「可除錯的開發工具」，請點選 `Ctrl-C` 在航站樓里 `npx adobe-asset-compute devtool` 步驟6中的命令

## 從Adobe I/O Runtime存取記錄{#aio-app-logs}

[AEMas a Cloud Service透過處理設定檔運用Asset compute背景工作](../deploy/processing-profiles.md) 直接在Adobe I/O Runtime叫用。 由於這些調用不涉及本地開發，因此無法使用本地工具(如Asset compute開發工具或wskdebug)調試它們的執行。 相反地，Adobe I/OCLI可用來從Adobe I/O Runtime中特定工作區中執行的工作器擷取記錄。

1. 確保 [工作區專用環境變數](../deploy/runtime.md) 透過 `AIO_runtime_namespace` 和 `AIO_runtime_auth`，根據需要偵錯的工作區。
1. 從命令行執行 `aio app logs`
   + 如果工作區發生大量流量，請透過 `--limit` 標幟：
      `$ aio app logs --limit=25`
1. 最近(截至提供的 `--limit`)激活日誌將作為命令的輸出返回以供查看。

   ![aio應用程式記錄](./assets/debug/aio-app-logs.png)

## 疑難排解

+ [除錯程式未附加](../troubleshooting.md#debugger-does-not-attach)
+ [斷點未暫停](../troubleshooting.md#breakpoints-no-pausing)
+ [VS程式碼除錯程式未附加](../troubleshooting.md#vs-code-debugger-not-attached)
+ [工作程式開始執行後附加的VS程式碼偵錯器](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [調試時工作器超時](../troubleshooting.md#worker-times-out-while-debugging)
+ [無法終止調試程式進程](../troubleshooting.md#cannot-terminate-debugger-process)
