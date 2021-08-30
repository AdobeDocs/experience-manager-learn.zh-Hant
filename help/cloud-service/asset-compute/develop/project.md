---
title: 建立Asset compute專案以擴充Asset compute
description: asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，才能部署至Adobe I/O Runtime並與AEM作為Cloud Service整合。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
source-git-commit: ac93d6ba636e64ba6d8bbdb0840810b8f47a25c8
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---


# 建立Asset compute專案

asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEM作為Cloud Service整合。 單一Asset compute專案可包含一或多個Asset compute背景工作，每個背景工作都有可從AEM作為Cloud Service處理設定檔參考的獨立HTTP端點。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_點進以產生Asset compute專案（無音訊）_

使用[Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli)生成新的空Asset compute項目。

1. 從命令列導覽至資料夾以包含專案。
1. 從命令行中，執行`aio app init`以開始交互項目生成CLI。
   + 此命令可產生Web瀏覽器提示驗證Adobe I/O。若有，請提供與[必要Adobe服務和產品](../set-up/accounts-and-services.md)相關聯的Adobe憑證。 如果您無法登入，請依照[下列指示，了解如何產生專案](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __選擇組織__
   + 選取以AEM作為Cloud Service的「Adobe組織」，Project Firefly會以註冊
1. __選取專案__
   + 找到並選取專案。 這是從Firefly專案範本建立的[專案標題](../set-up/firefly.md)，在此例中為`WKND AEM Asset Compute`
1. __選取工作區__
   + 選擇`Development`工作區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？選擇要包括的元件__
   + 選取 `Actions: Deploy runtime actions`
   + 使用箭頭鍵來選取和空格來取消選取/選取，並使用Enter確認選取
1. __選取要產生的動作類型__
   + 選取 `Adobe Asset Compute Worker`
   + 使用箭頭鍵進行選擇，使用空格鍵取消選擇/選擇，使用Enter確認選擇
1. __您要如何命名此動作？__
   + 使用預設名稱`worker`。
   + 如果您的專案包含多個執行不同資產計算的背景工作，請在語義上為它們命名

## 產生console.json

開發人員工具需要名為`console.json`的檔案，該檔案包含連接到Adobe I/O所需的憑據。此檔案會從Adobe I/O主控台下載。

1. 開啟Asset compute工作人員的[Adobe I/O](https://console.adobe.io)項目
1. 選擇項目工作區以下載`console.json`憑據，在此情況下，請選擇`Development`
1. 前往Adobe I/O專案的根目錄，點選右上角的&#x200B;__「下載全部」__。
1. 下載的檔案為`.json`檔案，首碼為專案和工作區，例如：`wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 將檔案更名為`config.json`，並將其移至Asset compute背景工作專案的根目錄中。 這是本教學課程中的方法。
   + 將其移入任意資料夾，並從配置項`ASSET_COMPUTE_INTEGRATION_FILE_PATH`的`.env`檔案中引用該資料夾。 檔案路徑可以是絕對路徑，或相對於專案的根路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 檔案包含憑據。 如果將檔案儲存在專案中，請務必將其新增至`.gitignore`檔案，以防止共用。 `.env`檔案亦同 — 這些憑據檔案不得共用或儲存在Git中。

## 檢視專案的結構

產生的Asset compute專案是Node.js專案，可作為專用的Adobe專案Firefly專案使用。 下列結構元素對Asset compute專案而言屬特殊：

+ `/actions` 包含子資料夾，每個子資料夾定義一個Asset compute工作。
   + `/actions/<worker-name>/index.js` 定義用於執行此工作人員的JavaScript。
      + 資料夾名稱`worker`為預設值，只要它已在`manifest.yml`中註冊，就可以是任何值。
      + 根據需要，可在`/actions`下定義多個工作資料夾，但必須在`manifest.yml`中註冊。
+ `/test/asset-compute` 包含每個工作人員的測試套裝。與`/actions`資料夾類似， `/test/asset-compute`可以包含多個子資料夾，每個子資料夾都與它所測試的工作器相對應。
   + `/test/asset-compute/worker`，代表特定工作人員的測試套裝，包含代表特定測試案例的子資料夾，以及測試輸入、參數和預期輸出。
+ `/build` 包含Asset compute測試案例執行的輸出、日誌和對象。
+ `/manifest.yml` 定義專案提供的Asset compute背景工作。必須在此檔案中列舉每個背景工作實施，才能將其作為Cloud Service供AEM使用。
+ `/console.json` 定義Adobe I/O配置
   + 可使用`aio app use`命令生成/更新此檔案。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可使用`aio app use`命令生成/更新此檔案。
+ `/.env` 會以語法定義環 `key=value` 境變數，並包含不應共用的機密。為保護這些機密，不應將此檔案簽入Git，並透過專案的預設`.gitignore`檔案忽略。
   + 可使用`aio app use`命令生成/更新此檔案。
   + 在此檔案中定義的變數可由命令列上的[匯出變數](../deploy/runtime.md)覆寫。

如需專案結構檢閱的詳細資訊，請檢閱[AdobeProject Firefly專案的解剖](https://www.adobe.io/project-firefly/docs/guides/)。

開發的大部分發生在開發工作實施的`/actions`資料夾中，以及為自訂Asset compute工作進行寫入測試的`/test/asset-compute`中。

## asset computeGitHub專案

最終Asset compute專案可在GitHub取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含專案的最終狀態，已完全填入背景工作和測試案例，但不包含任何憑證，即 `.env`或 `console.json`  `.aio`。_

