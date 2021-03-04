---
title: 將Asset compute員工部署到Adobe I/O Runtime，以AEM作為Cloud Service
description: 'asset compute項目及其所包含的工人必須部署到Adobe I/O Runtime，以作為AEMCloud Service。 '
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---


# 部署至Adobe I/O Runtime

asset compute項目及其所包含的員工必須通過Adobe I/OCLI部署到Adobe I/O Runtime，以便用作AEMCloud Service。

部署至Adobe I/O Runtime以做為Cloud Service作AEM者服務時，只需兩個環境變數：

+ `AIO_runtime_namespace` 指出要部署的Adobe項目Firefly工作區
+ `AIO_runtime_auth` 是AdobeProject Firefly工作區的驗證憑證

在`.env`檔案中定義的其他標準變數在調用Asset compute工AEM作器時由Cloud Service隱式提供。

## 開發工作區

由於此專案是使用`Development`工作區使用`aio app init`產生，因此`AIO_runtime_namespace`會自動設為`81368-wkndaemassetcompute-development`，並在本機`.env`檔案中使用相符的`AIO_runtime_auth`。  如果`.env`檔案位於用於發出deploy命令的目錄中，則使用其值，除非它們通過作業系統級別變數導出取代，這是[stage和production](#stage-and-production)工作區的定位方式。

![使用env變數部署AIO應用程式](./assets/runtime/development__aio.png)

要部署到在項目`.env`檔案中定義的工作區，請執行以下操作：

1. 在Asset compute項目的根目錄中開啟命令行
1. 執行命令`aio app deploy`
1. 執行命令`aio app get-url`以獲取工作器URL，以便作為Cloud Service處理配置檔案AEM用於參考此自定義Asset compute工作器。 如果項目包含多個工作者，則會列出每個工作者的離散URL。

如果本地開AEM發和作為Cloud Service開發環境使用單獨的Asset compute部署，則作為Cloud Service開發的部署可以與[Stage和Production部署](#stage-and-production)以相同的方式進行管理。

## 舞台和生產工作區{#stage-and-production}

部署至舞台和生產工作區通常由您選擇的CI/CD系統完成。 asset compute專案必須分別部署至每個工作區（舞台和生產）。

設定true環境變數會覆寫`.env`中相同名稱變數的值。

![使用匯出變數來部署一體應用程式](./assets/runtime/stage__export-and-aio.png)

部署到舞台和生產環境的一般方法（通常由CI/CD系統自動執行）是：

1. 確保已安裝[Adobe I/OCLI npm模組和Asset compute插件](../set-up/development-environment.md#aio)
1. 查看要從Git部署的Asset compute專案
1. 使用與目標工作區（「舞台」或「生產」）對應的值設定環境變數
   + 這兩個必要的變數是`AIO_runtime_namespace`和`AIO_runtime_auth`，並透過工作區的&#x200B;__下載全部__&#x200B;功能，依Adobe I/O開發人員主控台的每個工作區取得。

![Adobe開發人員主控台- AIO Runtime命名空間和驗證](./assets/runtime/stage-auth-namespace.png)

可通過從命令行發出導出命令來設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作者需要任何其他變數（例如雲端儲存空間），這些變數也應匯出為環境變數。

1. 將目標工作區的所有環境變數設定為部署後，請執行deploy命令：
   + `aio app deploy`
1. 作為Cloud Service處理配置檔案的工AEM作器URL也可通過以下方式獲得：
   + `aio app get-url`。

如果Asset compute項目版本更改了工作URL，則工作URL也會更改以反映新版本，而且需要在「處理配置檔案」中更新URL。

## 工作區API布建{#workspace-api-provisioning}

當[在Adobe I/O](../set-up/firefly.md)中設定Adobe專案Firefly專案以支援本機開發時，會建立新的開發工作區，並新增&#x200B;__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__。

__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API只顯式添加到用於本地開發的工作區。 與Cloud Service環境整AEM合（獨家）的工作區需要&#x200B;__not__&#x200B;明確新增這些API，因為API會自然地以Cloud Service形式AEM提供。
