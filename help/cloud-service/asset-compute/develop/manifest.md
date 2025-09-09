---
title: 設定Asset Compute專案的manifest.yml
description: Asset Compute專案的manifest.yml說明此專案中要部署的所有背景工作。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# 設定 manifest.yml

位於Asset Compute專案根目錄中的`manifest.yml`說明此專案中要部署的所有背景工作。

![manifest.yml](./assets/manifest/manifest.png)

## 預設背景工作定義

背景工作被定義為`actions`下的Adobe I/O Runtime動作專案，由一組設定組成。

存取其他Adobe I/O整合的工作者必須將`annotations -> require-adobe-auth`屬性設定為`true`，因為此[透過](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)物件公開工作者的Adobe I/O認證`params.auth`。 當背景工作呼叫Adobe I/O API (例如Adobe Photoshop或Lightroom API)時，通常需要此專案，而且每個背景工作可切換。

1. 開啟並檢閱自動產生的背景工作`manifest.yml`。 包含多個Asset Compute背景工作程式的專案，必須為`actions`陣列下的每個背景工作程式定義一個專案。

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
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, or Lightroom.
```

## 定義限制

每個背景工作可以在Adobe I/O Runtime中為其執行內容設定[限制](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)。 這些值應該根據工作者的數量、比率、將計算的資產型別，以及執行的工作型別，調整為工作者提供最佳規模。

在設定限制之前，請檢閱[Adobe大小調整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers)。 Asset Compute背景工作處理資產時，記憶體可能會用完，導致Adobe I/O Runtime執行終止，因此請確保背景工作的大小適合處理所有候選資產。

1. 新增`inputs`區段至新的`wknd-asset-compute`動作專案。 這可調整Asset Compute背景工作的整體效能和資源配置。

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

## 已完成的manifest.yml

最終`manifest.yml`看起來像這樣：

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

Github上的最終`.manifest.yml`位於：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 正在驗證manifest.yml

更新產生的Asset Compute `manifest.yml`後，請執行本機開發工具，並確保以更新的`manifest.yml`設定成功啟動。

若要啟動適用於Asset Compute專案的Asset Compute開發工具：

1. 在Asset Compute專案根目錄中開啟命令列（在VS Code中，這可以直接在IDE中透過「終端機>新終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset Compute開發工具將在您的預設網頁瀏覽器中開啟，網址為__http://localhost :9000__。

   ![aio應用程式執行](assets/environment-variables/aio-app-run.png)

1. 在開發工具初始化時，請觀察命令列輸出和網頁瀏覽器中的錯誤訊息。
1. 若要停止Asset Compute開發工具，請在執行`Ctrl-C`的視窗中點選`aio app run`以終止處理序。

## 疑難排解

+ [不正確的YAML縮排](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
