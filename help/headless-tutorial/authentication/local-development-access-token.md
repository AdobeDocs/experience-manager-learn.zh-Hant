---
title: 本機開發存取權杖
description: AEM本機開發存取權杖可用來加速與AEM整合的開發，以Cloud Service以程式設計方式與AEM製作或透過HTTP發佈服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: 無頭式整合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---


# 本機開發存取權杖

建置整合的開發人員需要以程式化方式存取AEM as aCloud Service，需要以簡單、快速的方式取得AEM的暫存存取權杖，以利進行本機開發活動。 為了滿足此需求，AEM開發人員控制台可讓開發人員自行產生暫時存取權杖，以供以程式設計方式存取AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 產生本機開發存取權杖

![取得本機開發存取權杖](assets/local-development-access-token/getting-a-local-development-access-token.png)

本機開發存取權杖提供AEM製作和發佈服務的存取權，以產生權杖的使用者身分及其權限。 儘管這是開發權杖，但請勿共用此權杖，或將其儲存在原始碼控制項中。

1. 在[AdobeAdminConsole](https://adminconsole.adobe.com/)中，確定您（開發人員）是以下成員：
   + __Cloud Manager — 開__ 發人員IMS產品設定檔(授予AEM開發人員控制台的存取權)
   + AEM環境服務的&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__ IMS產品設定檔，存取權杖會與
   + 將AEM作為Cloud Service環境時，只需要&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__&#x200B;產品設定檔的成員資格
1. 登入[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM作為Cloud Service環境的程式，以便與
1. 點選&#x200B;__Environments__&#x200B;區段中環境旁的&#x200B;__刪節號__，然後選取&#x200B;__Developer Console__
1. 點選&#x200B;__整合__&#x200B;標籤
1. 點選「__取得本機開發代號__」按鈕
1. 點選左上角的&#x200B;__下載按鈕__&#x200B;以下載包含`accessToken`值的JSON檔案，並將JSON檔案儲存至開發電腦上的安全位置。
   + 這是您24小時以來的開發人員存取權杖，以AEM作為Cloud Service環境。

![AEM開發人員主控台 — 整合 — 取得本機開發代號](./assets/local-development-access-token/developer-console.png)

## 已使用本機開發存取權杖{#use-local-development-access-token}

![本機開發存取權杖 — 外部應用程式](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 從AEM Developer Console下載暫時的本機開發存取權杖
   + 本機開發存取權杖每24小時就會過期，因此開發人員每天都需要下載新的存取權杖
1. 正在開發外部應用程式，以程式設計方式與AEM作為Cloud Service互動
1. 外部應用程式讀取本機開發存取權杖
1. 外部應用程式會將HTTP要求建構為AEM作為Cloud Service，並將本機開發存取權杖新增為HTTP要求的授權標題中的承載權杖
1. AEM as aCloud Service會接收HTTP要求、驗證要求、執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 範例外部應用程式

我們將建立簡單的外部JavaScript應用程式，以說明如何使用本機開發人員存取權杖，以程式設計方式透過HTTPS以Cloud Service方式存取AEM。 這說明&#x200B;_any_&#x200B;應用程式或系統如何使用存取權杖，以程式方式驗證AEM作為Cloud Service，並存取AEM。 在[下一節](./service-credentials.md)中，我們將更新此應用程式代碼以支援產生供生產使用的令牌的方法。

此範例應用程式從命令列執行，並透過下列流程使用AEM Assets HTTP API更新AEM資產中繼資料：

1. 從命令行讀取參數(`getCommandLineParams()`)
1. 獲取用於驗證AEM作為Cloud Service的訪問令牌(`getAccessToken(...)`)
1. 列出命令列參數(`listAssetsByFolder(...)`)中指定之AEM資產資料夾中的所有資產
1. 使用命令列參數(`updateMetadata(...)`)中指定的值更新列出的資產的中繼資料

以程式設計方式使用存取權杖來驗證AEM中的關鍵元素，是將授權HTTP要求標題新增至對AEM提出的所有HTTP要求，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 運行外部應用程式

1. 請確定已在本地開發電腦上安裝[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)，該電腦將用於運行外部應用程式
1. 下載並解壓縮[範例外部應用程式](./assets/aem-guides_token-authentication-external-application.zip)
1. 從命令行，在此項目的資料夾中，運行`npm install`
1. 將下載的本機開發存取權杖](#download-local-development-access-token)複製到專案根目錄中名為`local_development_token.json`的檔案[
   + 但請記住，絕不要向Git提交任何憑證！
1. 開啟`index.js`並查看外部應用程式代碼和注釋。

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

   查看`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`調用，並注意`headers`以值`Bearer ACCESS_TOKEN`定義`Authorization` HTTP請求標題。 這是從外部應用程式產生的HTTP要求以AEM作為Cloud Service進行驗證的方式。

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

   任何以AEM為Cloud Service的HTTP要求，都必須在授權標題中設定承載存取權杖。 請記住，每個AEM作為Cloud Service環境都需要其自己的存取權杖。 開發的訪問令牌在Stage或Production上無法工作，Stage在Development或Production上無法工作，而Production在Development或Stage上無法工作！

1. 使用命令行，從項目的根執行應用程式，並傳遞以下參數：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   會傳入下列參數：

   + `aem`:應用程式將與之互動的AEMCloud Service環境的配置和主機名稱(例如 `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:資產將以更新的資產資料夾路 `propertyValue`徑；請勿新增首 `/content/dam` 碼(例如 `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的資產屬性名稱，相對 `[dam:Asset]/jcr:content` 於(例如 `metadata/dc:rights`)。
   + `propertyValue`:要將設定為的 `propertyName` 值；含有空格的值需以( `"` 例如)封裝 `"WKND Limited Use"`)
   + `file`:從AEM開發人員控制台下載的JSON檔案的相對檔案路徑。

   已更新每個資產的應用程式結果輸出成功執行：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 驗證AEM中的中繼資料更新

以Cloud Service環境的身分登入AEM ，確認中繼資料已更新（確認已存取傳入`aem`命令列參數的相同主機）。

1. 以外部應用程式與之互動的Cloud Service環境登入AEM（使用`aem`命令列參數中提供的相同主機）
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽至`folder`命令列參數所指定的資產資料夾，例如&#x200B;__WKND__ > __English__ > __Adventures__ > __Napa品酒會__
1. 開啟資料夾中任何（非內容片段）資產的&#x200B;__屬性__
1. 點選&#x200B;__進階__&#x200B;標籤
1. 查看更新屬性的值，例如&#x200B;__Copyright__，該值映射到更新的`metadata/dc:rights` JCR屬性，該屬性反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Limited Use__

![WKND有限使用元資料更新](./assets/local-development-access-token/asset-metadata.png)

## 後續步驟

現在，我們已使用本機開發Token以程式設計方式存取AEM作為Cloud Service，因此需要更新應用程式以使用服務憑證來處理，因此此應用程式可用於生產環境。

+ [如何使用服務憑證](./service-credentials.md)
