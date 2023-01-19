---
title: 在App Builder動作中產生存取權杖
description: 了解如何使用JWT憑證產生存取權杖，以便用於「應用程式產生器」動作。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 643a9844f19aa1bd153661540ec7f7398a35118e
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 1%

---


# 在App Builder動作中產生存取權杖

App Builder動作可能需要與已部署App Builder應用程式之Adobe Developer Console專案相關聯的AdobeAPI互動。

這可能需要App Builder動作，才能產生與所需Adobe Developer Console專案相關聯的存取權杖。

>[!IMPORTANT]
>
> 檢閱 [App Builder安全性檔案](https://developer.adobe.com/app-builder/docs/guides/security/) 了解何時可產生存取權杖，以及何時可使用提供的存取權杖。
>
> 自訂動作可能需要提供自己的安全性檢查，以確保只有允許的消費者才能存取App Builder動作及其背後的Adobe服務。


## .env檔案

在App Builder專案的 `.env` 檔案中，為每個Adobe Developer Console專案的JWT憑證附加自訂金鑰。 您可從Adobe Developer主控台專案的 __憑證__ > __服務帳戶(JWT)__ 的URL區段。

![Adobe Developer Console JWT服務憑證](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

的值 `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` 可直接從Adobe Developer主控台專案的JWT憑證畫面複製。

### 梅塔斯科佩

判斷AdobeAPI，以及App Builder動作與之互動的元素。 清單元素，其中包含逗號分隔字元 `JWT_METASCOPES` 鍵。 有效的元目錄列於 [Adobe的JWT Metascope文檔](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


例如，下列值可新增至 `JWT_METASCOPES` 鍵 `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私密金鑰

此 `JWT_PRIVATE_KEY` 必須經過特殊格式，因為它是多行值， `.env` 檔案。 最簡單的方法是對私密金鑰進行base64編碼。 對私鑰進行編碼的Base64(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)可使用作業系統提供的原生工具來完成。

>[!BEGINTABS]

>[!TAB macOS]

1. 開啟 `Terminal`
1. 運行命令 `base64 -i /path/to/private.key | pbcopy`
1. base64輸出會自動複製剪貼簿
1. 貼入 `.env` 作為對應索引鍵的值

>[!TAB Windows]

1. 開啟 `Command Prompt`
1. 運行命令 `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 運行命令 `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. 將base64輸出複製到剪貼簿
1. 貼入 `.env` 作為對應索引鍵的值

>[!TAB Linux®]

1. 開放終端
1. 運行命令 `base64 private.key`
1. 將base64輸出複製到剪貼簿
1. 貼入 `.env` 作為對應索引鍵的值

>[!ENDTABS]

例如，下列base64編碼的私密金鑰可新增至 `JWT_PRIVATE_KEY` 鍵 `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 輸入映射

在 `.env` 檔案，則必須對應至AppBuilder動作輸入，才能在動作本身中讀取。 若要這麼做，請為 `ext.config.yaml` 動作 `inputs` 格式中： `INPUT_NAME=$ENV_KEY`.

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

在下定義的鍵 `inputs` 在 `params` 物件。


## 存取Token的JWT憑證

在「應用程式產生器」動作中，可在 `params` 對象，可由 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) 來產生存取權杖，如此便可存取其他AdobeAPI和服務。

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
