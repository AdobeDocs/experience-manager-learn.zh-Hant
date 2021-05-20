---
title: 疑難排解AEM Assets的Asset compute擴充性
description: 以下是開發和部署AEM Assets的自訂Asset compute背景工作時可能遇到的常見問題和錯誤索引，以及解決方案。
feature: asset compute微服務
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 0%

---


# 疑難排解Asset compute的擴充性

以下是開發和部署AEM Assets的自訂Asset compute背景工作時可能遇到的常見問題和錯誤索引，以及解決方案。

## 開發{#develop}

### 傳回部分繪製/損壞的轉譯{#rendition-returned-partially-drawn-or-corrupt}

+ __錯誤__:轉譯會完全呈現（當影像時）或損毀且無法開啟。

   ![返回部分繪製的格式副本](./assets/troubleshooting/develop__await.png)

+ __原因__:工作程式的 `renditionCallback` 函式在完全寫入格式副本之前退出 `rendition.path`。
+ __解決方法__:檢閱自訂背景工作程式碼，並確保所有非同步呼叫皆使用同步進行 `await`。

## 開發工具{#development-tool}

### asset compute專案{#missing-console-json}中缺少Console.json檔案

+ __錯誤：__ 錯誤：驗證時缺少必需檔案(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)（在async setupAssetCompute中）(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__  `console.json` 根目錄中缺少檔案
+ __解決方法：__ 下載新 `console.json` 的Adobe I/O專案
   1. 在console.adobe.io中，開啟Asset compute專案設定要使用的Adobe I/O專案
   1. 點選右上角的&#x200B;__下載__&#x200B;按鈕
   1. 使用檔案名`console.json`將下載的檔案儲存至Asset compute專案的根目錄

### manifest.yml{#incorrect-yaml-indentation}中的YAML縮排不正確

+ __錯誤：__ YAMLException:在行X、列Y處的映射項縮排錯誤：(通過從命令中取 `aio app run` 消)
+ __原因：__ Yaml檔案是白間隔的，可能是縮排不正確。
+ __解決方法：__ 檢閱您的 `manifest.yml` ，並確保所有縮排正確無誤。

### memorySize限制設定為過低{#memorysize-limit-is-set-too-low}

+ __錯誤：__  本地開發伺服器OpenWhisk錯誤：PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400(Bad Request)—> &quot;請求內容格式不正確：requirement失敗：記憶體低於允許的閾值64 MB 134217728 B」
+ __原因：__ 中 `memorySize` 的工作器限制設定在允 `manifest.yml` 許的最小閾值以下，如錯誤消息所報告的，以位元組為單位。
+ __解決方__  法：檢 `memorySize` 閱中的 `manifest.yml` 限制，並確定這些限制都大於允許的最低臨界值。

### 由於缺少private.key{#missing-private-key}，開發工具無法啟動

+ __錯誤：__ 本地開發伺服器錯誤：驗證PrivateKeyFile時缺少必需檔案…….（通過`aio app run`命令的標準輸出）
+ __原因：__ 檔 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 案 `.env` 中的值未指向 `private.key` 或 `private.key` 當前用戶無法讀取。
+ __解決方__ 法：檢 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 閱檔 `.env` 案中的值，並確認包含檔案系統上 `private.key` 的完整絕對路徑。

### 源檔案下拉清單不正確{#source-files-dropdown-incorrect}

asset compute開發工具可能會進入其提取過時資料的狀態，在&#x200B;__來源檔案__&#x200B;下拉式清單中最能注意到顯示錯誤項目。

+ __錯誤：__ 來源檔案下拉式清單顯示錯誤的項目。
+ __原因：__ 過時的快取瀏覽器狀態導致
+ __解決方法：__ 在您的瀏覽器中，完全清除瀏覽器標籤的「應用程式狀態」、瀏覽器快取、本機儲存和服務背景工作。

### 遺失或無效的devToolToken查詢參數{#missing-or-invalid-devtooltoken-query-parameter}

+ __錯誤：__ Asset compute開發工具中的「未授權」通知
+ __原因：__ `devToolToken` 遺失或無效
+ __解決方法：__ 關閉「Asset compute開發工具」瀏覽器窗口，終止通過命令啟動的任何正在運行的「開發工具」 `aio app run` 進程，然後重新啟動「開發工具」(使 `aio app run`用)。

### 無法刪除源檔案{#unable-to-remove-source-files}

+ __錯誤：__ 無法從開發工具UI中移除新增的來源檔案
+ __原因：__ 此功能尚未實作
+ __解決方法：__ 使用中定義的憑證登入您的雲端儲存空間提供 `.env`者。找出開發工具使用的容器（也在`.env`中指定），導覽至&#x200B;__source__&#x200B;資料夾，並刪除任何來源影像。 如果刪除的源檔案繼續顯示在下拉清單中，因為它們可能在開發工具的「應用程式狀態」中快取到本地，則您可能需要執行[源檔案下拉清單中列出的步驟，該步驟不正確](#source-files-dropdown-incorrect)。

   ![Microsoft Azure Blob儲存](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 測試{#test}

### 測試執行期間未生成任何格式副本{#test-no-rendition-generated}

+ __錯誤：__ 失敗：未產生任何轉譯。
+ __原因：__ 工作程式由於意外錯誤（如JavaScript語法錯誤）而無法產生轉譯。
+ __解決方法：__ 檢閱測試執行的 `test.log` 位 `/build/test-results/test-worker/test.log`置。在此檔案中找到與失敗測試案例對應的區段，並檢閱錯誤。

   ![疑難排解 — 未產生轉譯](./assets/troubleshooting/test__no-rendition-generated.png)

### 測試生成錯誤的格式副本，導致測試失敗{#tests-generates-incorrect-rendition}

+ __錯誤：__ 失敗：格式副本&#39;rendition.xxx&#39;未如預期。
+ __原因：__ 工作器輸出的格式副本與測試案 `rendition.<extension>` 例中提供的不同。
   + 如果未以與測試案例中本地生成的格式副本完全相同的方式建立預期的`rendition.<extension>`檔案，則測試可能會失敗，因為位中可能存在某些差異。 例如，如果Asset compute背景工作使用API變更對比，而預期的結果是透過調整Adobe Photoshop CC中的對比來建立，則檔案可能會顯示相同，但位元的微幅變化可能會不同。
+ __解析度：__ 導覽至，檢閱測試中的轉譯輸出， `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`並與測試案例中的預期轉譯檔案比較。若要建立確切的預期資產，請執行下列任一動作：
   + 使用「開發工具」產生轉譯、驗證其正確性，並將其當作預期的轉譯檔案使用
   + 或者，在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`驗證測試產生的檔案，驗證其正確性，並將其用作預期的轉譯檔案

## 偵錯

### 調試器未附加{#debugger-does-not-attach}

+ __錯誤__:處理啟動時出錯：錯誤：無法在……連接到調試目標
+ __原因__:本地系統上未運行Docker Desktop。檢閱VS程式碼偵錯主控台（檢視>除錯主控台），確認回報此錯誤，以確認此情況。
+ __解決方法__:啟 [動Docker Desktop並確認安裝了必要的Docker映像](./set-up/development-environment.md#docker)。

### 斷點未暫停{#breakpoints-no-pausing}

+ __錯誤__:從可除錯的開發工具執行Asset compute背景工作時，VS程式碼不會在中斷點暫停。

#### 未附加VS代碼調試器{#vs-code-debugger-not-attached}

+ __原因：__  VS程式碼偵錯工具已停止/中斷連線。
+ __解決方法：__ 重新啟動VS Code Debugger，並透過觀看VS Code Debug Output主控台（檢視>除錯主控台）來確認其已附加

#### 工作程式開始執行後附加的VS代碼調試器{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ 點選「執行中開發工具」前未附加VS程 ____ 式碼偵錯工具。
+ __解決方法：__ 檢閱VS Code的Debug Console（檢視>除錯主控台），然後從開發工具重新執行Asset compute背景工作，以確認除錯工具已附加。

### 調試{#worker-times-out-while-debugging}時工作器超時

+ __錯誤__:Debug Console報表「動作將在 — XXX毫秒內逾時」，或 [Asset compute開發工具的](./develop/development-tool.md) 轉譯預覽無限期旋轉或
+ __原因__:調試期間超過manifest. [ymlis中](./develop/manifest.md) 定義的工作超時。
+ __解決方法__:在manifest.ymlor中暫時增加工作器的逾 [時，](./develop/manifest.md) 會加速除錯活動。

### 無法終止調試器進程{#cannot-terminate-debugger-process}

+ __錯誤__: `Ctrl-C` 在命令列上不會終止除錯程式(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3. `@adobe/aio-cli-plugin-asset-compute` x中發生錯誤，導致無 `Ctrl-C` 法辨識為終止命令。
+ __解決方法__:更 `@adobe/aio-cli-plugin-asset-compute` 新至1.4.1+版

   ```
   $ aio update
   ```

   ![疑難排解 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM{#custom-rendition-missing-from-asset}中的資產遺失自訂轉譯

+ __錯誤：__ 新資產和重新處理的資產處理成功，但缺少自訂轉譯

#### 處理配置檔案未應用於上階資料夾

+ __原因：__ 資產在具有使用自訂背景工作之處理設定檔的資料夾下不存在
+ __解決方法：__ 將處理設定檔套用至資產的上階資料夾

#### 處理配置檔案被較低的處理配置檔案取代

+ __原因：__ 資產存在於套用自訂背景工作處理設定檔的資料夾下方，但該資料夾與資產之間已套用了不使用客戶背景工作的不同處理設定檔。
+ __解決方法：__ 合併或調解兩個處理設定檔，並移除中間處理設定檔

### AEM{#asset-processing-fails}中的資產處理失敗

+ __錯誤：資__ 產處理失敗徽章顯示在資產上
+ __原因：__ 自訂背景工作執行中發生錯誤
+ __解決方法：__ 依照使用除錯Adobe I/O Runtime [動作](./test-debug/debug.md#aio-app-logs) 的指示 `aio app logs`操作。


