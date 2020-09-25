---
title: 為資產計算擴充性建立資產計算項目
description: 資產計算應用程式是使用Adobe I/O CLI產生的Node.js專案，符合特定結構，可讓這些專案部署至Adobe I/O Runtime，並與AEM整合為雲端服務。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 0%

---


# 建立資產計算項目

資產計算應用程式是使用Adobe I/O CLI產生的Node.js專案，符合特定結構，可讓這些專案部署至Adobe I/O Runtime，並與AEM整合為雲端服務。 單一資產計算專案可以包含一或多個資產計算工作者，每個工作者都有可從AEM做為雲端服務處理設定檔的離散HTTP端點參考。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)
_產生資產計算專案的點進（無音訊）_


使用 [Adobe I/O CLI Asset Compute外掛程式](../set-up/development-environment.md#aio-cli) ，產生新的空白資產計算專案。

1. 從命令行導航到資料夾以包含項目。
1. 從命令行執行以 `aio app init` 開始交互項目生成CLI。
   + 這可能會衍生網頁瀏覽器，提示驗證Adobe I/O。如果有，請提供您與所需Adobe服務和產品 [相關的Adobe認證](../set-up/accounts-and-services.md)。 如果您無法登入，請依照下列指示，瞭解如何產生專案。
1. __選擇組織__
   + 選取AEM為雲端服務的Adobe組織，Project Firefly已註冊至
1. __選取專案__
   + 找到並選取專案。 這是從 [Firefly專案範本](../set-up/firefly.md) （在本例中）建立的專案標題 `WKND AEM Asset Compute`
1. __選擇工作區__
   + 選擇工作 `Development` 區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？ 選取要包含的元件__
   + 選取 `Actions: Deploy runtime actions`
   + 使用箭頭鍵來選擇並空格來取消選擇／選擇，使用Enter確認選擇
1. __選擇要生成的操作類型__
   + 選取 `Adobe Asset Compute Worker`
   + 使用箭頭鍵進行選擇，使用空格鍵取消選擇／選擇，使用Enter鍵確認選擇
1. __您要如何命名此動作？__
   + 使用預設名稱 `worker`。
   + 如果您的專案包含執行不同資產計算的多個工作者，請在語義上為他們命名

## 審查專案剖析

產生的「資產計算」專案是專用Adobe Project Firefly應用程式的Node.js專案，下列為「資產計算」專案的特有項目：

+ `/actions` 包含子資料夾，每個子資料夾都定義一個「資產計算」工作器。
   + `/actions/<worker-name>/index.js` 定義執行JavaScript以執行此工作器的工作。
      + 資料夾名 `worker` 稱是預設值，只要已在中註冊，就可以是任何項目 `manifest.yml`。
      + 可以根據需要在下定義多個工 `/actions` 作資料夾，但必須在中註冊 `manifest.yml`。
+ `/test/asset-compute` 包含每個工作者的測試套件。 與資料夾類 `/actions` 似， `/test/asset-compute` 可以包含多個子資料夾，每個子資料夾都與它所測試的工作器相對應。
   + `/test/asset-compute/worker`，表示特定工作者的測試套件，包含表示特定測試案例的子資料夾，以及測試輸入、參數和預期輸出。
+ `/build` 包含資產計算測試案例執行的輸出、日誌和對象。
+ `/manifest.yml` 定義項目提供的資產計算員工。 每個工作者實作都必須列舉在此檔案中，才能以雲端服務的形式提供給AEM使用。
+ `/.aio` 包含aio CLI工具使用的配置。 此檔案可通過命令進行 `aio config` 配置。
+ `/.env` 在語法中定義環 `key=value` 境變數，並包含不應共用的機密。 為保護這些機密，不應將此檔案簽入Git，而應透過專案的預設檔案忽略 `.gitignore` 此檔案。
   + 在此檔案中定義的變數可透過在命 [令列上匯出變](../deploy/runtime.md) 數來覆寫。

如需專案結構審核的詳細資訊，請參閱 [Adobe Project Firefly應用程式的剖析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

大部分的開發都發生在開發工作者實 `/actions` 施的資料夾中，以及為自定義資產計算 `/test/asset-compute` 工作者編寫測試時。
