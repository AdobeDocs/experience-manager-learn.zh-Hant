---
title: 設定環境變數以擴充Asset compute
description: 環境變數會保留在。env檔案中以進行本端開發，並用來提供本端開發所需的Adobe I/O憑證和雲端儲存憑證。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: 整合、開發
role: 開發人員
level: 中級，經驗豐富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# 配置環境變數

![點環境檔案](assets/environment-variables/dot-env-file.png)

在開始開發Asset compute工作人員之前，請確保項目已配置Adobe I/O和雲儲存資訊。 此資訊會儲存在專案的`.env`中，該項目僅用於本機開發，而不儲存在Git中。 `.env`檔案提供了將鍵／值配對公開至本機Asset compute本機開發環境的便利方式。 當[部署](../deploy/runtime.md)Asset compute工作者到Adobe I/O Runtime時，不使用`.env`檔案，而是通過環境變數傳入值的子集。 其他自訂參數和機密也可以儲存在`.env`檔案中，例如協力廠商Web服務的開發認證。

## 參考`private.key`

![私鑰](assets/environment-variables/private-key.png)

開啟`.env`檔案，取消註解`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`金鑰，並提供檔案系統上與`private.key`配對的絕對路徑，此路徑與新增至Adobe I/OFireFly專案的公用憑證。

+ 如果您的密鑰對是由Adobe I/O生成的，則會作為`config.zip`的一部分自動下載。
+ 如果您提供公開金鑰給Adobe I/O，則您也應擁有相符的私密金鑰。
+ 如果您沒有這些密鑰對，則可以生成新的密鑰對，或在以下位置的底部上載新的公鑰：
   [https://console.adobe.com](https://console.adobe.io) >您的Asset computeFirefly專案>開發>服務帳戶(JWT)中的工作區。

請記住，`private.key`檔案不應簽入Git，因為它包含機密，而應儲存在專案外的安全位置。

例如，在macOS上，這看起來可能是：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 設定雲端儲存憑證

本地開發Asset compute員工需要訪問[雲儲存](../set-up/accounts-and-services.md#cloud-storage)。 用於本機開發的雲端儲存憑證會提供在`.env`檔案中。

本教學課程偏好使用Azure Blob儲存，但AmazonS3和`.env`檔案中的對應金鑰則可改用。

### 使用Azure Blob儲存空間

取消注釋並在`.env`檔案中填入下列索引鍵，然後以Azure Portal上所提供之雲端儲存空間的值填入這些索引鍵。

![Azure Blob儲存空間](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`鍵的值
1. `AZURE_STORAGE_ACCOUNT`鍵的值
1. `AZURE_STORAGE_KEY`鍵的值

例如，這看起來可能類似（僅限插圖的值）:

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

生成的`.env`檔案如下所示：

![Azure Blob儲存憑據](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob儲存，請刪除或保留這些注釋（使用`#`前置詞）。

### 使用AmazonS3雲端儲存空間{#amazon-s3}

如果您使用AmazonS3雲端儲存空間取消註解，並在`.env`檔案中填入下列金鑰。

例如，這看起來可能類似（僅限插圖的值）:

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 驗證項目配置

在配置生成的Asset compute項目後，請先驗證配置，然後再進行代碼更改，以確保在`.env`檔案中提供支援服務。

要啟動Asset compute項目的Asset compute開發工具，請執行以下操作：

1. 在Asset compute項目根目錄中開啟命令行（在VS代碼中，此命令可直接在IDE中通過「終端機>新終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset compute開發工具將在您的預設Web瀏覽器中開啟，位於&#x200B;__http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 當開發工具初始化時，請觀看命令列輸出和Web瀏覽器中的錯誤訊息。
1. 若要停止Asset compute開發工具，請在執行`aio app run`的視窗中點選`Ctrl-C`以終止程式。

## 疑難排解

+ [由於缺少private.key，開發工具無法啟動](../troubleshooting.md#missing-private-key)
