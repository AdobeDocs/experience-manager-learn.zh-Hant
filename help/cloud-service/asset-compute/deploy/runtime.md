---
title: 將資產計算工作者部署至Adobe I/O Runtime，以便與AEM搭配使用為雲端服務
description: '資產計算專案及其所包含的工作者必須部署至Adobe I/O Runtime,AEM才能將其當做雲端服務使用。 '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# 部署至Adobe I/O Runtime

資產計算專案及其所包含的工作者必須透過Adobe I/O CLI部署至Adobe I/O Runtime,AEM才能將之用作雲端服務。

部署至Adobe I/O Runtime以供AEM做為雲端服務作者服務使用時，只需兩個環境變數：

+ `AIO_runtime_namespace` 指出Adobe Project Firefly Workspace要部署至
+ `AIO_runtime_auth` 是Adobe Project Firefly工作區的驗證認證

`.env`檔案中定義的其他標準變數，會由AEM在叫用「資產計算工作者」時隱式提供為雲端服務。

## 開發工作區

由於此專案是使用`Development`工作區使用`aio app init`產生，因此`AIO_runtime_namespace`會自動設為`81368-wkndaemassetcompute-development`，並在本機`.env`檔案中使用相符的`AIO_runtime_auth`。  如果`.env`檔案位於用於發出deploy命令的目錄中，則使用其值，除非它們通過作業系統級別變數導出取代，這是[stage和production](#stage-and-production)工作區的定位方式。

![使用env變數部署AIO應用程式](./assets/runtime/development__aio.png)

要部署到在項目`.env`檔案中定義的工作區，請執行以下操作：

1. 在「資產計算」項目的根目錄中開啟命令行
1. 執行命令`aio app deploy`
1. 執行命令`aio app get-url`以取得要在AEM中當做雲端服務處理設定檔使用的工作器URL，以參考此自訂的資產計算工作器。 如果項目包含多個工作者，則會列出每個工作者的離散URL。

如果本機開發與AEM(Cloud Service Development)環境使用個別的「資產計算」部署，則AEM(Cloud Service Dev)的部署方式可與[Stage和Production部署](#stage-and-production)相同。

## 舞台和生產工作區{#stage-and-production}

部署至舞台和生產工作區通常由您選擇的CI/CD系統完成。 資產計算項目必須單獨部署到每個工作區（舞台和生產）。

設定true環境變數會覆寫`.env`中相同名稱變數的值。

![使用匯出變數來部署一體應用程式](./assets/runtime/stage__export-and-aio.png)

部署到舞台和生產環境的一般方法（通常由CI/CD系統自動執行）是：

1. 確保已安裝[Adobe I/O CLI npm模組和Asset Compute插件](../set-up/development-environment.md#aio)
1. 查看要從Git部署的Asset Compute項目
1. 使用與目標工作區（「舞台」或「生產」）對應的值設定環境變數
   + 這兩個必要的變數是`AIO_runtime_namespace`和`AIO_runtime_auth`，並透過工作區的&#x200B;__下載全部__&#x200B;功能，依Adobe I/O Developer Console中的每個工作區取得。

![Adobe Developer Console - AIO Runtime命名空間和驗證](./assets/runtime/stage-auth-namespace.png)

可通過從命令行發出導出命令來設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的資產計算工作者需要任何其他變數（例如雲端儲存空間），這些變數也應匯出為環境變數。

1. 將目標工作區的所有環境變數設定為部署後，請執行deploy命令：
   + `aio app deploy`
1. AEM參照的「Cloud Service Processing Profile」（雲端服務處理設定檔）也可透過下列方式取得：
   + `aio app get-url`。

如果「資產計算」項目版本更改了工作URL，也更改為反映新版本，則需要在「處理配置檔案」中更新URL。

## 工作區API布建{#workspace-api-provisioning}

當[在Adobe I/O](../set-up/firefly.md)中設定Adobe Project Firefly專案以支援本機開發時，會建立新的開發工作區，並新增&#x200B;__資產計算、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__。

__資產計算、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API只顯式添加到用於本地開發的工作區。 將AEM整合（僅限）為雲端服務環境的工作區，__not__&#x200B;需要明確新增的這些API，因為API會自然提供給AEM做為雲端服務。
