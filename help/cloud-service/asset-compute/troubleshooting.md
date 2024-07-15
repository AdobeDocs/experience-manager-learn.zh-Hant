---
title: 疑難排解AEM Assets的Asset compute擴充性
description: 以下是為AEM Assets開發和部署自訂Asset compute背景工作時可能遇到的常見問題和錯誤的索引，以及解決方法。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# 疑難排解Asset compute擴充性

以下是為AEM Assets開發和部署自訂Asset compute背景工作時可能遇到的常見問題和錯誤的索引，以及解決方法。

## 開發{#develop}

### 轉譯已傳回部分繪製/損毀{#rendition-returned-partially-drawn-or-corrupt}

+ __錯誤__：轉譯未完全轉譯（影像時）或已損毀，無法開啟。

  ![傳回部分繪製的轉譯](./assets/troubleshooting/develop__await.png)

+ __原因__：背景工作程式的`renditionCallback`函式正在結束，才能將轉譯完全寫入`rendition.path`。
+ __解決方法__：檢閱自訂背景工作程式碼，並確定所有非同步呼叫都使用`await`進行同步。

## 開發工具{#development-tool}

### asset compute專案中缺少Console.json檔案{#missing-console-json}

+ __錯誤：__&#x200B;錯誤：在async setupAssetCompute (`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)驗證(`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`)時遺漏必要的檔案
+ __原因：__ Asset compute專案的根目錄中缺少`console.json`檔案
+ __解析度：__&#x200B;從您的Adobe I/O專案下載新的`console.json`
   1. 在console.adobe.io中，開啟Asset compute專案設定要使用的Adobe I/O專案
   1. 點選右上方的&#x200B;__下載__&#x200B;按鈕
   1. 使用檔案名稱`console.json`將下載的檔案儲存至Asset compute專案的根目錄

### manifest.yml中的YAML縮排不正確{#incorrect-yaml-indentation}

+ __錯誤：__ YAMLException：第X行、第Y欄的對應專案縮排錯誤：（透過`aio app run`命令的標準輸出）
+ __原因：__ Yaml檔案是區分白色的空間，您的縮排可能不正確。
+ __解析度：__&#x200B;請檢閱您的`manifest.yml`並確保所有縮排都正確。

### memorySize限制設定得太低{#memorysize-limit-is-set-too-low}

+ __錯誤：__&#x200B;本機Dev Server OpenWhiskError：PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true傳回HTTP 400 （錯誤請求） —>「請求內容格式錯誤：需求失敗：記憶體比允許閾值134217728 B低64 MB」
+ __原因：__ `manifest.yml`中背景工作程式的`memorySize`限制設定在錯誤訊息（位元組）所報告的最低允許臨界值以下。
+ __解析度：__&#x200B;檢閱`manifest.yml`中的`memorySize`限制，並確定這些限制都大於允許的最小臨界值。

### 開發工具無法啟動，因為遺失private.key{#missing-private-key}

+ __錯誤：__&#x200B;本機Dev ServerError：在validatePrivateKeyFile遺漏必要的檔案.... （透過`aio app run`命令中的標準輸出）
+ __原因：__ `.env`檔案中的`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`值未指向`private.key`或`private.key`不是目前使用者可讀取的。
+ __解析度：__&#x200B;檢閱`.env`檔案中的`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`值，並確定它包含檔案系統上`private.key`的完整絕對路徑。

### Source檔案下拉式清單不正確{#source-files-dropdown-incorrect}

asset compute開發工具可能會進入提取過時資料的狀態，在&#x200B;__Source檔案__&#x200B;顯示不正確專案的下拉式清單中最為明顯。

+ __錯誤：__ Source檔案下拉式清單顯示不正確的專案。
+ __原因：__&#x200B;過時快取的瀏覽器狀態導致
+ __解決方法：__&#x200B;在瀏覽器中，完全清除瀏覽器標籤的「應用程式狀態」、瀏覽器快取、本機儲存裝置和Service Worker。

### devToolToken查詢引數遺失或無效{#missing-or-invalid-devtooltoken-query-parameter}

+ __錯誤：__ Asset compute開發工具中的「未獲授權」通知
+ __原因：__ `devToolToken`遺失或無效
+ __解決方法：__&#x200B;關閉Asset compute開發工具瀏覽器視窗，終止任何透過`aio app run`命令起始的執行中開發工具處理序，然後重新啟動開發工具（使用`aio app run`）。

### 無法移除來源檔案{#unable-to-remove-source-files}

