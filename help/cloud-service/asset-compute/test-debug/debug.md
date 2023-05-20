---
title: 調試Asset compute工作進程
description: asset compute工作程式可通過多種方式進行調試，從簡單的調試日誌語句，到作為遠程調試器連接的VS代碼，再到從as a Cloud Service啟動的Adobe I/O Runtime激活的提取日AEM志。
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

# 調試Asset compute工作進程

asset compute工作程式可通過多種方式進行調試，從簡單的調試日誌語句，到作為遠程調試器連接的VS代碼，再到從as a Cloud Service啟動的Adobe I/O Runtime激活的提取日AEM志。

## 記錄

調試Asset compute工人的最基本形式使用傳統 `console.log(..)` 工作代碼中的語句。 的 `console` JavaScript對象是隱式全局對象，因此無需導入或要求它，因為它始終存在於所有上下文中。

這些日誌語句可根據Asset compute工作人員的執行方式進行不同的審閱：

+ 從 `aio app run`，將打印記錄到標準輸出， [開發工具](../develop/development-tool.md) 激活日誌
   ![aio應用運行console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 從 `aio app test`，日誌打印到 `/build/test-results/test-worker/test.log`
   ![aio應用testconsole.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用 `wskdebug`，日誌語句打印到VS代碼調試控制台（查看>調試控制台），標準輸出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用 `aio app logs`，日誌語句打印到激活日誌輸出

## 通過連接的調試器進行遠程調試

>[!WARNING]
>
>使用MicrosoftVisual Studio代碼1.48.0或更高版本與wskdebug相容

的 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，支援將調試器附加到Asset compute工作程式，包括在VS代碼中設定斷點並逐步執行代碼。

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_使用wskdebug調試Asset compute工作程式的點擊（無音頻）_

1. 確保 [wskdebug](../set-up/development-environment.md#wskdebug) 和 [恩格羅](../set-up/development-environment.md#ngork) npm模組已安裝
1. 確保 [Docker案頭和支援Docker映像](../set-up/development-environment.md#docker) 已安裝並正在運行
1. 關閉「開發工具」的任何活動運行實例。
1. 使用 `aio app deploy`  並記錄已部署的操作名稱( `[...]`)。 這用於更新 `launch.json` 的子例行程式。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令啟動Asset compute開發工具的新實例 `npx adobe-asset-compute devtool`
1. 在VS代碼中，按一下左側導航中的「調試」表徵圖
   + 如果出現提示，請點擊 __建立launch.json檔案> Node.js__ 新建 `launch.json` 的子菜單。
   + 否則，點擊 __齒輪__ 表徵圖 __啟動計畫__ 下拉菜單以開啟現有 `launch.json` 的雙曲餘切值。
1. 將以下JSON對象配置添加到 `configurations` 陣列：

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

1. 選擇新建 __wskdebug__ 從下拉清單
1. 點擊綠色 __運行__ 按鈕 __wskdebug__ 下拉清單
1. 開啟 `/actions/worker/index.js` 並點擊行號左側以添加斷點1。 導航到步驟6中開啟的Asset compute開發工具Web瀏覽器窗口
1. 點擊 __運行__ 按鈕以執行工作人員
1. 導航到VS代碼， `/actions/worker/index.js` 並逐步完成代碼
1. 要退出可調試的開發工具，請點擊 `Ctrl-C` 在航站樓里 `npx adobe-asset-compute devtool` 步驟6中的命令

## 訪問來自Adobe I/O Runtime的日誌{#aio-app-logs}

[AEMas a Cloud Service通過處理配置檔案利用Asset compute工作人員](../deploy/processing-profiles.md) 直接在Adobe I/O Runtime找他們。 由於這些調用不涉及本地開發，因此無法使用本地工具(如Asset compute開發工具或wskdebug)來調試它們的執行。 相反，Adobe I/OCLI可用於從在Adobe I/O Runtime特定工作區中執行的工作人員中提取日誌。

1. 確保 [特定於工作區的環境變數](../deploy/runtime.md) 設定為 `AIO_runtime_namespace` 和 `AIO_runtime_auth`，基於需要調試的工作區。
1. 從命令行執行 `aio app logs`
   + 如果工作區產生大量通信，請通過 `--limit` 標誌：
      `$ aio app logs --limit=25`
1. 最近(到提供的 `--limit`)激活日誌作為命令的輸出返回以供審閱。

   ![aio應用程式日誌](./assets/debug/aio-app-logs.png)

## 疑難排解

+ [調試器未附加](../troubleshooting.md#debugger-does-not-attach)
+ [斷點未暫停](../troubleshooting.md#breakpoints-no-pausing)
+ [未附加VS代碼調試器](../troubleshooting.md#vs-code-debugger-not-attached)
+ [工作程式執行開始後附加的VS代碼調試程式](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [調試時工作進程超時](../troubleshooting.md#worker-times-out-while-debugging)
+ [無法終止調試程式進程](../troubleshooting.md#cannot-terminate-debugger-process)
