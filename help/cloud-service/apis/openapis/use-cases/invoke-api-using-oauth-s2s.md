---
title: 使用OAuth伺服器對伺服器驗證叫用OpenAPI型AEM API
description: 瞭解如何使用OAuth伺服器對伺服器驗證，從自訂應用程式在AEM as a Cloud Service上叫用OpenAPI型AEM API。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 8338a905-c4a2-4454-9e6f-e257cb0db97c
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '1687'
ht-degree: 1%

---

# 使用OAuth伺服器對伺服器驗證叫用OpenAPI型AEM API

瞭解如何使用&#x200B;_OAuth伺服器對伺服器_&#x200B;驗證，從自訂應用程式叫用AEM as a Cloud Service上的OpenAPI型AEM API。

OAuth伺服器對伺服器驗證適用於需要API存取而不需使用者互動的後端服務。 它使用OAuth 2.0 _client_credentials_&#x200B;授權型別來驗證使用者端應用程式。

## 您能學到的內容{#what-you-learn}

在本教學課程中，您將學習如何：

- 設定Adobe Developer Console (ADC)專案，以使用&#x200B;_OAuth伺服器對伺服器驗證_&#x200B;存取Assets Author API。

- 開發範例NodeJS應用程式，呼叫Assets Author API以擷取特定資產的中繼資料。

開始之前，請務必檢閱下列內容：

