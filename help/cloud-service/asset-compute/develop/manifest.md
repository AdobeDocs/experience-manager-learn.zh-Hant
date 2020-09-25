---
title: 設定資產計算專案的manifest.yml
description: Asset Compute專案的manifest.yml說明此應用程式中要部署的所有工作者。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---


# 設定manifest.yml

位 `manifest.yml`於「資產計算」項目根目錄中的The the root in the Asset Compute project, describes all the workers in to be deployed.

![manifest.yml](./assets/manifest/manifest.png)

## 預設工作器定義

工作程式定義為下面的Adobe I/O Runtime操作項 `actions`目，由一組配置組成。

存取其他Adobe I/O整合的工作者必須將 `annotations -> require-adobe-auth` 屬性設 `true` 為 [透過物件公開工作者的Adobe I/O認](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)`params.auth` 證。 當員工呼出Adobe I/O API（例如Adobe Photoshop、Lightroom或Sensei API）時，通常需要此項功能，而且可依每位員工切換。

1. 開啟並查看自動生成的員工 `manifest.yml`。 包含多個資產計算工作的項目，必須為陣列下的每個工作者定義一個 `actions` 條目。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## 定義限制

每位員工都可在 [Adobe I](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) /O Runtime中設定其執行內容的限制。 應根據員工要計算的資產量、比率和類型，以及所執行的工作類型，調整這些值，以提供最佳的員工規模。

在設定 [限制前，請先檢閱](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) Adobe調整大小的指引。 資產計算工作者在處理資產時可能會耗盡記憶體，導致Adobe I/O Runtime執行中斷，因此請確定工作者的大小是否適當，以處理所有候選資產。

1. 新增 `inputs` 區段至新動 `wknd-asset-compute` 作項目。 這允許調整資產計算工作者的總體效能和資源分配。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## 完成的manifest.yml

最終結 `manifest.yml` 果如下：

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## 驗證manifest.yml

更新產生的資產計 `manifest.yml` 算後，請執行本機開發工具，並確保以更新的設定成功 `manifest.yml` 啟動。

要為資產計算項目啟動資產計算開發工具，請執行以下操作：

1. 在「資產計算」項目根目錄中開啟命令行（在VS代碼中，此命令可直接在IDE中通過「終端機>新終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機資產計算開發工具將在您的預設網頁瀏覽器中開啟，網址為 __http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 當開發工具初始化時，請觀看命令列輸出和Web瀏覽器中的錯誤訊息。
1. 若要停止資產計算開發工具，請在執 `Ctrl-C` 行的視窗中點選以 `aio app run` 終止程式。

## 疑難排解

### 錯誤的YAML縮排

+ __錯誤：__ YAMLException:在行X，列Y:（通過從命令中標準輸出）處對映條目進行不 `aio app run` 良縮進
+ __原因：__ Yaml檔案會以白色間隔顯示，可能是縮排不正確。
+ __解析度：__ 檢閱您的 `manifest.yml` 縮排，並確保所有縮排正確無誤。

### memorySize限制設定為過低

+ __錯誤：__ 本機開發伺服器OpenWhiskError:PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true傳回HTTP 400（錯誤請求）—> &quot;請求內容格式錯誤：requirement失敗：記憶體低於允許的閾值64 MB 134217728 B&quot;
+ __原因：__ 資訊 `memorySize` 清單中的限制設定在錯誤訊息所報告的最小允許臨界值以下（以位元組為單位）。
+ __解析度：__ 檢閱中 `memorySize` 的限制， `manifest.yml` 並確保這些限制都大於允許的最小閾值。