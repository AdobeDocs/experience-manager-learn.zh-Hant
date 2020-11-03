---
title: 對資產計算工作器進行調試
description: 資產計算工作者可透過數種方式進行除錯，從簡單的除錯記錄陳述式到附加的VS程式碼（做為遠端除錯程式），再到提取從AEM以雲端服務方式啟動的Adobe I/O Runtime中啟動的記錄檔。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# 對資產計算工作器進行調試

資產計算工作者可透過數種方式進行除錯，從簡單的除錯記錄陳述式到附加的VS程式碼（做為遠端除錯程式），再到提取從AEM以雲端服務方式啟動的Adobe I/O Runtime中啟動的記錄檔。

## 記錄

除錯「資產計算」工作程式的最基本形式是使用工 `console.log(..)` 作程式碼中的傳統語句。 JavaScript `console` 物件是隱式的全域物件，因此不需要匯入或要求它，因為它永遠存在於所有的上下文中。

這些日誌語句根據Asset Compute工作器的執行方式進行不同的審閱：

+ 從 `aio app run`列印記錄到標準輸出， [以及開發工具的啟動記錄](../develop/development-tool.md)
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 從 `aio app test`，將打印記錄到 `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用 `wskdebug`時，記錄陳述式會列印至VS程式碼除錯主控台（檢視>除錯主控台），標準輸出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用 `aio app logs`，日誌語句打印到激活日誌輸出

## 透過附加除錯程式進行遠端除錯

>[!WARNING]
>
>使用Microsoft Visual Studio代碼1.48.0或更新版本，以與wskdebug相容

wskdebug [](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組支援將調試器附加到資產計算工作，包括能夠在VS代碼中設定斷點並逐步執行代碼。

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_使用wskdebug調試資產計算工作器的點進（無音頻）_

1. 確 [保已安](../set-up/development-environment.md#wskdebug) 裝wskdebug [和](../set-up/development-environment.md#ngork) ngrok npm模組
1. 確保 [Docker Desktop和支援的Docker映像](../set-up/development-environment.md#docker) 已安裝並運行
1. 關閉「開發工具」的任何作用中執行例項。
1. 使用部署最新代碼 `aio app deploy` 並記錄已部署的操作名稱(在之間的名 `[...]`稱)。 這將用於更新步驟8 `launch.json` 中的。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. 使用命令啟動資產計算開發工具的新實例 `npx adobe-asset-compute devtool`
1. 在「VS程式碼」中，點選左側導覽中的「除錯」圖示
   + 如果出現提示， __請點選「建立launch.json檔案> Node.js__ 」以建立新 `launch.json` 檔案。
   + 否則，點選 __Launch Program（啟動程式）__ 右側的Gear __（齒輪）圖示，以開啟編輯器__`launch.json` 中現有的。
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

1. 從下拉式清 __單中選取__ 「新wskdebug」
1. 點選wskdebug下拉 __式清單左側的綠色__ 執行按鈕 ____
1. 開 `/actions/worker/index.js` 啟並點選行號左側以新增中斷點1。 定位至步驟6中開啟的「資產計算開發工具Web瀏覽器」窗口
1. 點選「 __Run__ （運行）」按鈕以執行工作器
1. 導覽回VS程式碼，並 `/actions/worker/index.js` 逐步執行程式碼
1. 若要退出可除錯的開發工具，請在執行 `Ctrl-C` 步驟6中命令的 `npx adobe-asset-compute devtool` 終端中點選

## 從Adobe I/O Runtime存取記錄檔{#aio-app-logs}

[AEM是雲端服務，透過「處理設定檔」](../deploy/processing-profiles.md) ，直接在Adobe I/O Runtime中叫用資產計算工作者。 由於這些調用不涉及本地開發，因此不能使用本地工具（如「資產計算開發工具」或wskdebug）來調試其執行。 Adobe I/O CLI可用來從Adobe I/O Runtime中特定工作區中執行的工作者擷取記錄檔。

1. 確保根據 [需要除錯的工作區](../deploy/runtime.md) ，透過 `AIO_runtime_namespace` 和來設定特定於工作區的環 `AIO_runtime_auth`境變數。
1. 從命令行執行 `aio app logs`
   + 如果工作區產生大量流量，請透過旗標擴充啟動記錄 `--limit` 檔數：
      `$ aio app logs --limit=25`
1. 最近(最多提供的 `--limit`)激活日誌將作為命令的輸出返回以供查看。

   ![aio app logs](./assets/debug/aio-app-logs.png)

## 疑難排解

+ [除錯程式未附加](../troubleshooting.md#debugger-does-not-attach)
+ [中斷點不會暫停](../troubleshooting.md#breakpoints-no-pausing)
+ [VS程式碼除錯程式未連接](../troubleshooting.md#vs-code-debugger-not-attached)
+ [VS代碼調試器在工作器執行開始後附加](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [在調試時工作器超時](../troubleshooting.md#worker-times-out-while-debugging)
+ [無法終止調試器進程](../troubleshooting.md#cannot-terminate-debugger-process)
