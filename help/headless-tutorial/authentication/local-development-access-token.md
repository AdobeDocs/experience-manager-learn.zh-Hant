---
title: 本機開發存取權杖
description: AEM本機開發存取權杖可用來加速與AEM as a Cloud Service整合的開發，以便透過HTTP以程式設計方式與AEM製作或發佈服務互動。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 本機開發存取權杖

在建置需要以程式設計方式存取AEM as a Cloud Service的整合功能時，開發人員需要透過簡單快速的方式，取得AEM的暫時存取權杖，以便利本機開發活動。 為了滿足此需求，AEM的Developer Console可讓開發人員自行產生暫時性存取權杖，這些權杖可用來以程式設計方式存取AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## 產生本機開發存取權杖

![取得本機開發存取權杖](assets/local-development-access-token/getting-a-local-development-access-token.png)

本機開發存取Token可讓您以產生Token的使用者身分，存取AEM作者和發佈服務，以及其許可權。 儘管這是開發權杖，請勿共用此權杖或儲存在原始檔控制中。

1. 在[Adobe Admin Console](https://adminconsole.adobe.com/)中，請確定您身為開發人員，是下列成員之一：
   + __Cloud Manager — 開發人員__ IMS產品設定檔(授與AEM Developer Console的存取權)
   + 存取權杖整合之AEM環境服務的&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__ IMS產品設定檔
   + 沙箱AEM as a Cloud Service環境僅需要&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__&#x200B;產品設定檔的成員資格
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM as a Cloud Service環境的程式以與整合
1. 點選&#x200B;__環境__&#x200B;區段中環境旁的&#x200B;__省略符號__，然後選取&#x200B;__Developer Console__
1. 點選「__整合__」標籤
1. 點選&#x200B;__本機權杖__&#x200B;索引標籤
1. 點選&#x200B;__取得本機開發權杖__&#x200B;按鈕
1. 點選左上角的&#x200B;__下載按鈕__&#x200B;以下載包含`accessToken`值的JSON檔案，並將JSON檔案儲存到開發電腦上的安全位置。
   + 這是您的24小時開發人員存取AEM as a Cloud Service環境的Token。

![AEM Developer Console — 整合 — 取得本機開發權杖](./assets/local-development-access-token/developer-console.png)

## 已使用本機開發存取權杖{#use-local-development-access-token}

![本機開發存取權杖 — 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從AEM Developer Console下載暫時的本機開發存取權杖
   + 本機開發存取權杖每24小時過期一次，因此開發人員需要每天下載新的存取權杖
1. 正在開發可與AEM as a Cloud Service以程式設計方式互動的外部應用程式
1. 外部應用程式會讀取本機開發存取權杖
1. 外部應用程式會建構HTTP要求給AEM as a Cloud Service，將本機開發存取權杖新增為持有人權杖到HTTP要求的授權標頭
1. AEM as a Cloud Service會收到HTTP要求、驗證要求，並執行HTTP要求所要求的工作，然後將HTTP回應傳回至外部應用程式

### 外部應用程式範例

我們將建立簡單的外部JavaScript應用程式，說明如何使用本機開發人員存取權杖透過HTTPS以程式設計方式存取AEM as a Cloud Service。 這可說明在AEM外部執行的&#x200B;_任何_&#x200B;應用程式或系統（不論架構或語言為何）如何使用存取權杖以程式設計方式驗證及存取AEM as a Cloud Service。 在[下一個區段](./service-credentials.md)中，我們將更新此應用程式程式碼以支援產生用於生產用途的權杖的方法。

此範例應用程式從命令列執行，並使用以下流程使用AEM HTTP API更新AEM Assets資產中繼資料：

1. 從命令列(`getCommandLineParams()`)讀取引數
1. 取得用來驗證AEM as a Cloud Service (`getAccessToken(...)`)的存取權杖
1. 列出在命令列引數(`listAssetsByFolder(...)`)中指定的AEM資產資料夾中的所有資產
1. 使用命令列引數(`updateMetadata(...)`)中指定的值更新列出的資產中繼資料

使用存取權杖以程式設計方式向AEM進行驗證的關鍵元素，是向AEM發出的所有HTTP請求新增授權HTTP請求標頭，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 執行外部應用程式

1. 確定您的本機開發電腦上已安裝[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)，此電腦是用來執行外部應用程式
1. 下載並解壓縮[範例外部應用程式](./assets/aem-guides_token-authentication-external-application.zip)
1. 從命令列，在此專案的資料夾中，執行`npm install`
1. 將[下載的本機開發存取權杖](#download-local-development-access-token)複製到專案根目錄中名為`local_development_token.json`的檔案
   + 但請記住，絕對不要向Git提交任何認證！
1. 開啟`index.js`並檢閱外部應用程式程式碼和註解。

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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=zh-Hant#retrieve-a-folder-listing
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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=zh-Hant#update-asset-metadata
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

   檢閱`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`引動過程，並注意`headers`定義值為`Bearer ACCESS_TOKEN`的`Authorization` HTTP要求標頭。 以下說明源自外部應用程式的HTTP要求向AEM as a Cloud Service驗證的方式。

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

   向AEM as a Cloud Service發出的任何HTTP請求都必須在授權標頭中設定持有者存取權杖。 請記住，每個AEM as a Cloud Service環境都需要各自的存取權杖。 開發的存取權杖在Stage或Production上無效，Stage的無法用於Development或Production，Production的無法用於Development或Stage！

1. 使用命令列，從專案的根目錄執行應用程式，傳遞下列引數：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   下列引數會傳入中：

   + `aem`：應用程式與其互動之AEM as a Cloud Service環境的配置和主機名稱（例如`https://author-p1234-e5678.adobeaemcloud.com`）。
   + `folder`：其資產已以`propertyValue`更新的資產資料夾路徑；請勿新增`/content/dam`首碼（例如`/wknd-shared/en/adventures/napa-wine-tasting`）
   + `propertyName`：要更新的資產屬性名稱，相對於`[dam:Asset]/jcr:content` （例如`metadata/dc:rights`）。
   + `propertyValue`：要設定`propertyName`的值；含有空格的值必須封裝為`"` （例如`"WKND Limited Use"`）
   + `file`：從AEM Developer Console下載之JSON檔案的相對檔案路徑。

   成功執行每個資產的應用程式結果輸出已更新：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證AEM中的中繼資料更新

登入AEM as a Cloud Service環境，驗證中繼資料是否已更新（確定已存取傳遞至`aem`命令列引數的相同主機）。

1. 登入外部應用程式互動的AEM as a Cloud Service環境（使用`aem`命令列引數中提供的相同主機）
1. 導覽至&#x200B;__Assets__ > __檔案__
1. 瀏覽至`folder`命令列引數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __Napa品酒會__
1. 開啟資料夾中任何（非內容片段）資產的&#x200B;__屬性__
1. 點選至&#x200B;__進階__&#x200B;標籤
1. 檢閱更新屬性的值，例如&#x200B;__Copyright__，其對應到更新的`metadata/dc:rights` JCR屬性，其反映`propertyValue`引數中提供的值，例如&#x200B;__WKND Limited Use__

![WKND限制使用中繼資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

現在我們已使用本機開發權杖以程式設計方式存取AEM as a Cloud Service。 接下來，我們需要更新應用程式以使用服務憑證來處理，以便此應用程式可用於生產環境中。

+ [如何使用服務認證](./service-credentials.md)
