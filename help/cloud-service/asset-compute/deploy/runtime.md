---
title: 將Asset compute工作人員部署到Adobe I/O Runtime，以便與AEMas a Cloud Service
description: asset compute項目及其所包含的工人必須部署到Adobe I/O Runtime，供as a Cloud Service使AEM用。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# 部署到Adobe I/O Runtime

asset compute項目及其所包含的工作程式必須通過Adobe I/OCLI部署到Adobe I/O Runtime，以供as a Cloud Service使AEM用。

部署到Adobe I/O Runtime供as a Cloud Service作者服AEM務使用時，只需要兩個環境變數：

+ `AIO_runtime_namespace` 指向要部署到的App Builder工作區
+ `AIO_runtime_auth` 是App Builder工作區的驗證憑據

中定義的其他標準變數 `.env` 檔案在調用Asset compute工AEM作器時由as a Cloud Service隱式提供。

## 開發工作區

因為此項目是使用 `aio app init` 使用 `Development` 工作區， `AIO_runtime_namespace` 自動設定為 `81368-wkndaemassetcompute-development` 匹配 `AIO_runtime_auth` 我們的本地 `.env` 的子菜單。  如果 `.env` 檔案存在於用於發出deploy命令的目錄中，其值將被使用，除非它們通過OS級別變數導出被取代。 [階段和生產](#stage-and-production) 工作區是目標。

![aio應用程式使用.env變數部署](./assets/runtime/development__aio.png)

部署到項目中定義的工作區 `.env` 檔案：

1. 在Asset compute項目的根目錄中開啟命令行
1. 執行命令 `aio app deploy`
1. 執行命令 `aio app get-url` 獲取工作器URL，供在「AEMas a Cloud Service處理配置檔案」中使用，以引用此自定義Asset compute工作器。 如果項目包含多個工作進程，則列出每個工作進程的離散URL。

如果本地開發和AEMas a Cloud Service開發環境使用單獨的Asset compute部署，則可以AEM以與as a Cloud Service開發相同的方式管理部署 [階段和生產部署](#stage-and-production)。

## 舞台和生產工作區{#stage-and-production}

部署到舞台和生產工作區通常由您選擇的CI/CD系統完成。 asset compute項目必須離散地部署到每個工作區（階段和生產）。

設定真環境變數將覆蓋中同名變數的值 `.env`。

![aio應用程式部署使用導出變數](./assets/runtime/stage__export-and-aio.png)

通常由CI/CD系統自動執行的一般方法是：

1. 確保 [Adobe I/OCLI npm模組和Asset compute插件](../set-up/development-environment.md#aio) 已安裝
1. 簽出要從Git部署的Asset compute項目
1. 使用與目標工作區（階段或生產）對應的值設定環境變數
   + 兩個必需變數是 `AIO_runtime_namespace` 和 `AIO_runtime_auth` 並通過Workspace在Adobe I/O開發人員控制台中按工作區獲取 __全部下載__ 的子菜單。

![Adobe Developer控制台 — AIO運行時命名空間和身份驗證](./assets/runtime/stage-auth-namespace.png)

可通過從命令行發出導出命令來設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作人員需要任何其他變數（如雲儲存），也應將這些變數導出為環境變數。

1. 在為要部署的目標工作區設定所有環境變數後，執行deploy命令：
   + `aio app deploy`
1. as a Cloud Service處理配置檔案引用的工作AEM人員URL也可通過以下方式提供：
   + `aio app get-url`。

如果Asset compute項目版本更改，則工作URL也會更改以反映新版本，並且需要在「處理配置檔案」中更新該URL。

## 工作區API設定{#workspace-api-provisioning}

當 [在Adobe I/O中設定App Builder項目](../set-up/app-builder.md) 為支援本地開發，建立了一個新的開發工作區， __asset compute、 I/O事件__ 和 __I/O事件管理API__ 被加進去。

的 __asset compute、 I/O事件__ 和 __I/O事件管理API__ APIS僅顯式添加到用於局部開發的工作區中。 與as a Cloud Service環境整合(獨AEM佔)的工作區 __不__ 需要顯式添加這些API，因為API自然可供AEMas a Cloud Service。
