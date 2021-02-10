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
source-git-commit: 23c91551673197cebeb517089e5ab6591f084846
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 1%

---


# 建立資產計算項目

「資產計算」專案是使用Adobe I/O CLI產生的Node.js專案，符合特定結構，可讓這些專案部署至Adobe I/O Runtime，並與AEM整合為雲端服務。 單一資產計算專案可以包含一或多個資產計算工作者，每個工作者都有可從AEM做為雲端服務處理設定檔的離散HTTP端點參考。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_產生資產計算專案的點進（無音訊）_

使用[Adobe I/O CLI Asset Compute外掛程式](../set-up/development-environment.md#aio-cli)產生新的空白資產計算專案。

1. 從命令行中，導航到要包含項目的資料夾。
1. 在命令行中，執行`aio app init`以開始互動式項目生成CLI。
   + 此命令可能會衍生網頁瀏覽器，提示驗證Adobe I/O。如果有，請提供您與[必要Adobe服務和產品](../set-up/accounts-and-services.md)相關的Adobe認證。 如果您無法登入，請依照[這些指示，瞭解如何產生專案](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __選擇組織__
   + 選取AEM為雲端服務、Project Firefly已註冊的Adobe組織
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

開發人員工具需要一個名為`console.json`的檔案，其中包含連線至Adobe I/O的必要憑證。此檔案是從Adobe I/O主控台下載。

1. 開啟資產計算工作者的[Adobe I/O](https://console.adobe.io)項目
1. 選擇項目工作區以下載`console.json`憑據，在這種情況下，請選擇`Development`
1. 前往Adobe I/O專案的根目錄，然後點選右上角的&#x200B;__下載全部__。
1. 檔案會下載為`.json`檔案，並加上專案和工作區的前置詞，例如：`wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 將檔案更名為`config.json` ，並將其移到資產計算員工項目的根目錄中。 這是本教學課程中的方法。
   + 將其移入任意資料夾，並從具有配置條目`ASSET_COMPUTE_INTEGRATION_FILE_PATH`的`.env`檔案引用該資料夾。 檔案路徑可以是絕對路徑，也可以是相對於專案的根路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 檔案包含憑據。 如果您將檔案儲存在專案中，請務必將它新增至`.gitignore`檔案，以免共用。 同樣的情況也適用於`.env`檔案——這些憑據檔案不能共用或儲存在Git中。

## 審查專案剖析

產生的「資產計算」專案是Node.js專案，可當成專業的Adobe Project Firefly專案使用。 以下結構元素是資產計算項目的特有元素：

+ `/actions` 包含子資料夾，每個子資料夾都定義了Asset Compute工作器。
   + `/actions/<worker-name>/index.js` 定義用於執行此工作器的JavaScript。
      + 資料夾名稱`worker`是預設的，可以是任何內容，只要它已在`manifest.yml`中註冊。
      + 可根據需要在`/actions`下定義多個工作資料夾，但必須在`manifest.yml`中註冊。
+ `/test/asset-compute` 包含每個工作者的測試套件。與`/actions`資料夾類似，`/test/asset-compute`可以包含多個子資料夾，每個子資料夾都與它所測試的工作器相對應。
   + `/test/asset-compute/worker`，代表特定工作者的測試套件，包含代表特定測試案例的子檔案夾，以及測試輸入、參數和預期輸出。
+ `/build` 包含資產計算測試案例執行的輸出、日誌和對象。
+ `/manifest.yml` 定義項目提供的資產計算員工。每個工作者實作都必須列舉在此檔案中，才能以雲端服務的形式提供給AEM使用。
+ `/console.json` 定義Adobe I/O組態
   + 可使用`aio app use`命令生成／更新此檔案。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可使用`aio app use`命令生成／更新此檔案。
+ `/.env` 定義語法中的環 `key=value` 境變數，並包含不應共用的機密。為保護這些機密，不應將此檔案簽入Git，而應透過專案的預設`.gitignore`檔案忽略。
   + 可使用`aio app use`命令生成／更新此檔案。
   + 在此檔案中定義的變數可由命令行上的[導出變數](../deploy/runtime.md)覆蓋。

如需專案結構審查的詳細資訊，請參閱[Adobe Project Firefly專案剖析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

開發的大部分發生在`/actions`資料夾中開發工作者實施中，在`/test/asset-compute`中為自定義資產計算工作者編寫測試中。

## GitHub上的資產計算項目

GitHub上提供最終資產計算項目，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含項目的最終狀態，已完全填入工作者和測試案例，但不包含任何憑證，即， `.env`或 `console.json` 者 `.aio`。_

