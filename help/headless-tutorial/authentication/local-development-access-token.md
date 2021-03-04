---
title: 本機開發存取Token
description: Local AEM Development Access TokenAEM可用來加速與Cloud Service整合的開發，此可透過HTTP以程式設計方式與AEM Author或Publish服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---


# 本機開發存取Token

建立整合的開發人員需要以程式化方式將AEMCloud Service存取為，需要簡單、快速的方式取得暫存存取Token，以AEM協助本端開發活動。 為滿足此需求，AEMDeveloper Console可讓開發人員自行產生暫存存取Token，以程式設計方式存取AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 產生本機開發存取Token

![取得本機開發存取Token](assets/local-development-access-token/getting-a-local-development-access-token.png)

「本機開發存取Token」提供AEM Author和Publish服務的存取權，以產生Token的使用者身分及其權限。 雖然這是開發Token，但請勿共用此Token，或儲存在來源控制項中。

1. 在[AdobeAdminConsole](https://adminconsole.adobe.com/)中，確保您（開發人員）是以下成員：
   + __雲端管理員-__ DeveloperIMS產品設定檔(授與Developer AEM Console的存取權)
   + __管理員AEM__&#x200B;或&#x200B;__AEM使用者__&#x200B;環境服務的IMS產品設定檔，存取Token將與
   + 沙AEM盒作為Cloud Service環境，只需要&#x200B;__AEM Administrators__&#x200B;或&#x200B;__AEM Users__&#x200B;產品設定檔的會籍
1. 登入[Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含的方AEM案做為Cloud Service環境，以整合
1. 點選&#x200B;__環境__&#x200B;區段中環境旁的&#x200B;__ellipsis__，然後選擇&#x200B;__開發人員主控台__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得本機開發Token__&#x200B;按鈕
1. 點選左上角的&#x200B;__下載按鈕__，下載包含`accessToken`值的JSON檔案，並將JSON檔案儲存至您開發機器上的安全位置。
   + 這是您24小時的開發人員存取Token,AEM做為Cloud Service環境。

![開AEM發人員主控台——整合——取得本機開發Token](./assets/local-development-access-token/developer-console.png)

## 已使用本機開發存取Token{#use-local-development-access-token}

![本機開發存取Token —— 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從Developer Console下載暫存本機開發存取AEMToken
   + 本機開發存取Token每24小時過期一次，因此開發人員每天必須下載新的存取Token
1. 正在開發以程式設計方式與外部應用程式AEM互動的Cloud Service
1. 外部應用程式在本機開發存取Token中讀取
1. 外部應用程式將HTTP請求建構AEM為Cloud Service，將本機開發存取Token新增為HTTP請求的授權標題中的承載Token
1. 當AEMCloud Service收到HTTP請求時，驗證請求並執行HTTP請求所要求的工作，並將HTTP回應傳回外部應用程式

### 範例外部應用程式

我們將建立簡單的外部JavaScript應用程式，以說明如何使用本機開發人員存取Token，以程式設計AEM方式，以HTTPSCloud Service方式存取。 這說明了&#x200B;_any_&#x200B;應用程式或系統如何使用存取Token以程式設計方式驗證並存取，以做為Cloud ServiceAEM，而不論該應用程式或系統是在哪個架構或語言AEM之外執行。 在[下一節](./service-credentials.md)中，我們將更新此應用程式碼，以支援產生代號以供生產使用的方法。

此範例應用程式是從命令列執行，並使用AEM AssetsHTTP APIAEM更新資產中繼資料，使用下列流程：

1. 從命令行(`getCommandLineParams()`)讀取參數
1. 取得用於驗證為Cloud ServiceAEM的存取Token(`getAccessToken(...)`)
1. 列出命令行參AEM數(`listAssetsByFolder(...)`)中指定的資產資料夾中的所有資產
1. 使用命令列參數(`updateMetadata(...)`)中指定的值更新已列出資產的中繼資料

以程式設計方式驗證以使用存取TokenAEM的關鍵元素，是將授權HTTP要求標題新增至所有以下格AEM式提出的HTTP要求：

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

   查看`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`調用，並注意`headers`定義了值`Bearer ACCESS_TOKEN`的`Authorization` HTTP請求標頭。 這是從外部應用程式產生的HTTP要求驗證為AEMCloud Service的方式。

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

   任何HTTP請求AEM都必須在「授權」標題中設定「承載存取」Token，做為Cloud Service。 請記住，每AEM個Cloud Service環境都需要有自己的存取Token。 開發的存取Token不適用於舞台或生產，Stage的不適用於開發或生產，而Production的不適用於開發或舞台！

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

   + `aem`:應用程式將與之互動AEM的Cloud Service環境(例如： `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:資產資料夾路徑，其資產將會以 `propertyValue`;請勿新增首 `/content/dam` 碼(例如 `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的資產屬性名稱，相對 `[dam:Asset]/jcr:content` 於(例如 `metadata/dc:rights`)。
   + `propertyValue`:將設定為的 `propertyName` 值；含空格的值需要以 `"` （例如）封裝 `"WKND Limited Use"`)
   + `file`:從Developer Console下載的JSON檔案的相AEM對檔案路徑。

   每個資產的應用程式結果輸出已更新：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證中繼資料更新AEM

通過以Cloud Service環境的身份登錄來驗證元AEM資料是否已更新（確保訪問傳入`aem`命令行參數的同一台主機）。

1. 以外部應AEM用程式與之交互的Cloud Service環境登錄（使用`aem`命令行參數中提供的相同主機）
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽`folder`命令列參數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟資料夾中任何（非內容片段）資產的&#x200B;__屬性__
1. 點選至&#x200B;__進階__&#x200B;標籤
1. 檢視已更新屬性的值，例如映射至已更新`metadata/dc:rights` JCR屬性的&#x200B;__Copyright__，反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Limited Use__

![WKND Limited使用中繼資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

既然我們已使用本機開發Token以程式設AEM計方式以Cloud Service方式存取，我們需要更新應用程式以使用「服務認證」來處理，因此此應用程式可用於生產環境。

+ [如何使用服務憑據](./service-credentials.md)
