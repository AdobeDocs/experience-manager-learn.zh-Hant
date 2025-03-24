---
title: 將Asset Compute背景工作程式部署到Adobe I/O Runtime以與AEM as a Cloud Service搭配使用
description: Asset Compute專案及其所包含的背景工作必須部署至Adobe I/O Runtime，才可供AEM as a Cloud Service使用。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# 部署至Adobe I/O Runtime

Asset Compute專案及其所包含的背景工作必須透過Adobe I/O CLI部署至Adobe I/O Runtime，才可供AEM as a Cloud Service使用。

部署到Adobe I/O Runtime以供AEM as a Cloud Service Author服務使用時，只需要兩個環境變數：

+ `AIO_runtime_namespace`指向App Builder Workspace以部署到
+ `AIO_runtime_auth`是App Builder工作區的驗證認證

在`.env`檔案中定義的其他標準變數，在AEM as a Cloud Service叫用Asset Compute背景工作程式時以隱含方式提供。

## 開發工作區

因為此專案是使用`aio app init`使用`Development`工作區所產生，`AIO_runtime_namespace`會自動設定為`81368-wkndaemassetcompute-development`，且在本機`.env`檔案中符合`AIO_runtime_auth`。  如果用於發出部署命令的目錄中存在`.env`檔案，則會使用其值，除非其值透過OS層級變數匯出取代，這就是[階段和生產](#stage-and-production)工作區的目標方式。

使用.env變數部署![aio應用程式](./assets/runtime/development__aio.png)

若要部署至專案`.env`檔案中定義的工作區：

1. 在Asset Compute專案的根目錄中開啟命令列
1. 執行命令`aio app deploy`
1. 執行命令`aio app get-url`以取得背景工作URL，以便在AEM as a Cloud Service處理設定檔中參考此自訂Asset Compute背景工作。 如果專案包含多個背景工作，則會列出每個背景工作的獨立URL。

如果本機開發和AEM as a Cloud Service開發環境使用不同的Asset Compute部署，則對AEM as a Cloud Service Dev的部署可採用與[中繼和生產部署](#stage-and-production)相同的方式進行管理。

## 中繼和生產工作區{#stage-and-production}

部署到中繼和生產工作區通常由您選擇的CI/CD系統完成。 Asset Compute專案必須離散地部署到每個Workspace （中繼然後生產）。

設定True環境變數會覆寫`.env`中同名變數的值。

使用匯出變數部署![aio應用程式](./assets/runtime/stage__export-and-aio.png)

部署到中繼和生產環境的一般方法通常由CI/CD系統自動化，即：

1. 確認已安裝[Adobe I/O CLI npm模組及Asset Compute外掛程式](../set-up/development-environment.md#aio)
1. 檢視要從Git部署的Asset Compute專案
1. 使用與目標工作區（「預備」或「生產」）相對應的值設定環境變數
   + 兩個必要變數為`AIO_runtime_namespace`和`AIO_runtime_auth`，是透過Adobe I/O Developer Console的&#x200B;__全部下載__&#x200B;功能在Workspace中為每個工作區取得的。

![Adobe Developer Console - AIO執行階段名稱空間和驗證](./assets/runtime/stage-auth-namespace.png)

從命令列發出匯出指令可以設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset Compute背景工作需要任何其他變數（例如雲端儲存空間），這些變數也應該匯出為環境變數。

1. 設定好要部署的目標工作區的所有環境變數後，請執行部署命令：
   + `aio app deploy`
1. AEM as a Cloud Service處理設定檔參考的工作者URL也可透過以下方式取得：
   + `aio app get-url`。

如果Asset Compute專案版本變更，背景工作URL也會變更以反映新版本，且需要在「處理設定檔」中更新該URL。

## Workspace API布建{#workspace-api-provisioning}

當[在Adobe I/O](../set-up/app-builder.md)中設定App Builder專案以支援本機開發時，已建立新的開發工作區，並已將&#x200B;__Asset Compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__&#x200B;新增到其中。

__Asset Compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API僅明確新增至本機開發所使用的工作區。 與AEM as a Cloud Service環境整合（獨佔）的工作區&#x200B;__不需要__&#x200B;明確新增這些API，因為API自然可供AEM as a Cloud Service使用。
