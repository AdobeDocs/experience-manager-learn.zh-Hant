---
title: 本機開發存取權杖
description: AEM本機開發存取Token可用來加速與AEMas a Cloud Service的整合開發，以程式設計方式與AEM製作或透過HTTP發佈服務互動。
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

# 本機開發存取權杖

建置整合功能(需要程式化存取AEM as a Cloud Service)的開發人員需要透過簡單、快速的方式取得AEM的暫時存取權杖，以利進行本機開發活動。 為了滿足此需求，AEM開發人員控制台可讓開發人員自行產生暫時存取權杖，以供以程式設計方式存取AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## 產生本機開發存取權杖

![取得本機開發存取權杖](assets/local-development-access-token/getting-a-local-development-access-token.png)

本機開發存取權杖提供AEM製作和發佈服務的存取權，以產生權杖的使用者身分及其權限。 儘管這是開發權杖，但請勿共用此權杖，或將其儲存在原始碼控制項中。

1. 在 [Adobe Admin Console](https://adminconsole.adobe.com/) 確保您（開發人員）是以下成員：
   + __Cloud Manager — 開發人員__ IMS產品設定檔(授予AEM Developer Console的存取權)
   + 其中 __AEM管理員__ 或 __AEM使用者__ AEM環境服務的IMS產品設定檔存取權杖已與
   + 沙箱AEMas a Cloud Service環境只需要 __AEM管理員__ 或 __AEM使用者__ 產品設定檔
1. 登入 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEMas a Cloud Service環境的程式以與
1. 點選 __省略號__ 環境旁邊 __環境__ 部分，然後選擇 __開發人員控制台__
1. 點選 __整合__ 標籤
1. 點選 __本機代號__ 標籤
1. 點選 __取得本機開發代號__ 按鈕
1. 點選 __下載按鈕__ 的JSON檔案(包含 `accessToken` 值，並將JSON檔案儲存至開發電腦上的安全位置。
   + 這是您24小時前到AEMas a Cloud Service環境的開發人員存取權杖。

![AEM開發人員主控台 — 整合 — 取得本機開發代號](./assets/local-development-access-token/developer-console.png)

## 已使用本機開發存取權杖{#use-local-development-access-token}

![本機開發存取權杖 — 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從AEM Developer Console下載暫時的本機開發存取權杖
   + 本機開發存取權杖每24小時就會過期，因此開發人員每天都需要下載新的存取權杖
1. 正在開發以程式設計方式與AEM as a Cloud Service互動的外部應用程式
1. 外部應用程式讀取本機開發存取權杖
1. 外部應用程式會建構AEMas a Cloud Service的HTTP要求，並將本機開發存取權杖新增為HTTP要求的授權標題中的承載權杖
1. AEMas a Cloud Service會接收HTTP要求、驗證要求，並執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 範例外部應用程式

我們將建立簡單的外部JavaScript應用程式，以說明如何使用本機開發人員存取權杖，透過HTTPS以程式設計方式存取AEMas a Cloud Service。 這說明如何 _any_ 在AEM外部執行的應用程式或系統（不論是架構或語言）可使用存取權杖，以程式設計方式驗證AEM，並存取as a Cloud Service。 在 [下一節](./service-credentials.md)，我們將更新此應用程式程式碼，以支援產生代號以供生產使用的方法。

此範例應用程式從命令列執行，並透過下列流程使用AEM Assets HTTP API更新AEM資產中繼資料：

1. 從命令行讀取參數(`getCommandLineParams()`)
1. 取得用來驗證AEMas a Cloud Service的存取權杖(`getAccessToken(...)`)
1. 列出在命令列參數中指定之AEM資產資料夾中的所有資產(`listAssetsByFolder(...)`)
1. 使用命令列參數中指定的值更新列出的資產的中繼資料(`updateMetadata(...)`)

以程式設計方式使用存取權杖來驗證AEM中的關鍵元素，是將授權HTTP要求標題新增至對AEM提出的所有HTTP要求，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 運行外部應用程式

1. 確保 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) 安裝在本機開發電腦上，用來執行外部應用程式
1. 下載並解壓縮 [範例外部應用程式](./assets/aem-guides_token-authentication-external-application.zip)
1. 從命令行，在此項目的資料夾中，運行 `npm install`
1. 複製 [已下載本機開發存取權杖](#download-local-development-access-token) 檔案名為 `local_development_token.json` 在項目根目錄中
   + 但請記住，絕不要向Git提交任何憑證！
1. 開啟 `index.js` 並查看外部應用程式代碼和注釋。

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

   檢閱 `fetch(..)` 調用 `listAssetsByFolder(...)` 和 `updateMetadata(...)`，和通知 `headers` 定義 `Authorization` 值為的HTTP要求標題 `Bearer ACCESS_TOKEN`. 這是從外部應用程式產生的HTTP要求如何驗證AEMas a Cloud Service。

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

   對AEMas a Cloud Service的任何HTTP要求，都必須在授權標題中設定Bearer存取權杖。 請記住，每個AEMas a Cloud Service環境都需有其專屬的存取權杖。 開發的存取權杖無法用於預備或生產，預備的不適用於開發或生產，而生產的不適用於開發或預備！

1. 使用命令行，從項目的根執行應用程式，並傳遞以下參數：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   會傳入下列參數：

   + `aem`:應用程式與之互動的AEMas a Cloud Service環境的配置和主機名稱(例如 `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:資產已更新為的資產資料夾路徑 `propertyValue`;請勿新增 `/content/dam` 前置詞(如 `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的資產屬性名稱，相對於 `[dam:Asset]/jcr:content` (例如 `metadata/dc:rights`)。
   + `propertyValue`:設定 `propertyName` 至；包含空格的值需要以 `"` (例如 `"WKND Limited Use"`)
   + `file`:從AEM開發人員控制台下載的JSON檔案的相對檔案路徑。

   已更新每個資產的應用程式結果輸出成功執行：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證AEM中的中繼資料更新

登入AEMas a Cloud Service環境，確認中繼資料已更新（確認傳入的主機相同） `aem` 命令列參數)。

1. 登入外部應用程式與之互動的AEMas a Cloud Service環境(使用 `aem` 命令行參數)
1. 導覽至 __資產__ > __檔案__
1. 導覽至 `folder` 命令列參數，例如 __WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟 __屬性__ （非內容片段）資產
1. 點選 __進階__ 標籤
1. 檢閱已更新屬性的值，例如 __版權__ 已對應至已更新 `metadata/dc:rights` JCR屬性，反映 `propertyValue` 參數，例如 __WKND有限使用__

![WKND有限使用元資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

現在，我們已使用本機開發代號，以程式設計方式存取AEM as a Cloud Service。 接下來，我們需要更新應用程式以使用服務憑證來處理，因此此應用程式可用於生產環境。

+ [如何使用服務憑證](./service-credentials.md)