- [存取Adobe API和相關概念](../overview.md#accessing-adobe-apis-and-related-concepts)區段。
- [設定OpenAPI型AEM API](../setup.md)文章。

## 先決條件

若要完成本教學課程，您需要：

- 更新的AEM as a Cloud Service環境，其中包含：
   - AEM版本`2024.10.18459.20241031T210302Z`或更新版本。
   - 新樣式的產品設定檔（如果環境是在2024年11月之前建立的）

  如需詳細資訊，請參閱[設定OpenAPI型AEM API](../setup.md)文章。

- 必須在其上部署範例[WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)專案。

- 存取[Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started)。

- 在本機電腦上安裝[Node.js](https://nodejs.org/en/)，以執行範例NodeJS應用程式。

## 開發步驟

高階開發步驟為：

1. 設定ADC專案
   1. 新增Assets Author API
   1. 將其驗證方法設定為OAuth伺服器對伺服器
   1. 將產品設定檔與驗證設定建立關聯
1. 設定AEM執行個體以啟用ADC專案通訊
1. 開發範例NodeJS應用程式
1. 驗證端對端流程

## 設定ADC專案

設定ADC專案步驟是[設定OpenAPI型AEM API](../setup.md)中的&#x200B;_重複_。 您需重複新增Assets Author API，並將其驗證方法設定為OAuth伺服器對伺服器。

>[!TIP]
>
>請確定您已完成[設定OpenAPI型AEM API](../setup.md#enable-aem-apis-access)文章中的&#x200B;**啟用AEM API存取**&#x200B;步驟。 若沒有它，將無法使用伺服器對伺服器驗證選項。


1. 從[Adobe Developer Console](https://developer.adobe.com/console/projects)，開啟所需的專案。

1. 若要新增AEM API，請按一下&#x200B;**新增API**&#x200B;按鈕。

   ![新增API](../assets/s2s/add-api.png)

1. 在&#x200B;_新增API_&#x200B;對話方塊中，依&#x200B;_Experience Cloud_&#x200B;篩選，並選取&#x200B;**AEM Assets作者API**&#x200B;卡片，然後按一下&#x200B;**下一步**。

   ![新增AEM API](../assets/s2s/add-aem-api.png)

1. 接著，在&#x200B;_設定API_&#x200B;對話方塊中，選取&#x200B;**伺服器對伺服器**&#x200B;驗證選項，然後按一下&#x200B;**下一步**。 伺服器對伺服器驗證適用於需要API存取而不需使用者互動的後端服務。

   ![選取驗證](../assets/s2s/select-authentication.png)

1. 重新命名認證以方便識別（如有需要），然後按一下&#x200B;**下一步**。 為了示範目的，會使用預設名稱。

   ![重新命名認證](../assets/s2s/rename-credential.png)

1. 選取&#x200B;**AEM Assets Collaborator使用者 — 作者 — 方案XXX — 環境XXX**&#x200B;產品設定檔，然後按一下&#x200B;**儲存**。 如您所見，僅與AEM Assets API Users Service相關聯的產品設定檔可供選取。

   ![選取產品設定檔](../assets/s2s/select-product-profile.png)

1. 檢閱AEM API和驗證設定。

   ![AEM API設定](../assets/s2s/aem-api-configuration.png)

   ![驗證組態](../assets/s2s/authentication-configuration.png)

## 設定AEM執行個體以啟用ADC專案通訊

遵循[設定OpenAPI型AEM API](../setup.md#configure-the-aem-instance-to-enable-adc-project-communication)文章中的指示，設定AEM執行個體以啟用ADC專案通訊。

## 開發範例NodeJS應用程式

讓我們開發一個呼叫Assets Author API的範例NodeJS應用程式。

您可以使用其他程式語言（如Java、Python等）來開發應用程式。

為了測試目的，您可以使用[Postman](https://www.postman.com/)、[curl](https://curl.se/)或任何其他REST使用者端來叫用AEM API。

### 檢閱API

在開發應用程式之前，請先檢閱&#x200B;_Assets作者API_&#x200B;中的[傳遞指定資產的中繼資料](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata)端點。 API語法為：

```http
GET https://{bucket}.adobeaemcloud.com/adobe/../assets/{assetId}/metadata
```

若要擷取特定資產的中繼資料，您需要`bucket`和`assetId`值。 `bucket`是不含AEM網域名稱(.adobeaemcloud.com)的Adobe執行個體名稱，例如`author-p63947-e1420428`。

`assetId`是首碼為`urn:aaid:aem:`之資產的JCR UUID，例如`urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`。 有多種方式可取得`assetId`：

- 附加AEM資產路徑`.json`擴充功能以取得資產中繼資料。 例如，`https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json`並尋找`jcr:uuid`屬性。

- 或者，您可以在瀏覽器的元素檢查器中檢查資產，以取得`assetId`。 尋找`data-id="urn:aaid:aem:..."`屬性。

  ![檢查資產](../assets/s2s/inspect-asset.png)

### 使用瀏覽器叫用API

在開發應用程式之前，請先使用[API檔案](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)中的&#x200B;**嘗試它**&#x200B;功能叫用API。

1. 在瀏覽器中開啟[Assets作者API檔案](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)。

1. 展開&#x200B;_中繼資料_&#x200B;區段，然後按一下&#x200B;**傳遞指定資產的中繼資料**&#x200B;選項。

1. 在右窗格中，按一下&#x200B;**嘗試它**按鈕。
   ![API檔案](../assets/s2s/api-documentation.png)

1. 輸入下列值：

   | 區段 | 參數 | 值 |
   | --- | --- | --- |
   |  | 貯體 | 不含AEM網域名稱(.adobeaemcloud.com)的Adobe執行個體名稱，例如`author-p63947-e1420428`。 |
   | **安全性** | 持有人權杖 | 使用ADC專案的OAuth伺服器對伺服器認證中的存取權杖。 |
   | **安全性** | X-Api-Key | 使用ADC專案的OAuth伺服器對伺服器認證中的`ClientID`值。 |
   | **引數** | assetId | AEM中資產的唯一識別碼，例如`urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da` |
   | **引數** | X-Adobe-Accept-Experimental | 1 |

   ![叫用API — 存取權杖](../assets/s2s/generate-access-token.png)

   ![叫用API — 輸入值](../assets/s2s/invoke-api-input-values.png)

1. 按一下&#x200B;**傳送**&#x200B;以叫用API，並在&#x200B;**回應**&#x200B;索引標籤中檢閱回應。

   ![叫用API — 回應](../assets/s2s/invoke-api-response.png)

上述步驟確認AEM as a Cloud Service環境的現代化，啟用AEM API存取權。 這也會確認ADC專案的成功設定，以及與AEM Author例項的OAuth伺服器對伺服器認證ClientID通訊。

### NodeJS應用程式範例

讓我們開發一個範例NodeJS應用程式。

若要開發應用程式，您可以使用&#x200B;_Run-the-sample-application_&#x200B;或&#x200B;_逐步開發_&#x200B;指示。

>[!BEGINTABS]

>[!TAB Run-the-sample-application]

1. 下載範例[demo-nodejs-app-to-invoke-aem-openapi](../assets/s2s/demo-nodejs-app-to-invoke-aem-openapi.zip)應用程式zip檔案並解壓縮。

1. 導覽至解壓縮的資料夾並安裝相依性。

   ```bash
   $ npm install
   ```

1. 以ADC專案的OAuth伺服器對伺服器認證中的實際值取代`.env`檔案中的預留位置。

1. 將`src/index.js`檔案中的`<BUCKETNAME>`和`<ASSETID>`取代為實際值。

1. 執行NodeJS應用程式。

   ```bash
   $ node src/index.js
   ```

>[!TAB 逐步開發]

1. 建立新的NodeJS專案。

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. 安裝&#x200B;_擷取_&#x200B;和&#x200B;_dotenv_&#x200B;程式庫，分別發出HTTP要求並讀取環境變數。

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. 在您最愛的程式碼編輯器中開啟專案，並更新`package.json`檔案以將`type`新增至`module`。

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. 建立`.env`檔案並新增下列組態。 將預留位置取代為ADC專案的OAuth伺服器對伺服器認證中的實際值。

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. 建立`src/index.js`檔案並新增下列程式碼，並以實際值取代`<BUCKETNAME>`和`<ASSETID>`。

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/../assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. 執行NodeJS應用程式。

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### API回應

成功執行後，主控台中會顯示API回應。 回應包含指定資產的中繼資料。

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

恭喜！您已成功使用OAuth伺服器對伺服器驗證，從自訂應用程式叫用以OpenAPI為基礎的AEM API。

### 檢閱應用程式程式碼

範例NodeJS應用程式程式碼中的關鍵圖說文字為：

1. **IMS驗證**：已在ADC專案中使用OAuth伺服器對伺服器認證設定擷取存取權杖。

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **API引動**：叫用Assets Author API，以提供授權的存取權杖來擷取特定資產的中繼資料。

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/../assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## 隱藏在機殼下

成功引發API後，會在AEM Author服務中建立代表ADC專案的OAuth伺服器對伺服器認證的使用者，以及符合產品設定檔和服務設定的使用者群組。 _技術帳戶使用者_&#x200B;與產品設定檔和&#x200B;_服務_&#x200B;使用者群組相關聯，該使用者群組具有&#x200B;_讀取_&#x200B;資產中繼資料的必要許可權。

若要驗證技術帳戶使用者和使用者群組的建立，請遵循下列步驟：

- 在ADC專案中，瀏覽至&#x200B;**OAuth伺服器對伺服器**&#x200B;認證組態。 記下&#x200B;**技術帳戶電子郵件**&#x200B;值。

  ![技術帳戶電子郵件](../assets/s2s/technical-account-email.png)

- 在AEM作者服務中，導覽至&#x200B;**工具** > **安全性** > **使用者**，並搜尋&#x200B;**技術帳戶電子郵件**&#x200B;值。

  ![技術帳戶使用者](../assets/s2s/technical-account-user.png)

- 按一下技術帳戶使用者即可檢視使用者詳細資訊，例如&#x200B;**群組**&#x200B;成員資格。 如下所示，技術帳戶使用者與&#x200B;**AEM Assets Collaborator使用者 — 作者 — 方案XXX — 環境XXX**&#x200B;和&#x200B;**AEM Assets Collaborator使用者 — 服務**&#x200B;使用者群組相關聯。

  ![技術帳戶使用者成員資格](../assets/s2s/technical-account-user-membership.png)

- 請注意，技術帳戶使用者與&#x200B;**AEM Assets Collaborator使用者 — 作者 — 方案XXX — 環境XXX**&#x200B;產品設定檔相關聯。 產品設定檔與&#x200B;**AEM Assets API使用者**&#x200B;和&#x200B;**AEM Assets Collaborator使用者**&#x200B;服務相關聯。

  ![技術帳戶使用者產品設定檔](../assets/s2s/technical-account-user-product-profile.png)

- 您可以在&#x200B;**產品設定檔**&#x200B;的&#x200B;**API認證**&#x200B;索引標籤中驗證產品設定檔與技術帳戶使用者關聯。

  ![產品設定檔API認證](../assets/s2s/product-profile-api-credentials.png)

## 非GET請求的403錯誤

若要&#x200B;_讀取_&#x200B;資產中繼資料，為OAuth伺服器對伺服器認證建立的技術帳戶使用者必須透過Services使用者群組(例如AEM Assets Collaborator Users - Service)擁有必要許可權。

不過，若要&#x200B;_建立、更新、刪除_ (CLOUD)資產中繼資料，技術帳戶使用者需要其他許可權。 您可以使用非GET請求(例如，PATCH、DELETE)叫用API來驗證它，並觀察403錯誤回應。

讓我們叫用&#x200B;_PATCH_&#x200B;要求來更新資產中繼資料，並觀察403錯誤回應。

- 在瀏覽器中開啟[Assets作者API檔案](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)。

- 輸入下列值：

  | 區段 | 參數 | 值 |
  | --- | --- | --- |
  | **貯體** |  | 不含AEM網域名稱(.adobeaemcloud.com)的Adobe執行個體名稱，例如`author-p63947-e1420428`。 |
  | **安全性** | 持有人權杖 | 使用ADC專案的OAuth伺服器對伺服器認證中的存取權杖。 |
  | **安全性** | X-Api-Key | 使用ADC專案的OAuth伺服器對伺服器認證中的`ClientID`值。 |
  | **內文** |  | `[{ "op": "add", "path": "foo","value": "bar"}]` |
  | **引數** | assetId | AEM中資產的唯一識別碼，例如`urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da` |
  | **引數** | X-Adobe-Accept-Experimental | * |
  | **引數** | X-Adobe-Accept-Experimental | 1 |

- 按一下&#x200B;**傳送**&#x200B;以叫用&#x200B;_PATCH_&#x200B;要求，並觀察403錯誤回應。

  ![叫用API - PATCH要求](../assets/s2s/invoke-api-patch-request.png)

若要修正403錯誤，您有兩個選項：

- 在ADC Project中，以適當的產品設定檔更新OAuth伺服器對伺服器認證的相關產品設定檔，該產品設定檔具有&#x200B;_建立、更新、刪除_ (CUD)資產中繼資料的必要許可權，例如，**AEM管理員 — 作者 — 方案XXX — 環境XXX**。 如需詳細資訊，請參閱[如何 — API的連線認證和產品設定檔管理](../how-to/credentials-and-product-profile-management.md)文章。

- 使用AEM專案，在AEM Author中更新關聯的AEM Service使用者群組(例如，AEM Assets Collaborator Users - Service)許可權，以允許建立、更新、刪除&#x200B;_(CLOUD)資產中繼資料。_&#x200B;如需詳細資訊，請參閱[如何進行 — AEM Service使用者群組許可權管理](../how-to/services-user-group-permission-management.md)文章。

## 摘要

在本教學課程中，您已瞭解如何從自訂應用程式叫用OpenAPI型AEM API。 您已啟用AEM API存取，並建立和設定Adobe Developer Console (ADC)專案。
在ADC專案中，您已新增AEM API、設定其驗證型別，並與產品設定檔建立關聯。 您也將AEM例項設定為啟用ADC專案通訊，並開發呼叫Assets Author API的範例NodeJS應用程式。

## 其他資源

- [OAuth 伺服器對伺服器認證實作指南](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation)
