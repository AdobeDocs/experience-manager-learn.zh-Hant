---
title: 將Asset compute背景工作部署至Adobe I/O Runtime，以搭配AEM作為Cloud Service使用
description: 'asset compute專案及其所包含的背景工作必須部署至Adobe I/O Runtime，才能供AEM作為Cloud Service使用。 '
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# 部署至Adobe I/O Runtime

asset compute項目及其包含的工作程式必須透過Adobe I/OCLI部署至Adobe I/O Runtime，以供AEM作為Cloud Service使用。

部署至Adobe I/O Runtime供AEM作為Cloud Service製作服務使用時，只需要兩個環境變數：

+ `AIO_runtime_namespace` 指向AdobeProject Firefly Workspace以部署至
+ `AIO_runtime_auth` 是AdobeProject Firefly工作區的驗證憑證

`.env`檔案中定義的其他標準變數在調用Asset compute工作器時由AEM隱式提供為Cloud Service。

## 開發工作區

因為此專案是使用`Development`工作區使用`aio app init`產生，因此會在本機`.env`檔案中，將`AIO_runtime_namespace`自動設為`81368-wkndaemassetcompute-development`並搭配使用相符的`AIO_runtime_auth`。  如果`.env`檔案存在於用於發出部署命令的目錄中，則會使用其值，除非它們通過作業系統級別變數導出取代，即[stage和生產](#stage-and-production)工作區的目標定位方式。

![使用env變數部署aio應用程式](./assets/runtime/development__aio.png)

要部署到項目`.env`檔案中的工作區定義：

1. 在Asset compute項目的根目錄中開啟命令行
1. 執行命令`aio app deploy`
1. 執行命令`aio app get-url`以取得工作URL，以便在AEM中作為Cloud Service處理設定檔使用，以參考此自訂Asset compute工作。 如果項目包含多個工作人員，則會列出每個工作人員的離散URL。

如果本機開發和AEM as aCloud Service開發環境使用個別的Asset compute部署，則部署至AEM as aCloud Service開發可以與[預備和生產部署](#stage-and-production)相同的方式管理。

## 預備和生產工作區{#stage-and-production}

部署至預備和生產工作區通常由您選擇的CI/CD系統完成。 必須將Asset compute專案分散部署至每個工作區（預備和生產）。

設定true環境變數會覆寫`.env`中相同名稱變數的值。

![使用匯出變數部署aio應用程式](./assets/runtime/stage__export-and-aio.png)

部署至預備和生產環境的一般方法（通常由CI/CD系統自動化）是：

1. 確保[Adobe I/OCLI npm模組和Asset compute插件](../set-up/development-environment.md#aio)已安裝
1. 查看要從Git部署的Asset compute專案
1. 使用與目標工作區（預備或生產）對應的值設定環境變數
   + 這兩個必要變數分別為`AIO_runtime_namespace`和`AIO_runtime_auth`，並透過工作區的&#x200B;__下載全部__&#x200B;功能，在Adobe I/O開發人員控制台中針對每個工作區取得。

![Adobe開發人員主控台 — AIO執行階段命名空間與驗證](./assets/runtime/stage-auth-namespace.png)

通過從命令行發出導出命令，可以設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute背景工作需要任何其他變數（例如雲端儲存空間），這些變數也應匯出為環境變數。

1. 將目標工作區的所有環境變數設定為部署後，請執行部署命令：
   + `aio app deploy`
1. AEM參考的Cloud Service處理設定檔工作URL也可透過下列方式取得：
   + `aio app get-url`。

如果Asset compute專案版本變更了背景URL也會變更以反映新版本，則URL必須在處理設定檔中更新。

## 工作區API布建{#workspace-api-provisioning}

當[在Adobe I/O](../set-up/firefly.md)中設定AdobeProject Firefly專案以支援本機開發時，會建立新的開發工作區，並新增&#x200B;__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__。

__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API只會明確新增至用於本機開發的工作區。 將AEM作為Cloud Service環境整合（僅限）的工作區&#x200B;__not__&#x200B;需要明確新增這些API，因為API可自然供AEM作為Cloud Service使用。
