---
title: 建立Asset compute項目以擴展Asset compute
description: asset compute項目是使用Adobe I/OCLI生成的Node.js項目，它遵循一定的結構，允許將它們部署到Adobe I/O Runtime並與as a Cloud Service集AEM成。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 136049776140746c61d42ad1496df15a2d226e3a
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---

# 建立Asset compute項目

asset compute項目是使用Adobe I/OCLI生成的Node.js項目，它遵循一定的結構，允許將它們部署到Adobe I/O Runtime並與as a Cloud Service集AEM成。 單個Asset compute項目可以包含一個或多個Asset compute工作程式，每個工作程式都具有可從as a Cloud Service處理配置檔案中引AEM用的離散HTTP端點。

## 生成項目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_按一下直接生成Asset compute項目（無音頻）_

使用 [Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli) 生成新的空Asset compute項目。

1. 在命令行中，導航到要包含項目的資料夾。
1. 從命令行執行 `aio app init` 開始交互項目生成CLI。
   + 此命令可生成Web瀏覽器提示進行身份驗證以Adobe I/O。如果確實如此，請提供與 [所需的Adobe服務和產品](../set-up/accounts-and-services.md)。 如果無法登錄，請遵循 [有關如何生成項目的這些說明](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __選擇組織__
   + 選擇具有as a Cloud Service的「Project AEM Firefly」註冊的Adobe組織
1. __選取專案__
   + 查找並選擇項目。 這是 [項目標題](../set-up/firefly.md) 從Firefly項目模板建立，在本例中 `WKND AEM Asset Compute`
1. __選擇工作區__
   + 選擇 `Development` 工作區
1. __您要為此項目啟用哪些Adobe I/O應用功能？ 選擇要包括的元件__
   + 選取 `Actions: Deploy runtime actions`
   + 使用箭頭鍵選擇和空格鍵取消選擇/選擇，使用Enter鍵確認選擇
1. __選擇要生成的操作類型__
   + 選取 `DX Asset Compute Worker v1`
   + 使用箭頭鍵進行選擇，使用空格取消選擇/選擇，使用Enter確認選擇
1. __您要如何命名此操作？__
   + 使用預設名稱 `worker`。
   + 如果項目包含多個執行不同資產計算的工作人員，請在語義上命名這些工作人員

## 生成console.json

開發人員工具需要名為 `console.json` 包含連接到Adobe I/O所需的憑據。此檔案從Adobe I/O控制台下載。

1. 開啟Asset compute工人 [Adobe I/O](https://console.adobe.io) 項目
1. 選擇項目工作區以下載 `console.json` 憑據，在此情況下，選擇 `Development`
1. 轉到Adobe I/O項目的根並點擊 __全部下載__ 在右上角。
1. 檔案作為 `.json` 檔案以項目和工作區為前置詞，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 將檔案更名為 `config.json` 並移到Asset compute工作項目的根中。 這是本教程中的方法。
   + 將其移入任意資料夾中，並引用您的 `.env` 帶有配置項的檔案 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`。 檔案路徑可以是絕對路徑，也可以是相對於項目根路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 檔案包含憑據。 如果將檔案儲存在項目中，請確保將其添加到 `.gitignore` 檔案以阻止共用。 這同樣適用於 `.env` 檔案 — 這些憑據檔案不能共用或儲存在Git中。

## 審查項目的結構

生成的Asset compute項目是Node.js項目，用作專門的Adobe項目Firefly項目。 以下結構要素對Asset compute項目是特有的：

+ `/actions` 包含子資料夾，每個子資料夾定義一個Asset compute工作程式。
   + `/actions/<worker-name>/index.js` 定義用於執行此工作程式的JavaScript。
      + 資料夾名稱 `worker` 是預設值，並且可以是任何，只要它在 `manifest.yml`。
      + 可以在下定義多個工作資料夾 `/actions` 但是，必須在 `manifest.yml`。
+ `/test/asset-compute` 包含每個工作人員的test套件。 與 `/actions` 資料夾， `/test/asset-compute` 可以包含多個子資料夾，每個子資料夾都與ittest的工作人員相對應。
   + `/test/asset-compute/worker`，表示特定工作人員的test套件，包含表示特定test案例的子資料夾以及test輸入、參數和預期輸出。
+ `/build` 包含Asset computetest執行的輸出、日誌和對象。
+ `/manifest.yml` 定義項目提供的Asset compute工作程式。 必須在此檔案中枚舉每個工作程式實現，以使它們可供AEMas a Cloud Service使用。
+ `/console.json` 定義Adobe I/O配置
   + 可以使用 `aio app use` 的子菜單。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可以使用 `aio app use` 的子菜單。
+ `/.env` 定義環境變數 `key=value` 語法，包含不應共用的機密。 為保護這些機密，不應將此檔案簽入Git，並通過項目的預設值忽略該檔案 `.gitignore` 的子菜單。
   + 可以使用 `aio app use` 的子菜單。
   + 此檔案中定義的變數可由 [導出變數](../deploy/runtime.md) 命令行上。

有關項目結構審閱的詳細資訊，請查看 [Adobe項目螢火蟲項目剖析](https://www.adobe.io/project-firefly/docs/guides/)。

大部分開發工作於 `/actions` 資料夾開發工作進程實施，以及 `/test/asset-compute` 為定制Asset compute工作人員編寫test。

## asset computeGitHub項目

GitHub上提供的最終Asset compute項目位於：

+ [輔助線 — wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含項目的最終狀態，它完全填充了工作程式和test案例，但不包含任何憑據，即， `.env`。 `console.json` 或 `.aio`。_
