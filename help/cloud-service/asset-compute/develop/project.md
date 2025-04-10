---
title: 建立Asset compute專案以增加Asset compute擴充性
description: asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEM as a Cloud Service整合。
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# 建立Asset compute專案

asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEM as a Cloud Service整合。 單一Asset compute專案可包含一或多個Asset compute背景工作，每個背景工作具有獨立的HTTP端點，可從AEM as a Cloud Service處理設定檔參照。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_產生Asset compute專案的點進（無音訊）_

使用[Adobe I/OCLIAsset compute外掛程式](../set-up/development-environment.md#aio-cli)來產生新的空的Asset compute專案。

1. 從命令列，瀏覽至資料夾以包含專案。
1. 從命令列，執行`aio app init`以開始互動式專案產生CLI。
   + 這個命令可能會產生一個Web瀏覽器，提示您Adobe I/O驗證。如果是，請提供您與[必要的Adobe服務和產品](../set-up/accounts-and-services.md)相關聯的Adobe認證。 如果您無法登入，請依照[這些說明產生專案](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __選取組織__
   + 選取擁有AEM as a Cloud Service、App Builder註冊的Adobe組織
1. __選取專案__
   + 找到並選取專案。 這是從App Builder專案範本建立的[專案標題](../set-up/app-builder.md)，在此案例中為`WKND AEM Asset Compute`
1. __選取Workspace__
   + 選取`Development`工作區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？ 選取要包含的元件__
   + 選取`Actions: Deploy runtime actions`
   + 使用箭頭鍵來選取，空格鍵來取消選取/選取，Enter鍵來確認選取
1. __選取要產生的動作型別__
   + 選取`DX Asset Compute Worker v1`
   + 使用箭頭鍵來選取，空格鍵來取消選取/選取，Enter鍵來確認選取
1. __您要如何命名此動作？__
   + 使用預設名稱`worker`。
   + 如果您的專案包含多個執行不同資產計算的背景工作，請在語義上將其命名

## 產生console.json

開發人員工具需要名為`console.json`的檔案，該檔案包含連線至Adobe I/O所需的認證。此檔案是從Adobe I/O主控台下載。

1. 開啟Asset compute工作者的[Adobe I/O](https://console.adobe.io)專案
1. 選取要下載`console.json`認證的專案工作區，在此案例中選取`Development`
1. 移至Adobe I/O專案的根目錄，然後點選右上角的&#x200B;__全部下載__。
1. 下載的檔案會以專案和工作區為前置詞的`.json`檔案，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 將檔案重新命名為`console.json`，並將其移動到Asset compute背景工作專案的根目錄中。 這就是本教學課程中的方法。
   + 將其移至任意資料夾，並從包含組態專案`ASSET_COMPUTE_INTEGRATION_FILE_PATH`的`.env`檔案中參照該資料夾。 檔案路徑可以是絕對路徑，也可以是相對於專案根目錄的路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     或
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> 注意
> 檔案包含認證。 如果您將此檔案儲存在專案中，請務必將其新增到您的`.gitignore`檔案中，以防止共用。 同樣適用於`.env`檔案 — 這些認證檔案不得共用或儲存在Git中。

## 在GitHub上Asset compute專案

最終Asset compute專案可在GitHub上取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含專案的最終狀態，已完整填入Worker和測試案例，但不包含任何認證，即`.env`、`console.json`或`.aio`。_