+ __錯誤：__&#x200B;無法從開發工具UI中移除新增的來源檔案
+ __原因：__&#x200B;此功能尚未實作
+ __解析度：__&#x200B;使用`.env`中定義的認證登入您的雲端儲存提供者。 找出開發工具使用的容器（也在`.env`中指定），導覽至&#x200B;__來源__&#x200B;資料夾，並刪除任何來源影像。 如果刪除的來源檔案繼續顯示在下拉式清單中，您可能需要執行[Source檔案下拉式清單中概述的步驟](#source-files-dropdown-incorrect)，因為這些檔案可能快取到開發工具的「應用程式狀態」中。

  ![Microsoft Azure Blob儲存體](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 測試{#test}

### 測試執行期間未產生轉譯{#test-no-rendition-generated}

+ __錯誤：__&#x200B;失敗：未產生轉譯。
+ __原因：__&#x200B;背景工作無法產生轉譯，因為發生非預期的錯誤，例如JavaScript語法錯誤。
+ __解決方法：__&#x200B;檢閱測試執行在`/build/test-results/test-worker/test.log`的`test.log`。 找到此檔案中對應失敗測試案例的區段，並檢閱錯誤。

  ![疑難排解 — 未產生轉譯](./assets/troubleshooting/test__no-rendition-generated.png)

### 測試產生錯誤的轉譯，導致測試失敗{#tests-generates-incorrect-rendition}

+ __錯誤：__&#x200B;失敗：轉譯&#39;rendition.xxx&#39;未如預期。
+ __原因：__&#x200B;背景工作輸出與測試案例中提供的`rendition.<extension>`不相同的轉譯。
   + 如果預期的`rendition.<extension>`檔案不是以與測試案例中本機產生之轉譯完全相同的方式建立的，則測試可能會失敗，因為位元之間可能有一些差異。 例如，如果Asset compute背景工作使用API變更對比，並且預期結果藉由調整Adobe Photoshop CC中的對比而建立，則檔案可能顯示相同，但位元中的細微變化可能不同。
+ __解析度：__&#x200B;瀏覽至`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，檢閱測試中的轉譯輸出，並將其與測試案例中預期的轉譯檔案進行比較。 若要建立完全相同的預期資產，您可以：
   + 使用開發工具來產生轉譯、驗證是否正確，並當做預期的轉譯檔案使用
   + 或者，在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`驗證測試產生的檔案，驗證它是否正確，並當做預期的轉譯檔案使用

## 偵錯

### 偵錯工具未附加{#debugger-does-not-attach}

+ __錯誤__：處理啟動時發生錯誤：錯誤：無法連線到偵錯目標……
+ __原因__： Docker Desktop未在本機系統上執行。 檢閱VS程式碼偵錯主控台（「檢視>偵錯主控台」）以確認此錯誤，加以確認。
+ __解析度__：啟動[Docker Desktop，並確認已安裝必要的Docker影像](./set-up/development-environment.md#docker)。

### 中斷點未暫停{#breakpoints-no-pausing}

+ __錯誤__：從可偵錯的開發工具執行Asset compute工作者時，VS Code不會在中斷點暫停。

#### 未附加VS程式碼偵錯工具{#vs-code-debugger-not-attached}

+ __原因：__ VS程式碼偵錯工具已停止/中斷連線。
+ __解析度：__&#x200B;重新啟動VS程式碼偵錯工具，並透過觀看VS程式碼偵錯輸出主控台（[檢視] > [偵錯主控台]）來驗證其是否附加

#### 背景工作執行開始後附加的VS程式碼偵錯工具{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ VS程式碼偵錯工具在點選[開發工具]中的&#x200B;__執行__&#x200B;之前並未附加。
+ __解決方法：__&#x200B;請檢閱VS程式碼的偵錯主控台（檢視>偵錯主控台），然後從開發工具重新執行Asset compute背景工作，以確定偵錯工具已附加。

### Worker在偵錯時逾時{#worker-times-out-while-debugging}

+ __錯誤__：偵錯主控台報告「動作將在 — XXX毫秒內逾時」或[Asset compute開發工具的](./develop/development-tool.md)轉譯預覽無限期旋轉，或
+ __原因__：偵錯期間超過[manifest.yml](./develop/manifest.md)中定義的背景工作逾時。
+ __解決方法__：在[manifest.yml](./develop/manifest.md)中暫時增加背景工作程式的逾時，或加速偵錯活動。

### 無法終止偵錯工具程式{#cannot-terminate-debugger-process}

+ __錯誤__：命令列上的`Ctrl-C`未終止偵錯工具處理序(`npx adobe-asset-compute devtool`)。
+ __原因__： `@adobe/aio-cli-plugin-asset-compute` 1.3.x發生錯誤，導致`Ctrl-C`無法辨識為終止命令。
+ __解析度__：將`@adobe/aio-cli-plugin-asset-compute`更新至1.4.1+版

  ```
  $ aio update
  ```

  ![疑難排解 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM資產中缺少自訂轉譯{#custom-rendition-missing-from-asset}

+ __錯誤：__&#x200B;新的和重新處理的資產處理成功，但缺少自訂轉譯

#### 處理設定檔未套用至上級資料夾

+ __原因：__&#x200B;資產不存在於具有使用自訂背景工作之處理設定檔的資料夾下
+ __解析度：__&#x200B;將處理設定檔套用至資產的上階資料夾

#### 由較低層處理設定檔取代的處理設定檔

+ __原因：__&#x200B;資產存在於已套用自訂背景工作處理設定檔的資料夾下方，但已在該資料夾與資產之間套用未使用客戶背景工作的不同處理設定檔。
+ __解析度：__&#x200B;合併或以其他方式調解兩個處理設定檔，並移除中繼處理設定檔

### AEM中的資產處理失敗{#asset-processing-fails}

+ __錯誤：__&#x200B;資產處理失敗徽章已顯示在資產上
+ __原因：__&#x200B;執行自訂背景工作時發生錯誤
+ __解析度：__&#x200B;請依照[使用`aio app logs`偵錯Adobe I/O Runtime啟用](./test-debug/debug.md#aio-app-logs)上的指示操作。
