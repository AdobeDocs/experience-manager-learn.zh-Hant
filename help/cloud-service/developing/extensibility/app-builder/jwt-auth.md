---
title: 在App Builder動作中產生JWT存取權杖
description: 瞭解如何使用JWT憑證產生存取權杖，以用於App Builder動作。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# 在App Builder動作中產生JWT存取權杖

App Builder動作可能需要與已部署Adobe應用程式的Adobe Developer Console專案相關聯的App Builder API互動。

這可能會要求App Builder動作產生與所需Adobe Developer Console專案相關聯的JWT存取權杖。

>[!IMPORTANT]
>
> 請檢閱[App Builder安全性檔案](https://developer.adobe.com/app-builder/docs/guides/security/)，以瞭解何時適合產生存取權杖而不適合使用提供的存取權杖。
>
> 自訂動作可能需要提供自己的安全性檢查，以確保只有允許的消費者才能存取App Builder動作及其背後的Adobe服務。


## .env檔案

在App Builder專案的`.env`檔案中，為每個Adobe Developer Console專案的JWT憑證附加自訂金鑰。 可以從指定工作區的Adobe Developer Console專案的&#x200B;__認證__ > __服務帳戶(JWT)__&#x200B;取得JWT認證值。

![Adobe Developer Console JWT服務認證](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

`JWT_CLIENT_ID`、`JWT_CLIENT_SECRET`、`JWT_TECHNICAL_ACCOUNT_ID`、`JWT_IMS_ORG`的值可以從Adobe Developer Console專案的JWT認證畫面直接複製。

### Metascope

判斷App Builder動作互動的Adobe API及其中繼資料。 在`JWT_METASCOPES`索引鍵中列出帶有逗號分隔符號的Metascope。 [Adobe的JWT Metascope檔案](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/scopes)中列出了有效的Metascope。


例如，下列值可新增至`.env`中的`JWT_METASCOPES`機碼：

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私密金鑰

`JWT_PRIVATE_KEY`必須特別格式化，因為它原本是多行值，在`.env`檔案中不支援。 最簡單的方法是以base64編碼私密金鑰。 Base64編碼私密金鑰(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)可使用作業系統提供的原生工具來完成。

>[!BEGINTABS]

>[!TAB macOS]

1. 開啟`Terminal`
1. 執行命令`base64 -i /path/to/private.key | pbcopy`
1. base64輸出會自動複製到剪貼簿
1. 將值貼上到`.env`中作為對應索引鍵

>[!TAB Windows]

1. 開啟`Command Prompt`
1. 執行命令`certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 執行命令`findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. 將base64輸出複製到剪貼簿
1. 將值貼上到`.env`中作為對應索引鍵

>[!TAB Linux®]

1. 開啟終端機
1. 執行命令`base64 private.key`
1. 將base64輸出複製到剪貼簿
1. 將值貼上到`.env`中作為對應索引鍵

>[!ENDTABS]

例如，下列base64編碼私密金鑰可以新增至`.env`中的`JWT_PRIVATE_KEY`金鑰：

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 輸入對應

在`.env`檔案中設定JWT認證值後，這些值必須對應至AppBuilder動作輸入，才能在動作中讀取。 若要這麼做，請在`ext.config.yaml`動作`inputs`中為每個變數新增專案，格式為： `PARAMS_INPUT_NAME: $ENV_KEY`。

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

在`inputs`下定義的金鑰可在提供給App Builder動作的`params`物件上使用。


## 用於存取權杖的JWT認證

在App Builder動作中， JWT認證可在`params`物件中使用，並可由[`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth)用來產生存取權杖，進而可存取其他Adobe API和服務。

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
