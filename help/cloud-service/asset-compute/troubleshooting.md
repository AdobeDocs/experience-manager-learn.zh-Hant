---
title: 排除AEM AssetsAsset compute擴展性故障
description: 以下是開發和部署AEM Assets的自定義Asset compute工作程式時可能遇到的常見問題和錯誤以及解決方案的索引。
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# 排除Asset compute擴展性故障

以下是開發和部署AEM Assets的自定義Asset compute工作程式時可能遇到的常見問題和錯誤以及解決方案的索引。

## 開發{#develop}

### 返回部分繪製/損壞的格式副本{#rendition-returned-partially-drawn-or-corrupt}

+ __錯誤__:格式副本呈現不完全（當影像）或已損壞且無法開啟。

   ![返回部分繪製的格式副本](./assets/troubleshooting/develop__await.png)

+ __原因__:工人的 `renditionCallback` 函式在格式副本完全寫入之前退出 `rendition.path`。
+ __解決__:查看自定義工作程式碼並確保使用 `await`。

## 開發工具{#development-tool}

### asset compute項目中缺少Console.json檔案{#missing-console-json}

+ __錯誤：__ 錯誤：驗證時缺少所需檔案(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)，非同步setupAssetCompute(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ 的 `console.json` 檔案在Asset compute項目的根中缺失
+ __解決方案：__ 下載新 `console.json` 形成您的Adobe I/O項目
   1. 在console.adobe.io中，開啟Adobe I/O項目，該Asset compute項目配置為使用
   1. 點擊 __下載__ 按鈕
   1. 使用檔案名將下載的檔案保存到Asset compute項目的根目錄 `console.json`

### manifest.yml中的YAML縮進不正確{#incorrect-yaml-indentation}

+ __錯誤：__ YAMLException:第X行第Y列處映射項的縮排錯誤：(通過標準從 `aio app run` 命令)
+ __原因：__ Yaml檔案是白色間隔的，可能是縮排不正確。
+ __解決方案：__ 查看 `manifest.yml` 並確保所有縮進正確。

### memorySize限制設定得過低{#memorysize-limit-is-set-too-low}

+ __錯誤：__  本地開發伺服器OpenWhiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400（錯誤請求） — > &quot;請求內容格式不正確：requirement失敗：記憶體低於允許的閾值64 MB 134217728 B&quot;
+ __原因：__ A `memorySize` 對於 `manifest.yml` 設定為低於錯誤消息報告的允許的最小閾值（以位元組為單位）。
+ __解決方案：__  查看 `memorySize` 限制 `manifest.yml` 確保它們都大於允許的最小閾值。

### 由於缺少private.key，開發工具無法啟動{#missing-private-key}

+ __錯誤：__ 本地開發伺服器錯誤：validatePrivateKeyFile中缺少所需檔案……。 (通過標準從 `aio app run` 命令)
+ __原因：__ 的 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 值 `.env` 檔案，不指向 `private.key` 或 `private.key` 當前用戶不可讀。
+ __解決方案：__ 查看 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 值 `.env` 檔案，並確保它包含到 `private.key` 檔案系統上。

### 源檔案下拉清單不正確{#source-files-dropdown-incorrect}

asset compute開發工具可能進入它提取陳舊資料的狀態，在 __源檔案__ 下拉清單顯示不正確的項目。

+ __錯誤：__ 源檔案下拉清單顯示的項目不正確。
+ __原因：__ 過時的快取瀏覽器狀態導致
+ __解決方案：__ 在瀏覽器中，完全清除瀏覽器頁籤的「應用程式狀態」、瀏覽器快取、本地儲存和服務工作程式。

### 缺少或無效的devToolToken查詢參數{#missing-or-invalid-devtooltoken-query-parameter}

+ __錯誤：__ asset compute開發工具中的「未授權」通知
+ __原因：__ `devToolToken` 缺少或無效
+ __解決方案：__ 關閉Asset compute開發工具瀏覽器窗口，終止通過 `aio app run` 命令，並重新啟動開發工具(使用 `aio app run`)。

### 無法刪除源檔案{#unable-to-remove-source-files}

+ __錯誤：__ 無法從開發工具UI中刪除添加的源檔案
+ __原因：__ 此功能尚未實現
+ __解決方案：__ 使用中定義的憑據登錄到雲儲存提供程式 `.env`。 找到開發工具使用的容器(也在 `.env`)，導航到 __源__ 資料夾，並刪除任何源映像。 YOu可能需要執行中概述的步驟 [源檔案下拉清單不正確](#source-files-dropdown-incorrect) 如果刪除的源檔案繼續顯示在下拉清單中，因為它們可能會在開發工具「應用程式狀態」中本地快取。

   ![MicrosoftAzure Blob儲存](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 測試{#test}

### 執行test期間未生成格式副本{#test-no-rendition-generated}

+ __錯誤：__ 失敗：未生成任何格式副本。
+ __原因：__ 由於意外錯誤（如JavaScript語法錯誤），工作程式無法生成格式副本。
+ __解決方案：__ 查看test執行 `test.log` 在 `/build/test-results/test-worker/test.log`。 找到此檔案中與失敗test案例對應的部分，並查看錯誤。

   ![疑難解答 — 未生成任何格式副本](./assets/troubleshooting/test__no-rendition-generated.png)

### Test生成不正確的格式副本，導致test失敗{#tests-generates-incorrect-rendition}

+ __錯誤：__ 失敗：格式副本「rendition.xxx」不如預期。
+ __原因：__ 工作人員輸出與 `rendition.<extension>` 在test案中。
   + 如果 `rendition.<extension>` 檔案的建立方式與本地生成的格式副本在test情況下的建立方式不完全相同，test可能會失敗，因為位中可能存在一些差異。 例如，如果Asset compute工作人員使用API更改對比度，並且通過調整Adobe Photoshop CC的對比度來建立預期結果，則檔案可能顯示相同，但位中的細微變化可能不同。
+ __解決方案：__ 通過導航至，查看test的格式副本輸出 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，並將其與test案例中的預期格式副本檔案進行比較。 要建立確切的預期資產，請執行以下操作之一：
   + 使用「開發工具」生成格式副本，驗證其正確性，並將其用作預期格式副本檔案
   + 或者，驗證test生成的檔案 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，驗證其正確性，並將其用作預期的格式副本檔案

## 偵錯

### 調試器未附加{#debugger-does-not-attach}

+ __錯誤__:處理啟動時出錯：錯誤：無法連接到調試目標……
+ __原因__:Docker Desktop未在本地系統上運行。 通過查看VS代碼調試控制台（「查看」>「調試控制台」）來驗證這一點，確認報告了此錯誤。
+ __解決__:開始 [Docker Desktop並確認安裝了必需的Docker映像](./set-up/development-environment.md#docker)。

### 斷點未暫停{#breakpoints-no-pausing}

+ __錯誤__:從可調試開發工具運行Asset compute工作程式時，VS代碼不會在斷點處暫停。

#### 未附加VS代碼調試器{#vs-code-debugger-not-attached}

+ __原因：__ VS代碼調試器已停止/斷開連接。
+ __解決方案：__ 重新啟動VS代碼調試器，並通過查看VS代碼調試輸出控制台（「查看」>「調試控制台」）來驗證它是否連接

#### 工作程式執行開始後附加的VS代碼調試程式{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ 輕擊前未附加VS代碼調試器 __運行__ 的子菜單。
+ __解決方案：__ 通過查看VS代碼的調試控制台（「視圖」>「調試控制台」），然後從開發工具重新運行Asset compute工作程式，確保已附加調試程式。

### 調試時工作進程超時{#worker-times-out-while-debugging}

+ __錯誤__:調試控制台報告「操作將在 — XXX毫秒內超時」或 [asset compute開發工具](./develop/development-tool.md) 格式副本預覽無限期地旋轉
+ __原因__:工作進程超時(如 [manifest.yml](./develop/manifest.md) 在調試期間超出。
+ __解決__:臨時增加工人的超時 [manifest.yml](./develop/manifest.md) 或加速調試活動。

### 無法終止調試程式進程{#cannot-terminate-debugger-process}

+ __錯誤__: `Ctrl-C` 命令行上不終止調試器進程(`npx adobe-asset-compute devtool`)。
+ __原因__:錯誤 `@adobe/aio-cli-plugin-asset-compute` 1.3.x，結果 `Ctrl-C` 無法識別為終止命令。
+ __解決__:更新 `@adobe/aio-cli-plugin-asset-compute` 到1.4.1+版

   ```
   $ aio update
   ```

   ![疑難解答 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### 中的資產中缺少自定義格式副AEM本{#custom-rendition-missing-from-asset}

+ __錯誤：__ 已成功處理新資產和重新處理的資產，但缺少自定義格式副本

#### 處理配置檔案未應用於上級資料夾

+ __原因：__ 該資產在具有使用自定義工作人員的處理配置檔案的資料夾下不存在
+ __解決方案：__ 將處理配置檔案應用於資產的祖先資料夾

#### 處理配置檔案被較低處理配置檔案取代

+ __原因：__ 該資產存在於應用了自定義工作人員處理配置檔案的資料夾下，但在該資料夾和資產之間應用了不使用客戶工作人員的不同處理配置檔案。
+ __解決方案：__ 合併或協調兩個處理配置檔案並刪除中間處理配置檔案

### 資產處理失敗AEM{#asset-processing-fails}

+ __錯誤：__ 資產處理失敗標籤顯示在資產上
+ __原因：__ 執行自定義工作程式時出錯
+ __解決方案：__ 按照 [調試Adobe I/O Runtime激活](./test-debug/debug.md#aio-app-logs) 使用 `aio app logs`。
