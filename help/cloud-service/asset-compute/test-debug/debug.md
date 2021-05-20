---
title: 調試Asset compute工作
description: asset compute背景工作可透過數種方式進行除錯，從簡單的除錯記錄陳述式，到附加的VS Code（作為遠端除錯工具），再到從AEM作為Cloud Service啟動的Adobe I/O Runtime中提取啟動記錄。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 0%

---


# 調試Asset compute工作

asset compute背景工作可透過數種方式進行除錯，從簡單的除錯記錄陳述式，到附加的VS Code（作為遠端除錯工具），再到從AEM作為Cloud Service啟動的Adobe I/O Runtime中提取啟動記錄。

## 記錄

調試Asset compute工作的最基本形式在工作程式代碼中使用傳統的`console.log(..)`語句。 `console` JavaScript物件是隱式的全域物件，因此不需要匯入或要求它，因為它一律存在於所有內容中。

根據Asset compute工作程式的執行方式，這些日誌陳述式可用於查看的方式不同：

+ 從`aio app run`，記錄打印到標準輸出，以及[開發工具的](../develop/development-tool.md)激活日誌
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 從`aio app test`，將日誌打印到`/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用`wskdebug`，記錄陳述式會列印至VS程式碼除錯主控台（檢視>除錯主控台），標準輸出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用`aio app logs`，日誌語句打印到激活日誌輸出

## 透過附加偵錯工具進行遠端除錯

>[!WARNING]
>
>使用Microsoft Visual Studio Code 1.48.0或更高版本，以與wskdebug相容

[wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組支援將調試器附加到Asset compute工作，包括在VS代碼中設定斷點並逐步執行代碼的功能。

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_使用wskdebug對Asset compute工作程式進行偵錯的點進（無音訊）_

1. 確保已安裝[wskdebug](../set-up/development-environment.md#wskdebug)和[ngrok](../set-up/development-environment.md#ngork) npm模組
1. 確保[Docker Desktop和支援的Docker映像](../set-up/development-environment.md#docker)已安裝並正在運行
1. 關閉「開發工具」的任何作用中執行個體。
1. 使用`aio app deploy`部署最新代碼，並記錄已部署的操作名稱（`[...]`之間的名稱）。 這將用於更新步驟8中的`launch.json`。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令`npx adobe-asset-compute devtool`啟動Asset compute開發工具的新實例
1. 在VS程式碼中，點選左側導覽中的「除錯」圖示
   + 如果出現提示，請點選&#x200B;__建立launch.json檔案> Node.js__&#x200B;以建立新的`launch.json`檔案。
   + 否則，點選&#x200B;__Launch Program__&#x200B;下拉式清單右側的&#x200B;__Gear__&#x200B;圖示，以開啟編輯器中現有的`launch.json`。
1. 將下列JSON物件設定新增至`configurations`陣列：

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

1. 從下拉式清單中選取新的&#x200B;__wskdebug__
1. 點選&#x200B;__wskdebug__&#x200B;左側的綠色&#x200B;__執行__&#x200B;按鈕
1. 開啟`/actions/worker/index.js` ，點選行號左側以新增中斷點1。 導覽至在步驟6中開啟的Asset compute開發工具Web瀏覽器視窗
1. 點選&#x200B;__Run__&#x200B;按鈕以執行工作器
1. 導覽回「VS程式碼」、`/actions/worker/index.js`，然後逐步執行程式碼
1. 若要退出可除錯的開發工具，請在步驟6中執行`npx adobe-asset-compute devtool`命令的終端中點選`Ctrl-C`

## 從Adobe I/O Runtime{#aio-app-logs}存取記錄

[AEM as aCloud Service會直接在Adobe I/O Runtime中叫用Asset compute背景工作，](../deploy/processing-profiles.md) 透過處理設定檔來運用背景工作。由於這些調用不涉及本地開發，因此無法使用本地工具(如Asset compute開發工具或wskdebug)調試它們的執行。 相反地，Adobe I/OCLI可用來從Adobe I/O Runtime中特定工作區中執行的工作器擷取記錄。

1. 根據需要偵錯的工作區，確認[工作區特定環境變數](../deploy/runtime.md)是透過`AIO_runtime_namespace`和`AIO_runtime_auth`設定。
1. 從命令行執行`aio app logs`
   + 如果工作區發生大量流量，請透過`--limit`標幟展開啟用記錄的數量：
      `$ aio app logs --limit=25`
1. 將返回最近的（直到提供的`--limit`）激活日誌作為要查看的命令的輸出。

   ![aio應用程式記錄](./assets/debug/aio-app-logs.png)

## 疑難排解

+ [除錯程式未附加](../troubleshooting.md#debugger-does-not-attach)
+ [斷點未暫停](../troubleshooting.md#breakpoints-no-pausing)
+ [VS程式碼除錯程式未附加](../troubleshooting.md#vs-code-debugger-not-attached)
+ [工作程式開始執行後附加的VS程式碼偵錯器](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [調試時工作器超時](../troubleshooting.md#worker-times-out-while-debugging)
+ [無法終止調試程式進程](../troubleshooting.md#cannot-terminate-debugger-process)
