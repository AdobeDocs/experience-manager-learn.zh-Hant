---
title: 本地開發訪問令牌
description: 本AEM地開發訪問令牌用於加快與as a Cloud Service的整合開發，該以寫程式方式與AEM Author或通過HTTP發佈服務交互。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 本地開發訪問令牌

需要寫程式訪問as a Cloud Service的整合開發AEM商需要一種簡單、快速的方法來獲取臨時訪問令牌，以AEM便於本地開發活動。 為了滿足這一需AEM要，開發人員控制台允許開發人員自行生成臨時訪問令牌，這些令牌可用於以寫程式方式訪AEM問。

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## 生成本地開發訪問令牌

![獲取本地開發訪問令牌](assets/local-development-access-token/getting-a-local-development-access-token.png)

本地開發訪問令牌提供對AEM作者和發佈服務的訪問，以及這些服務的權限。 儘管這是開發令牌，但不要共用此令牌或將其儲存在原始碼管理中。

1. 在 [Adobe Admin Console](https://adminconsole.adobe.com/) 確保您（開發人員）是以下成員：
   + __雲管理器 — 開發人員__ IMS產品配置檔案(授予對開發人員控AEM制台的訪問權)
   + 或者 __管AEM理員__ 或 __用AEM戶__ IMS產品配置檔案，AEM用於環境服務訪問令牌與
   + 沙盒AEMas a Cloud Service環境只要求在 __管AEM理員__ 或 __用AEM戶__ 產品配置檔案
1. 登錄到 [Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含要與整合AEM的as a Cloud Service環境的程式
1. 點擊 __省略號__ 環境旁邊 __環境__ ，然後選擇 __開發人員控制台__
1. 點擊 __整合__ 頁籤
1. 點擊 __本地令牌__ 頁籤
1. 點擊 __獲取本地開發令牌__ 按鈕
1. 點擊 __下載按鈕__ 下載包含 `accessToken` 值，並將JSON檔案保存到開發電腦上的安全位置。
   + 這是您對as a Cloud Service環境的24小時開發人員訪AEM問令牌。

![開AEM發者控制台 — 整合 — 獲取本地開發令牌](./assets/local-development-access-token/developer-console.png)

## 已使用本地開發訪問令牌{#use-local-development-access-token}

![本地開發訪問令牌 — 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從開發人員控制台下載臨時本地開發訪AEM問令牌
   + 本地開發訪問令牌每24小時過期一次，因此開發人員需要每天下載新的訪問令牌
1. 正在開發一個以寫程式方式與as a Cloud Service交互的外部應AEM用
1. 本地開發訪問令牌中的外部應用程式讀取
1. 外部應用程式將HTTP請求構建AEM到as a Cloud Service，並將本地開發訪問令牌作為承載令牌添加到HTTP請求的授權標頭
1. AEMas a Cloud Service接收HTTP請求，驗證請求，並執行HTTP請求所請求的工作，並將HTTP響應返回到外部應用程式

### 外部應用程式示例

我們將建立一個簡單的外部JavaScript應用程式，以說明如何使用本地開發AEM者訪問令牌通過HTTPS以寫程式方式訪問as a Cloud Service。 這說明了 _任何_ 在外部運行的應用程式或AEM系統，無論框架或語言如何，都可以使用訪問令牌以寫程式方式對as a Cloud Service進行身份驗證和訪AEM問。 在 [下一部分](./service-credentials.md)，我們將更新此應用程式碼以支援生成令牌以供生產使用的方法。

此示例應用程式從命令行運行，並使AEM用AEM AssetsHTTP API更新資產元資料，使用以下流：

1. 從命令行讀取參數(`getCommandLineParams()`)
1. 獲取用於驗證到as a Cloud Service的訪AEM問令牌(`getAccessToken(...)`)
1. 列出在命令行參AEM數中指定的資產資料夾中的所有資產(`listAssetsByFolder(...)`)
1. 使用命令行參數中指定的值更新列出的資產的元資料(`updateMetadata(...)`)

以寫程式方式驗證到使用訪AEM問令牌時的關鍵元素是，將授權HTTP請求標頭添加到所有向其發出的HTTP請AEM求，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 運行外部應用程式

1. 確保 [節點.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) 安裝在本地開發電腦上，用於運行外部應用程式
1. 下載並解壓縮 [示例外部應用程式](./assets/aem-guides_token-authentication-external-application.zip)
1. 從命令行中，在此項目的資料夾中運行 `npm install`
1. 複製 [已下載本地開發訪問令牌](#download-local-development-access-token) 到名為 `local_development_token.json` 在項目的根部
   + 但請記住，千萬不要向Git提交任何憑據！
1. 開啟 `index.js` 並審閱外部應用程式碼和注釋。

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   查看 `fetch(..)` 調用 `listAssetsByFolder(...)` 和 `updateMetadata(...)`，通知 `headers` 定義 `Authorization` 值為的HTTP請求標頭 `Bearer ACCESS_TOKEN`。 這是從外部應用程式發起的HTTP請求如何驗證到AEMas a Cloud Service。

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   對as a Cloud Service的任何HTTPAEM請求都必須在授權標頭中設定承載訪問令牌。 請記住，每AEM個as a Cloud Service環境都需要自己的訪問令牌。 開發的訪問令牌不在階段或生產上工作，階段不在開發或生產上工作，生產不在開發或階段工作！

1. 使用命令行，從項目的根執行應用程式，傳遞以下參數：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   傳入以下參數：

   + `aem`:應用程式與之交互的AEMas a Cloud Service環境的方案和主機名(例如 `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:資產資料夾路徑，其資產使用 `propertyValue`;不添加 `/content/dam` 前置詞（例如） `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的資產屬性名稱，相對於 `[dam:Asset]/jcr:content` (例如 `metadata/dc:rights`)。
   + `propertyValue`:設定 `propertyName` 至；包含空格的值需要用 `"` (例如 `"WKND Limited Use"`)
   + `file`:從Developer Console下載的JSON檔案的相AEM對檔案路徑。

   成功執行每個更新資產的應用程式結果輸出：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證元資料更新AEM

通過登錄到as a Cloud Service環境來驗證元資料是否已更AEM新(確保將同一主機傳入到 `aem` 命令行參數被訪問)。

1. 登錄外部應AEM用程式與之交互的as a Cloud Service環境(使用中提供的同一主機 `aem` 命令行參數)
1. 導航到 __資產__ > __檔案__
1. 導航它由 `folder` 命令行參數，例如 __WKND__ > __英語__ > __冒險__ > __納帕品酒會__
1. 開啟 __屬性__ 資料夾中任何（非內容片段）資產
1. 點擊 __高級__ 頁籤
1. 查看已更新屬性的值，例如 __版權__ 映射到已更新 `metadata/dc:rights` JCR屬性，它反映在 `propertyValue` 參數，例如 __WKND有限使用__

![WKND有限使用元資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

現在，我們已使用本地開發令牌AEM以寫程式方式訪問as a Cloud Service。 接下來，我們需要更新應用程式以使用服務憑據進行處理，以便此應用程式可以在生產上下文中使用。

+ [如何使用服務憑據](./service-credentials.md)
