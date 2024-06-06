---
title: Github.com webhook驗證
description: 瞭解如何在App Builder動作中驗證來自Github.com的webhook請求。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Github.com webhook驗證

Webhook可讓您建立或設定可訂閱GitHub.com上特定事件的整合。 觸發其中一項事件時，GitHub會傳送HTTPPOST裝載至webhook已設定的URL。 然而，基於安全考量，請務必確認傳入的webhook請求是否實際來自GitHub，而非惡意執行者。 本教學課程會逐步引導您執行步驟，在Adobe App Builder動作中使用共用密碼驗證GitHub.com webhook要求。

## 在AppBuilder中設定Github機密

1. **新增密碼至 `.env` 檔案：**

   在App Builder專案的 `.env` 檔案中，新增GitHub.com webhook密碼的自訂金鑰：

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **更新 `ext.config.yaml` 檔案：**

   此 `ext.config.yaml` 必須更新檔案以驗證GitHub.com webhook請求。

   - 設定AppBuilder動作 `web` 設定到 `raw` 從GitHub.com接收原始要求內文。
   - 在 `inputs` 在AppBuilder動作設定中，新增 `GITHUB_SECRET` 索引鍵，將其對應至 `.env` 包含密碼的欄位。 此索引鍵的值為 `.env` 前置詞為的欄位名稱 `$`.
   - 設定 `require-adobe-auth` AppBuilder動作設定中的註解至 `false` 允許不需要Adobe驗證就呼叫動作。

   產生的結果 `ext.config.yaml` 檔案應如下所示：

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## 將驗證程式碼新增至AppBuilder動作

接下來，新增下方提供的JavaScript程式碼(複製自 [GitHub.com的檔案](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example))，完成您的AppBuilder動作。 請務必匯出 `verifySignature` 函式。

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## 在AppBuilder動作中實作驗證

接下來，透過比較請求標頭中的簽名與產生的簽名，確認請求來自GitHub。 `verifySignature` 函式。

在AppBuilder動作的 `index.js`，將下列程式碼新增至 `main` 函式：


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## 在GitHub中設定webhook

返回GitHub.com，在建立webhook時為GitHub.com提供相同的機密值。 使用您指定的密碼值 `.env` 檔案的 `GITHUB_SECRET` 機碼。

在GitHub.com中，前往存放庫設定，並編輯webhook。 在webhook設定中，提供中的密碼值 `Secret` 欄位。 按一下 __更新webhook__ 以儲存變更。

![Github Webhook密碼](./assets/github-webhook-verification/github-webhook-settings.png)

按照以下步驟操作，您能確定您的App Builder動作可以安全地驗證傳入的webhook請求確實來自您的GitHub.com webhook。
