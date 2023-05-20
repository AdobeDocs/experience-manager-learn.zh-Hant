---
title: 配置Asset compute項目的manifest.yml
description: asset compute項目的manifest.yml描述了要部署的此項目中的所有工作程式。
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

# 配置manifest.yml

的 `manifest.yml`，位於Asset compute項目的根目錄中，描述了要部署的此項目中的所有工作程式。

![manifest.yml](./assets/manifest/manifest.png)

## 預設工作人員定義

工作人員定義為Adobe I/O Runtime活動條目 `actions`，由一組配置組成。

訪問其他Adobe I/O整合的工作人員必須設定 `annotations -> require-adobe-auth` 屬性 `true` 這 [公開工作人員的Adobe I/O憑據](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) 通過 `params.auth` 的雙曲餘切值。 當工作人員調出到Adobe I/OAPI(如Adobe Photoshop、Lightroom或SenseiAPI)時，這通常是必需的，並且可以按工作人員切換。

1. 開啟並查看自動生成的工作人員 `manifest.yml`。 包含多個Asset compute工作人員的項目，必須在 `actions` 陣列。

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

每個工作人員都可以配置 [限制](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) 在Adobe I/O Runtime的執行情況。 應根據將要計算的資產數量、比率和類型以及所執行的工作類型來調整這些值，以為員工提供最佳規模調整。

審閱 [Adobe規模指導](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) 設定限制之前。 asset compute員工在處理資產時可能會耗盡記憶體，導致Adobe I/O Runtime執行死亡，因此請確保員工的大小適合處理所有候選資產。

1. 添加 `inputs` 的 `wknd-asset-compute` 按鈕。 這允許調整Asset compute工作人員的總體效能和資源分配。

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

決賽 `manifest.yml` 看起來如下：

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

## manifest.yml在Github上

決賽 `.manifest.yml` 在Github上提供，網址為：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 正在驗證manifest.yml

生成的Asset compute `manifest.yml` 已更新，運行本地開發工具，並確保在更新後成功啟動 `manifest.yml` 的子菜單。

要啟動Asset compute項目的Asset compute開發工具，請執行以下操作：

1. 在Asset compute項目根目錄中開啟命令行（在VS代碼中，可通過「終端」>「新終端」直接在IDE中開啟），然後執行以下命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute開發工具將在預設Web瀏覽器中開啟， __http://localhost:9000__。

   ![aio應用程式運行](assets/environment-variables/aio-app-run.png)

1. 在開發工具初始化時，查看命令行輸出和Web瀏覽器中的錯誤消息。
1. 要停止Asset compute開發工具，請點擊 `Ctrl-C` 窗口中 `aio app run` 終止進程。

## 疑難排解

+ [YAML縮進不正確](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得過低](../troubleshooting.md#memorysize-limit-is-set-too-low)
