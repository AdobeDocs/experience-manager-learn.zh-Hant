---
title: 設定環境變數以利於Asset Compute的擴充功能
description: 環境變數會保留在.env檔案中以供本機開發使用，並用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# 設定環境變數

![dot env檔案](assets/environment-variables/dot-env-file.png)

在開始開發Asset Compute背景工作之前，請確定專案已設定Adobe I/O和雲端儲存空間資訊。 此資訊儲存在專案的`.env`中，僅用於本機開發，不會儲存在Git中。 `.env`檔案提供一種便利的方法，將索引鍵/值配對公開至本機Asset Compute本機開發環境。 將[部署](../deploy/runtime.md)個Asset Compute背景工作程式至Adobe I/O Runtime時，並未使用`.env`檔案，而是透過環境變數傳入值的子集。 其他自訂引數和密碼也可以儲存在`.env`檔案中，例如協力廠商Web服務的開發認證。

## 參考`private.key`

![私密金鑰](assets/environment-variables/private-key.png)

開啟`.env`檔案、取消註解`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`金鑰，然後提供檔案系統上與Adobe I/O App Builder專案中新增的公開憑證配對的`private.key`的絕對路徑。

+ 如果您的金鑰組是由Adobe I/O產生，則會自動下載為`config.zip`的一部分。
+ 如果您已將公開金鑰提供給Adobe I/O，則您應同時擁有相符的私密金鑰。
+ 如果您沒有這些金鑰組，可以在下方產生新的金鑰組或上傳新的公開金鑰：
  [https://console.adobe.com](https://console.adobe.io) >您的Asset Compute App Builder專案> Workspaces @開發>服務帳戶(JWT)。

請記住，不應將`private.key`檔案簽入Git，因為它包含秘密，而應儲存在專案外部的安全位置。

例如，在macOS上，這可能如下所示：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 設定雲端儲存空間認證

Asset Compute背景工作程式的本機開發需要存取[雲端儲存空間](../set-up/accounts-and-services.md#cloud-storage)。 `.env`檔案中提供用於本機開發的雲端儲存空間認證。

本教學課程偏好使用Azure Blob儲存體，但可以改用Amazon S3及其`.env`檔案中的對應金鑰。

### 使用Azure Blob儲存體

在`.env`檔案中取消註解並填入下列金鑰，然後以Azure入口網站上提供的雲端儲存空間值填入。

![Azure Blob儲存體](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`鍵的值
1. `AZURE_STORAGE_ACCOUNT`鍵的值
1. `AZURE_STORAGE_KEY`鍵的值

例如，其外觀可能類似（值僅供圖解之用）：

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果`.env`檔案如下所示：

![Azure Blob儲存體認證](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob儲存體，請移除或保留這些註解（以`#`為前置詞）。

### 使用Amazon S3雲端儲存空間{#amazon-s3}

如果您正在使用Amazon S3雲端儲存空間，請取消註解並在`.env`檔案中填入下列金鑰。

例如，其外觀可能類似（值僅供圖解之用）：

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 驗證專案設定

設定產生的Asset Compute專案後，請在變更程式碼之前驗證設定，以確保在`.env`檔案中布建支援服務。

若要啟動適用於Asset Compute專案的Asset Compute開發工具：

1. 在Asset Compute專案根目錄中開啟命令列（在VS Code中，這可以直接在IDE中透過「終端機>新終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset Compute開發工具將在您的預設網頁瀏覽器中開啟，網址為&#x200B;__http://localhost:9000__。

   ![aio應用程式執行](assets/environment-variables/aio-app-run.png)

1. 在開發工具初始化時，請觀察命令列輸出和網頁瀏覽器中的錯誤訊息。
1. 若要停止Asset Compute開發工具，請在執行`aio app run`的視窗中點選`Ctrl-C`以終止處理序。

## 疑難排解

+ [開發工具無法啟動，因為遺失private.key](../troubleshooting.md#missing-private-key)
