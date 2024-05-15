---
title: 在App Builder動作中產生伺服器對伺服器存取權杖
description: 瞭解如何使用OAuth伺服器對伺服器憑證產生存取權杖，以用於App Builder動作。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# 在App Builder動作中產生伺服器對伺服器存取權杖

App Builder動作可能需要與支援的Adobe API互動 **OAuth伺服器對伺服器認證** 和已與Adobe Developer Console專案關聯，並已部署App Builder應用程式。

本指南說明如何使用產生存取權杖 _OAuth伺服器對伺服器認證_ 用於App Builder動作。

>[!IMPORTANT]
>
> 服務帳戶(JWT)憑證已遭取代，改用OAuth伺服器對伺服器憑證。 不過，仍有部分AdobeAPI僅支援服務帳戶(JWT)憑證，且正在移轉至OAuth伺服器對伺服器。 請檢閱Adobe API檔案，以瞭解支援的認證。

## Adobe Developer Console專案設定

將所需的AdobeAPI新增至Adobe Developer Console專案時，請在 _設定API_ 步驟，選取 **OAuth伺服器對伺服器** 驗證型別。

![Adobe Developer主控台 — OAuth伺服器對伺服器](./assets/s2s-auth/oauth-server-to-server.png)

若要指派上述自動建立的整合服務帳戶，請選取所需的產品設定檔。 因此，透過產品設定檔，可控制服務帳戶許可權。

![Adobe Developer Console — 產品設定檔](./assets/s2s-auth/select-product-profile.png)

## .env檔案

在App Builder專案的 `.env` 檔案，附加Adobe Developer主控台專案的OAuth伺服器對伺服器憑證的自訂金鑰。 OAuth伺服器對伺服器認證值可從Adobe Developer主控台專案的 __認證__ > __OAuth伺服器對伺服器__ 指定工作區的。

![Adobe Developer主控台OAuth伺服器對伺服器認證](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

以下專案的值： `OAUTHS2S_CLIENT_ID`， `OAUTHS2S_CLIENT_SECRET`， `OAUTHS2S_CECREDENTIALS_METASCOPES` 可以從Adobe Developer主控台專案的OAuth伺服器對伺服器憑證畫面直接複製。

## 輸入對應

OAuth伺服器對伺服器認證值設定於 `.env` 檔案中，這些區段必須對應至AppBuilder動作輸入，才能在動作中讀取。 若要這麼做，請在每個變數中新增專案 `ext.config.yaml` 動作 `inputs` 格式為： `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

下定義的索引鍵 `inputs` 可在 `params` 提供給App Builder動作的物件。

## 用於存取Token的OAuth伺服器對伺服器認證

在App Builder動作中， OAuth伺服器對伺服器憑證位於 `params` 物件。 使用這些認證，可以使用以下專案產生存取權杖 [OAuth 2.0資料庫](https://oauth.net/code/). 或者，您可以使用 [節點擷取程式庫](https://www.npmjs.com/package/node-fetch) 向Adobe IMS權杖端點發出POST請求以取得存取權杖。

下列範例示範如何使用 `node-fetch` 資料庫，向Adobe IMS權杖端點發出POST請求以取得存取權杖。

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
