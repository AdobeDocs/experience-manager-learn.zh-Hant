---
title: 設定環境變數以提升Asset compute擴充性
description: 環境變數會保留在.env檔案中供本機開發使用，並用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---


# 設定環境變數

![圓點環境檔案](assets/environment-variables/dot-env-file.png)

開始開發Asset compute背景工作之前，請確定專案已設定Adobe I/O和雲端儲存資訊。 此資訊會儲存在專案的`.env`中，而僅用於本機開發，不會儲存在Git中。 `.env`檔案提供了一種將鍵/值對公開到本地Asset compute本地開發環境的方便方法。 當[將](../deploy/runtime.md)Asset compute工作程式部署至Adobe I/O Runtime時，不會使用`.env`檔案，而是會透過環境變數傳入值的子集。 其他自訂參數和機密也可儲存在`.env`檔案中，例如第三方Web服務的開發憑證。

## 參考`private.key`

![私密金鑰](assets/environment-variables/private-key.png)

開啟`.env`檔案，取消對`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`鍵的注釋，並將檔案系統上的絕對路徑提供給`private.key`，該路徑與添加到您Adobe I/OFireFly項目的公共證書配對。

+ 如果您的金鑰組是由Adobe I/O產生，則會隨著`config.zip`自動下載。
+ 如果您提供公開金鑰來Adobe I/O，則您也應擁有相符的私密金鑰。
+ 如果您沒有這些索引鍵組，您可以在下列位置產生新的索引鍵組或上傳新的公開索引鍵：
   [https://console.adobe.com](https://console.adobe.io)  >您的Asset computeFirefly專案>在開發>服務帳戶(JWT)中的工作區。

請記住，`private.key`檔案不應簽入Git，因為它包含機密，而應儲存在專案外的安全位置。

例如，在macOS上，這看起來可能像：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 配置雲儲存憑據

本機開發Asset compute背景工作需要存取[雲端儲存空間](../set-up/accounts-and-services.md#cloud-storage)。 `.env`檔案中提供了用於本地開發的雲儲存憑據。

本教學課程偏好使用Azure Blob儲存，不過Amazon S3，以及`.env`檔案中對應的金鑰可改用。

### 使用Azure Blob儲存

取消註解並填入`.env`檔案中的下列索引鍵，然後填入已布建雲端儲存空間（在Azure入口網站上找到）的值。

![Azure Blob儲存](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`鍵的值
1. `AZURE_STORAGE_ACCOUNT`鍵的值
1. `AZURE_STORAGE_KEY`鍵的值

例如，這看起來可能像（僅限插圖的值）:

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果的`.env`檔案如下所示：

![Azure Blob儲存憑據](assets/environment-variables/cloud-storage-credentials.png)

如果您沒有使用Microsoft Azure Blob儲存，請移除或保留這些備注（方法是加上`#`前置詞）。

### 使用Amazon S3雲端儲存空間{#amazon-s3}

如果您使用Amazon S3雲端儲存空間，請取消註解，並在`.env`檔案中填入下列索引鍵。

例如，這看起來可能像（僅限插圖的值）:

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 驗證專案設定

在配置生成的Asset compute項目後，在進行代碼更改之前驗證配置，以確保`.env`檔案中的支援服務已布建。

要啟動Asset compute項目的Asset compute開發工具，請執行以下操作：

1. 在Asset compute項目根目錄中開啟命令行（在VS代碼中，可通過「終端機」>「新終端機」直接在IDE中開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset compute開發工具將在您的預設Web瀏覽器中開啟，網址為&#x200B;__http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 當開發工具初始化時，查看命令行輸出和Web瀏覽器中的錯誤消息。
1. 若要停止「Asset compute開發工具」，請在執行`aio app run`的視窗中點選`Ctrl-C`以終止程式。

## 疑難排解

+ [由於缺少private.key，開發工具無法啟動](../troubleshooting.md#missing-private-key)
