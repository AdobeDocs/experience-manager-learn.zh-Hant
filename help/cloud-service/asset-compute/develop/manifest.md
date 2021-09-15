---
title: 設定Asset compute專案的manifest.yml
description: asset compute項目的manifest.yml描述了此項目中要部署的所有工作。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---

# 設定manifest.yml

`manifest.yml`位於Asset compute項目的根目錄中，描述了此項目中要部署的所有工作。

![manifest.yml](./assets/manifest/manifest.png)

## 預設工作器定義

背景工作定義為`actions`下的Adobe I/O Runtime動作項目，並由一組設定組成。

存取其他Adobe I/O整合的背景工作必須將`annotations -> require-adobe-auth`屬性設為`true`，因為此[會透過`params.auth`物件顯示背景工作的Adobe I/O憑證](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)。 當工作人員向外呼叫Adobe I/OAPI(例如Adobe Photoshop、Lightroom或Sensei API)時，通常需要這個選項，並且可以根據每個工作人員切換。

1. 開啟並查看自動生成的工作器`manifest.yml`。 包含多個Asset compute工作的項目必須在`actions`陣列下為每個工作定義一個條目。

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

每個工作人員都可以在Adobe I/O Runtime中為其執行上下文配置[limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)。 應根據工作人員要計算的資產量、速率和類型，以及其執行的工作類型，調整這些值以為工作人員提供最佳大小調整。

在設定限制之前，請檢閱[Adobe大小調整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers)。 asset compute背景工作在處理資產時可能會耗盡記憶體，導致Adobe I/O Runtime執行中斷，因此請確定背景工作的大小適當，以處理所有候選資產。

1. 將`inputs`區段新增至新的`wknd-asset-compute`動作項目。 這允許調整Asset compute工作人員的總體效能和資源分配。

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

## 已完成manifest.yml

最後`manifest.yml`看起來類似：

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

## Github上的manifest.yml

最終`.manifest.yml`可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 驗證manifest.yml

更新產生的Asset compute`manifest.yml`後，請執行本機開發工具，並確保以更新的`manifest.yml`設定成功啟動。

要啟動Asset compute項目的Asset compute開發工具，請執行以下操作：

1. 在Asset compute項目根目錄中開啟命令行（在VS代碼中，可通過「終端機」>「新終端機」直接在IDE中開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset compute開發工具將在您的預設Web瀏覽器中開啟，網址為&#x200B;__http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 當開發工具初始化時，查看命令行輸出和Web瀏覽器中的錯誤消息。
1. 若要停止「Asset compute開發工具」，請在執行`aio app run`的視窗中點選`Ctrl-C`以終止程式。

## 疑難排解

+ [YAML縮進錯誤](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
