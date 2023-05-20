---
title: 在App Builder操作中生成訪問令牌
description: 瞭解如何使用JWT憑據生成訪問令牌，以在App Builder操作中使用。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 1%

---

# 在App Builder操作中生成訪問令牌

App Builder操作可能需要與與App Builder應用程式部署的App Builder項目相關聯的AdobeAPI交互。

這可能需要App Builder操作來生成與所需的Adobe Developer控制台項目關聯的自己的訪問令牌。

>[!IMPORTANT]
>
> 審閱 [App Builder安全文檔](https://developer.adobe.com/app-builder/docs/guides/security/) 瞭解何時生成訪問令牌與使用提供的訪問令牌是合適的。
>
> 自定義操作可能需要提供自己的安全檢查，以確保只有允許的用戶才能訪問App Builder操作及其後面的Adobe服務。


## .env檔案

在App Builder項目中 `.env` 檔案，為每個Adobe Developer控制台項目的JWT憑據添加自定義密鑰。 JWT憑據值可以從Adobe Developer控制台項目的 __憑據__ > __服務帳戶(JWT)__ 的下界。

![Adobe Developer控制台JWT服務憑據](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

的值 `JWT_CLIENT_ID`。 `JWT_CLIENT_SECRET`。 `JWT_TECHNICAL_ACCOUNT_ID`。 `JWT_IMS_ORG` 可以從Adobe Developer控制台項目的JWT憑據螢幕直接複製。

### 梅塔斯科普

確定AdobeAPI及其元目標，以便App Builder操作與之交互。 在中列出帶逗號分隔符的元組 `JWT_METASCOPES` 按鈕 有效元目錄列於 [Adobe的JWT Metascope文檔](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/)。


例如，可以將以下值添加到 `JWT_METASCOPES` 的 `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私鑰

的 `JWT_PRIVATE_KEY` 必須特別格式化，因為它是本機的多行值，在中不支援 `.env` 的子菜單。 最簡單的方法是base64編碼私鑰。 Base64編碼私鑰(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)可以使用作業系統提供的本機工具完成。

>[!BEGINTABS]

>[!TAB macOS]

1. 開啟 `Terminal`
1. 運行命令 `base64 -i /path/to/private.key | pbcopy`
1. 會自動複製base64輸出
1. 貼上到 `.env` 值與相應鍵

>[!TAB Windows]

1. 開啟 `Command Prompt`
1. 運行命令 `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 運行命令 `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. 將base64輸出複製到剪貼簿
1. 貼上到 `.env` 值與相應鍵

>[!TAB Linux®]

1. 開放終端
1. 運行命令 `base64 private.key`
1. 將base64輸出複製到剪貼簿
1. 貼上到 `.env` 值與相應鍵

>[!ENDTABS]

例如，以下base64編碼的私鑰可能添加到 `JWT_PRIVATE_KEY` 的 `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 輸入映射

將JWT憑據值設定為 `.env` 檔案，必須將其映射到AppBuilder操作輸入，以便在操作本身中讀取這些輸入。 為此，請在 `ext.config.yaml` 動作 `inputs` 的下界： `PARAMS_INPUT_NAME: $ENV_KEY`。

例如：

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

定義的鍵 `inputs` 在 `params` 提供給App Builder操作的對象。


## 訪問令牌的JWT憑據

在App Builder操作中，JWT憑據在 `params` 對象，並可用於 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) 生成訪問令牌，而訪問令牌又可訪問其他AdobeAPI和服務。

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
