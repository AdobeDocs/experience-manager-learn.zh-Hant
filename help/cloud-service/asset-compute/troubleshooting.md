---
title: 疑難排解AEM Assets的Asset compute擴充性
description: 以下是開發和部署AEM Assets自訂Asset compute工人時可能遇到的常見問題和錯誤以及解決方案的索引。
feature: asset compute微服務
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1249'
ht-degree: 0%

---


# 疑難排解Asset compute擴充性

以下是開發和部署AEM Assets自訂Asset compute工人時可能遇到的常見問題和錯誤以及解決方案的索引。

## 開發{#develop}

### 傳回部分繪製／損毀的轉譯{#rendition-returned-partially-drawn-or-corrupt}

+ __錯誤__:轉譯會呈現不完整（當影像時）或損毀且無法開啟。

   ![傳回部分繪製的轉譯](./assets/troubleshooting/develop__await.png)

+ __原因__:工作者的功 `renditionCallback` 能會在轉譯完全寫入之前退出 `rendition.path`。
+ __解析度__:檢閱自訂工作程式碼，並確保所有非同步呼叫都是使用同步呼叫 `await`。

## 開發工具{#development-tool}

### asset compute專案{#missing-console-json}中遺失Console.json檔案

+ __錯誤：錯__ 誤：在驗證時缺少所需檔案(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)，位於async setupAssetCompute(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ 文 `console.json` 件在Asset compute項目的根目錄中缺失
+ __解析度：__ 下載新 `console.json` 的Adobe I/O專案
   1. 在console.adobe.io中，開啟Adobe I/O專案，該Asset compute專案設定為使用
   1. 點選右上角的&#x200B;__Download__&#x200B;按鈕
   1. 使用檔案名`console.json`將下載的檔案保存到Asset compute項目的根目錄

### manifest.yml{#incorrect-yaml-indentation}中的YAML縮排不正確

+ __錯誤：__ YAMLException:在行X，列Y:（通過從命令中標準輸出）處映射條目的縮 `aio app run` 排錯誤
+ __原因：__ Yaml檔案會以白間隔區分，可能是縮排不正確。
+ __解析度：__ 檢閱 `manifest.yml` 並確保所有縮排正確。

### memorySize限制設定為過低{#memorysize-limit-is-set-too-low}

+ __錯誤：本__  地開發伺服器OpenWhisk錯誤：PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true傳回HTTP 400（錯誤請求）—> &quot;請求內容格式錯誤：requirement失敗：記憶體低於允許的閾值64 MB 134217728 B&quot;
+ __原因：__ 錯誤 `memorySize` 消息以位元組為單位報告， `manifest.yml` 已將中的工作器限制設定在允許的最小閾值以下。
+ __解析度：__  檢閱 `memorySize` 中的限制 `manifest.yml` ，並確定這些限制都大於允許的最小閾值。

### 由於缺少private.key{#missing-private-key}，開發工具無法啟動

+ __錯誤：本__ 地開發伺服器錯誤：validatePrivateKeyFile中缺少所需檔案…….（通過`aio app run`命令的標準輸出）
+ __原因：__ 檔 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 案中 `.env` 的值不指向 `private.key` 或 `private.key` 目前使用者無法讀取。
+ __解析度：__ 檢視 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 檔案 `.env` 中的值，並確定它包含檔案系統上的完整 `private.key` 絕對路徑。

### 來源檔案下拉式清單不正確{#source-files-dropdown-incorrect}

asset compute開發工具可能會進入其提取過時資料的狀態，在顯示錯誤項目的&#x200B;__來源檔案__&#x200B;下拉式清單中最為明顯。

+ __錯誤：來源__ 檔案下拉式清單顯示錯誤項目。
+ __原因：過__ 時的快取瀏覽器狀態會導致
+ __解析度：__ 在您的瀏覽器中，請完全清除瀏覽器標籤的「應用程式狀態」、瀏覽器快取、本機儲存空間和服務工作者。

### 遺失或無效的devToolToken查詢參數{#missing-or-invalid-devtooltoken-query-parameter}

+ __錯誤：__ 「未授權」通知在Asset compute開發工具中
+ __原因：__ `devToolToken` 遺失或無效
+ __解析度：__ 關閉「Asset compute開發工具」瀏覽器視窗、終止任何透過命令啟動的執行中「開發工具」 `aio app run` 程式，然後重新啟動「開發工具」(使用 `aio app run`)。

### 無法刪除源檔案{#unable-to-remove-source-files}

+ __錯誤：__ 無法從「開發工具」UI移除新增的來源檔案
+ __原因：__ 此功能尚未實作
+ __解析度：__ 使用中定義的憑證登入您的雲端儲存空間提供者 `.env`。找出「開發工具」（也在`.env`中指定）所使用的容器，導覽至&#x200B;__source__&#x200B;資料夾，並刪除任何來源影像。 如果刪除的源檔案繼續顯示在下拉式清單中，因為它們可能會在開發工具「應用程式狀態」的本機快取中，則YOu可能需要執行[來源檔案下拉式清單中描述的步驟不正確](#source-files-dropdown-incorrect)。

   ![Microsoft Azure Blob儲存空間](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 測試{#test}

### 測試執行期間未產生轉譯{#test-no-rendition-generated}

+ __錯誤：__ 失敗：未產生轉譯。
+ __原因：__ 由於意外錯誤（如JavaScript語法錯誤），工作者無法生成轉譯。
+ __解析度：__ 請參閱測試執行的 `test.log` 網址 `/build/test-results/test-worker/test.log`。在此檔案中找到與失敗測試案例對應的部分，並查看錯誤。

   ![疑難排解——未產生轉譯](./assets/troubleshooting/test__no-rendition-generated.png)

### Test會產生錯誤的轉譯，導致測試失敗{#tests-generates-incorrect-rendition}

+ __錯誤：__ 失敗：轉譯&#39;rendition.xxx&#39;不如預期。
+ __原因：__ 工作者輸出與測試案例中提供的格 `rendition.<extension>` 式副本不同的格式副本。
   + 如果預期的`rendition.<extension>`檔案建立方式與測試案例中本機產生的轉譯方式完全相同，則測試可能會失敗，因為位可能會有一些差異。 例如，如果Asset compute工作者使用API更改對比，並且通過調整Adobe Photoshop CC的對比來建立預期結果，則檔案的顯示可能相同，但位中的細微變化可能不同。
+ __解析度：__ 導覽至測試以檢視轉譯輸出，並 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`將它與測試案例中預期的轉譯檔案比較。若要建立精確的預期資產，請執行下列任一動作：
   + 使用「開發工具」產生轉譯、驗證其正確性，並將其當做預期的轉譯檔案使用
   + 或者，在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`驗證測試產生的檔案，驗證其正確性，並將它當做預期的轉譯檔案使用

## 偵錯

### 除錯程式未附加{#debugger-does-not-attach}

+ __錯誤__:處理啟動時出錯：錯誤：無法連接到調試目標……
+ __原因__:Docker Desktop未在本地系統上運行。檢視VS程式碼除錯主控台（「檢視>除錯主控台」）以確認此錯誤，並報告此錯誤。
+ __解析度__:啟動 [Docker Desktop，並確認安裝了必要的Docker映像](./set-up/development-environment.md#docker)。

### 中斷點不暫停{#breakpoints-no-pausing}

+ __錯誤__:從可除錯的開發工具執行Asset compute工具時，VS代碼不會在中斷點暫停。

#### VS代碼調試器未連接{#vs-code-debugger-not-attached}

+ __原因：__ VS代碼調試器已停止／斷開連接。
+ __解析度：__ 重新啟動VS程式碼除錯程式，並透過檢視VS程式碼除錯輸出主控台（檢視>除錯主控台）來驗證它是否連接

#### VS工作器開始執行後附加的代碼調試器{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：在__ 點選「執行程式開發工具」之前，VS程式碼除錯程 ____ 式未附加。
+ __解析度：__ 請檢視VS程式碼的除錯控制台（「檢視>除錯控制台」），然後從開發工具重新執行Asset compute工具，以確保除錯程式已附加。

### 調試時工作器超時{#worker-times-out-while-debugging}

+ __錯誤__:除錯主控台報告「動作將逾時-XXX毫秒」或 [Asset compute開發工具的轉](./develop/development-tool.md) 譯預覽無限期旋轉或
+ __原因__:除錯期間超出manifest. [](./develop/manifest.md) ymlis中定義的工作器逾時。
+ __解析度__:暫時增加manifest. [ymlor中工作者的逾時](./develop/manifest.md) 時間，以加速除錯活動。

### 無法終止調試器進程{#cannot-terminate-debugger-process}

+ __錯誤__: `Ctrl-C` 命令行上不終止調試程式進程(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3.x `@adobe/aio-cli-plugin-asset-compute` 中出現錯誤，導致 `Ctrl-C` 無法識別為終止命令。
+ __解析度__:更 `@adobe/aio-cli-plugin-asset-compute` 新至1.4.1+版

   ```
   $ aio update
   ```

   ![疑難排解——一體機更新](./assets/troubleshooting/debug__terminate.png)

## Deploy{#deploy}

### {#custom-rendition-missing-from-asset}中資產中缺少的自AEM訂轉譯

+ __錯誤：__ 新資產和重新處理資產處理成功，但缺少自訂轉譯

#### 處理配置檔案未應用到祖先資料夾

+ __原因：__ 資產不存在於使用自訂工作器的「處理設定檔」資料夾下
+ __解析度：__ 將處理設定檔套用至資產的上階資料夾

#### 處理設定檔由較低的處理設定檔取代

+ __原因：__ 資產存在於套用自訂工作者處理設定檔的資料夾下方，但該資料夾與資產之間已套用了不使用客戶工作者的不同處理設定檔。
+ __解析度：__ 合併或協調兩個處理描述檔，並移除中間處理描述檔

### {#asset-processing-fails}中的資AEM產處理失敗

+ __錯誤：資__ 產處理失敗標章顯示在資產上
+ __原因：__ 自定義工作器執行時出錯
+ __解析度：__ 依照使用除錯Adobe I/O Runtime [活動](./test-debug/debug.md#aio-app-logs) 的指示進行 `aio app logs`。


