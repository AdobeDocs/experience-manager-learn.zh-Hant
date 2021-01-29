---
title: 本機開發存取Token
description: AEM Local Development Access Token可用來加速與AEM整合的開發，以Cloud Services的形式，透過HTTP以程式設計方式與AEM Author或Publish服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
translation-type: tm+mt
source-git-commit: c4f3d437b5ecfe6cb97314076cd3a5e31b184c79
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---


# 本機開發存取Token

建立整合的開發人員需要以程式化方式以雲端服務形式存取AEM，他們需要一種簡單、快速的方式，來取得AEM的暫存存取Token，以利進行本機開發活動。 為了滿足這個需求，AEM的Developer Console可讓開發人員自行產生暫時存取Token，以程式設計方式存取AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 產生本機開發存取Token

![取得本機開發存取Token](assets/local-development-access-token/getting-a-local-development-access-token.png)

「本機開發存取Token」提供AEM Author和Publish服務的存取權，以產生Token的使用者身分及其權限。 雖然這是開發Token，但請勿共用此Token，或儲存在來源控制項中。

1. 在[Adobe AdminConsole](https://adminconsole.adobe.com/)中，請確定您（開發人員）為下列成員：
   + __Cloud Manager -__ DeveloperIMS產品設定檔（授與AEM Developer Console的存取權）
   + AEM環境服務的&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__ IMS產品設定檔，存取Token將與
   + 將AEM當做雲端服務環境執行時，只需要&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__&#x200B;產品設定檔的會籍
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM做為雲端服務環境的方案，以與
1. 點選&#x200B;__環境__&#x200B;區段中環境旁的&#x200B;__ellipsis__，然後選擇&#x200B;__開發人員主控台__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得本機開發Token__&#x200B;按鈕
1. 點選左上角的&#x200B;__下載按鈕__，下載包含`accessToken`值的JSON檔案，並將JSON檔案儲存至您開發機器上的安全位置。
   + 這是您24小時的AEM Cloud服務環境開發人員存取Token。

![AEM Developer Console —— 整合——取得本機開發Token](./assets/local-development-access-token/developer-console.png)

## 已使用本機開發存取Token{#use-local-development-access-token}

![本機開發存取Token —— 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從AEM Developer Console下載臨時本機開發存取Token
   + 本機開發存取Token每24小時過期一次，因此開發人員每天必須下載新的存取Token
1. 正在開發以程式設計方式與AEM互動的外部應用程式，做為雲端服務
1. 外部應用程式在本機開發存取Token中讀取
1. 外部應用程式會將HTTP請求建構為AEM的雲端服務，並將本機開發存取Token新增為HTTP請求的授權標題中的承載Token
1. AEM作為雲端服務，會接收HTTP要求、驗證要求並執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 範例外部應用程式

我們將建立簡單的外部JavaScript應用程式，以說明如何使用本機開發人員存取Token，以程式設計方式透過HTTPS以雲端服務形式存取AEM。 這說明&#x200B;_任何_&#x200B;在AEM外部執行的應用程式或系統（不論架構或語言）如何使用存取Token，以程式設計方式驗證AEM並以雲端服務的身分存取AEM。 在[下一節](./service-credentials.md)中，我們將更新此應用程式碼，以支援產生代號以供生產使用的方法。

此範例應用程式是從命令列執行，並使用AEM Assets HTTP API更新AEM Asset中繼資料，使用下列流程：

1. 從命令行(`getCommandLineParams()`)讀取參數
1. 取得用來驗證AEM為雲端服務(`getAccessToken(...)`)的存取Token
1. 列出在命令列參數(`listAssetsByFolder(...)`)中指定之AEM資產資料夾中的所有資產
1. 使用命令列參數(`updateMetadata(...)`)中指定的值更新已列出資產的中繼資料

使用存取Token以程式設計方式驗證AEM的關鍵元素，是將「授權HTTP要求」標題新增至對AEM提出的所有HTTP要求，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 運行外部應用程式

1. 請確定本機開發機器上已安裝[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)，將用來執行外部應用程式
1. 下載並解壓縮[範例外部應用程式](./assets/aem-guides_token-authentication-external-application.zip)
1. 在此項目資料夾的命令行中運行`npm install`
1. 將下載的[本機開發存取Token](#download-local-development-access-token)複製至專案根目錄中名為`local_development_token.json`的檔案
   + 但請記住，千萬不要向Git提交任何認證！
1. 開啟`index.js`並檢閱外部應用程式碼和註解。

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   查看`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`調用，並注意`headers`定義了值`Bearer ACCESS_TOKEN`的`Authorization` HTTP請求標頭。 這是從外部應用程式產生的HTTP要求如何以雲端服務的身分驗證AEM。

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

   任何以雲端服務形式向AEM提出的HTTP要求，都必須在「授權」標題中設定「承載存取」Token。 請記住，每個AEM都是雲端服務環境，需要有自己的存取Token。 開發的存取Token不適用於舞台或生產，Stage的不適用於開發或生產，而Production的不適用於開發或舞台！

1. 使用命令行，從項目的根執行應用程式，傳遞以下參數：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   傳入下列參數：

   + `aem`:AEM的方案和主機名稱是應用程式要與之互動的雲端服務環境(例如 `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:資產資料夾路徑，其資產將會以 `propertyValue`;請勿新增首 `/content/dam` 碼(例如 `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的資產屬性名稱，相對 `[dam:Asset]/jcr:content` 於(例如 `metadata/dc:rights`)。
   + `propertyValue`:將設定為的 `propertyName` 值；含空格的值需要以 `"` （例如）封裝 `"WKND Limited Use"`)
   + `file`:從AEM Developer Console下載之JSON檔案的相對檔案路徑。

   每個資產的應用程式結果輸出已更新：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證AEM中的中繼資料更新

以雲端服務環境的身分登入AEM，確認中繼資料已更新（請確定已存取傳入`aem`命令列參數的相同主機）。

1. 以外部應用程式互動的雲端服務環境（使用`aem`命令列參數中提供的相同主機）登入AEM
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽`folder`命令列參數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟資料夾中任何（非內容片段）資產的&#x200B;__屬性__
1. 點選至&#x200B;__進階__&#x200B;標籤
1. 檢視已更新屬性的值，例如映射至已更新`metadata/dc:rights` JCR屬性的&#x200B;__Copyright__，反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Limited Use__

![WKND Limited使用中繼資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

現在，我們已使用本機開發Token以程式設計方式將AEM存取為Cloud Service，因此需要更新應用程式以使用「服務認證」來處理，如此此應用程式就可用於生產環境。

+ [如何使用服務憑據](./service-credentials.md)
