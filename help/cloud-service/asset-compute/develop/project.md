---
title: 建立Asset compute專案以擴充Asset compute
description: asset compute專案是使用Adobe I/OCLI產生的Node.js專案，其符合特定結構，可部署至Adobe I/O Runtime並與AEM as a Cloud Service整合。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# 建立Asset compute專案

asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEM as a Cloud Service整合。 單一Asset compute專案可包含一或多個Asset compute背景工作，每個背景工作都有可從AEMas a Cloud Service處理設定檔參考的獨立HTTP端點。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_點進以產生Asset compute專案（無音訊）_

使用 [Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli) 來產生新的空白Asset compute專案。

1. 從命令列導覽至資料夾以包含專案。
1. 從命令行執行 `aio app init` 開始交互項目生成CLI。
   + 此命令可產生Web瀏覽器提示驗證Adobe I/O。若有，請提供與 [必要的Adobe服務與產品](../set-up/accounts-and-services.md). 如果您無法登入，請遵循 [如何產生專案的這些指示](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __選擇組織__
   + 選取具有AEMas a Cloud Service、App Builder的Adobe組織
1. __選取專案__
   + 找到並選取專案。 這是 [專案標題](../set-up/app-builder.md) 從App Builder專案範本建立，在此案例中為 `WKND AEM Asset Compute`
1. __選取工作區__
   + 選取 `Development` 工作區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？ 選擇要包括的元件__
   + 選取 `Actions: Deploy runtime actions`
   + 使用箭頭鍵來選取和空格來取消選取/選取，並使用Enter確認選取
1. __選取要產生的動作類型__
   + 選取 `DX Asset Compute Worker v1`
   + 使用箭頭鍵進行選擇，使用空格鍵取消選擇/選擇，使用Enter確認選擇
1. __您要如何命名此動作？__
   + 使用預設名稱 `worker`.
   + 如果您的專案包含多個執行不同資產計算的背景工作，請在語義上為它們命名

## 產生console.json

開發人員工具需要名為 `console.json` 包含連線至Adobe I/O的必要憑證。此檔案會從Adobe I/O主控台下載。

1. 開啟Asset compute員工的 [Adobe I/O](https://console.adobe.io) 專案
1. 選取要下載的專案工作區 `console.json` 的憑據，在此情況下，請選擇 `Development`
1. 前往Adobe I/O專案的根目錄，然後點選 __全部下載__ 在右上角。
1. 檔案下載為 `.json` 以專案和工作區為首碼的檔案，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以執行下列兩個動作中的一個
   + 將檔案重新命名為 `console.json` 並將其移至Asset compute背景工作專案的根目錄中。 這是本教學課程中的方法。
   + 將其移入任意資料夾，並從 `.env` 帶有配置項的檔案 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. 檔案路徑可以是絕對路徑，或相對於專案的根路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 檔案包含憑據。 如果將檔案儲存在專案中，請務必將其新增至 `.gitignore` 檔案來防止共用。 這同樣適用於 `.env` 檔案 — 這些憑證檔案不得共用或儲存在Git中。

## asset computeGitHub專案

最終Asset compute專案可在GitHub取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含專案的最終狀態，已完全填入背景工作和測試案例，但不包含任何憑證，即 `.env`, `console.json` 或 `.aio`._
