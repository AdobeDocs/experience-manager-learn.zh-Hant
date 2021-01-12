---
title: 為資產計算擴充性建立資產計算項目
description: 「資產計算」專案是使用Adobe I/O CLI產生的Node.js專案，符合特定結構，可讓這些專案部署至Adobe I/O Runtime，並與AEM整合為雲端服務。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 676d4bfceaaec3ae8d4feb9f66294ec04e1ecd2b
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# 建立資產計算項目

「資產計算」專案是使用Adobe I/O CLI產生的Node.js專案，符合特定結構，可讓這些專案部署至Adobe I/O Runtime，並與AEM整合為雲端服務。 單一資產計算專案可以包含一或多個資產計算工作者，每個工作者都有可從AEM做為雲端服務處理設定檔的離散HTTP端點參考。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_產生資產計算專案的點進（無音訊）_

使用[Adobe I/O CLI Asset Compute plugin](../set-up/development-environment.md#aio-cli)產生新的空資產計算專案。

1. 從命令行導航到資料夾以包含項目。
1. 在命令行中，執行`aio app init`以開始互動式項目生成CLI。
   + 這可能會衍生網頁瀏覽器，提示驗證Adobe I/O。如果有，請提供您與[必要Adobe服務和產品](../set-up/accounts-and-services.md)相關的Adobe認證。 如果您無法登入，請依照[這些指示，瞭解如何產生專案](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __選擇組織__
   + 選取AEM為雲端服務的Adobe組織，Project Firefly已註冊至
1. __選取專案__
   + 找到並選取專案。 這是從Firefly專案範本建立的[專案標題](../set-up/firefly.md)，在本例中為`WKND AEM Asset Compute`
1. __選擇工作區__
   + 選擇`Development`工作區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？選擇要包含的元件__
   + 選取 `Actions: Deploy runtime actions`
   + 使用箭頭鍵來選擇並空格來取消選擇／選擇，使用Enter確認選擇
1. __選擇要生成的操作類型__
   + 選取 `Adobe Asset Compute Worker`
   + 使用箭頭鍵進行選擇，使用空格鍵取消選擇／選擇，使用Enter鍵確認選擇
1. __您要如何命名此動作？__
   + 使用預設名稱`worker`。
   + 如果您的專案包含執行不同資產計算的多個工作者，請在語義上為他們命名

## 產生console.json

從新建立的「資產計算」項目的根目錄中，運行以下命令以生成`console.json`。

```
$ aio app use
```

驗證當前工作區詳細資訊是否正確，請按`Y`或輸入以生成`console.json`。 如果檢測到`.env`和`.aio`已存在，請點選`x`以跳過其建立。

如果建立新的，或覆寫`.env` ，請將任何缺少的鍵／值重新添加到新的`.env` :

```
## please provide the following environment variables for the Asset Compute devtool. You can use AWS or Azure, not both:
#ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=
#S3_BUCKET=
#AWS_ACCESS_KEY_ID=
#AWS_SECRET_ACCESS_KEY=
#AWS_REGION=
#AZURE_STORAGE_ACCOUNT=
#AZURE_STORAGE_KEY=
#AZURE_STORAGE_CONTAINER_NAME=
```

## 審查專案剖析

產生的「資產計算」專案是專用Adobe Project Firefly專案的Node.js專案，下列項目為資產計算專案的特有項目：

+ `/actions` 包含子資料夾，每個子資料夾都定義一個「資產計算」工作器。
   + `/actions/<worker-name>/index.js` 定義執行JavaScript以執行此工作器的工作。
      + 資料夾名稱`worker`是預設的，可以是任何內容，只要它已在`manifest.yml`中註冊。
      + 可根據需要在`/actions`下定義多個工作資料夾，但必須在`manifest.yml`中註冊。
+ `/test/asset-compute` 包含每個工作者的測試套件。與`/actions`資料夾類似，`/test/asset-compute`可以包含多個子資料夾，每個子資料夾都與它所測試的工作器相對應。
   + `/test/asset-compute/worker`，表示特定工作者的測試套件，包含表示特定測試案例的子資料夾，以及測試輸入、參數和預期輸出。
+ `/build` 包含資產計算測試案例執行的輸出、日誌和對象。
+ `/manifest.yml` 定義項目提供的資產計算員工。每個工作者實作都必須列舉在此檔案中，才能以雲端服務的形式提供給AEM使用。
+ `/console.json` 定義Adobe I/O組態
   + 可使用`aio app use`命令生成／更新此檔案。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可使用`aio app use`命令生成／更新此檔案。
+ `/.env` 定義語法中的環 `key=value` 境變數，並包含不應共用的機密。可以生成此檔案或要保護這些機密，不應將此檔案簽入Git中，並通過項目的預設`.gitignore`檔案忽略。
   + 可使用`aio app use`命令生成／更新此檔案。
   + 在此檔案中定義的變數可由命令行上的[導出變數](../deploy/runtime.md)覆蓋。

如需專案結構審查的詳細資訊，請參閱[ Adobe Project Firefly專案剖析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

開發的大部分發生在`/actions`資料夾中開發工作者實施中，在`/test/asset-compute`中為自定義資產計算工作者編寫測試中。

## Github上的資產計算項目

Github上提供最終資產計算專案，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是專案的最終狀態，已填入工作者和測試案例，但不包含任何憑證，例如。`.env`或 `console.json` 者 `.aio`。_

