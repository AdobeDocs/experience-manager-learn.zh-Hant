---
title: 設定Asset compute專案的manifest.yml
description: asset compute項目的manifest.yml描述了要部署的此項目中的所有員工。
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---


# 設定manifest.yml

`manifest.yml`位於Asset compute項目的根目錄中，描述了要部署的此項目中的所有員工。

![manifest.yml](./assets/manifest/manifest.png)

## 預設工作器定義

工人定義為`actions`下的Adobe I/O Runtime操作條目，由一組配置組成。

訪問其他Adobe I/O整合的工作者必須將`annotations -> require-adobe-auth`屬性設定為`true`，因為此[通過`params.auth`對象公開工作者的Adobe I/O憑據](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)。 當員工呼出Adobe I/OAPI(例如Adobe Photoshop、Lightroom或Sensei API)時，通常需要此選項，而且可以依每位員工切換。

1. 開啟並查看自動生成的工作器`manifest.yml`。 包含多個Asset compute工作的項目必須在`actions`陣列下為每個工作者定義一個條目。

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

每個員工都可以配置[limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)以用於其在Adobe I/O Runtime的執行上下文。 應根據員工要計算的資產量、比率和類型，以及所執行的工作類型，調整這些值，以提供最佳的員工規模。

在設定限制之前，請查看[Adobe大小指南](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers)。 asset compute工人在處理資產時可能會用盡記憶體，導致Adobe I/O Runtime執行死亡，因此請確保員工的大小適當，以處理所有候選資產。

1. 將`inputs`區段新增至新的`wknd-asset-compute`動作項目。 這允許調整Asset compute工作者的總體效能和資源分配。

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

最終`manifest.yml`看起來如下：

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

## github上的manifest.yml

最終`.manifest.yml`可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 驗證manifest.yml

更新產生的Asset compute`manifest.yml`後，請執行本機開發工具，並確保以更新的`manifest.yml`設定成功啟動。

要啟動Asset compute項目的Asset compute開發工具，請執行以下操作：

1. 在Asset compute項目根目錄中開啟命令行（在VS代碼中，此命令可直接在IDE中通過「終端機>新終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset compute開發工具將在您的預設Web瀏覽器中開啟，位於&#x200B;__http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 當開發工具初始化時，請觀看命令列輸出和Web瀏覽器中的錯誤訊息。
1. 若要停止Asset compute開發工具，請在執行`aio app run`的視窗中點選`Ctrl-C`以終止程式。

## 疑難排解

+ [錯誤的YAML縮排](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定為過低](../troubleshooting.md#memorysize-limit-is-set-too-low)
