---
title: 服務認證
description: 瞭解如何使用服務認證，這些認證可用來協助外部應用程式、系統和服務，以程式設計方式透過HTTP與作者或發佈服務互動。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
duration: 881
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '1963'
ht-degree: 0%

---

# 服務認證

與Adobe Experience Manager (AEM) as a Cloud Service的整合必須能夠安全地向AEM服務進行驗證。 AEM的Developer Console會授予服務憑證的存取權，這些憑證可用來促進外部應用程式、系統和服務以程式設計方式透過HTTP與AEM作者或發佈服務互動。

AEM與使用[S2S OAuth (透過Adobe Developer Console](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/setting-up-ims-integrations-for-aem-as-a-cloud-service)管理)的其他Adobe產品整合。 對於與服務帳戶的自訂整合，需在AEM Developer Console中使用和管理JWT憑證。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

服務認證可能會顯示類似於[本機開發存取權杖](./local-development-access-token.md)，但在幾個關鍵方面不同：

+ 服務認證與技術帳戶相關聯。 技術帳戶可使用多個服務認證。
+ 服務認證&#x200B;_不是_&#x200B;存取權杖，而是用於&#x200B;_取得_&#x200B;存取權杖的認證。
+ 服務憑證更具永久性（其憑證每365天過期一次），且除非被撤銷，否則不會變更，而本機開發存取權杖每天都會過期。
+ AEM as a Cloud Service環境的服務憑證對應至單一AEM技術帳戶使用者，而本機開發存取權杖會針對產生存取權杖的AEM使用者進行驗證。
+ AEM as a Cloud Service環境最多可以有10個技術帳戶，每個帳戶都有自己的服務憑證，每個憑證對應至分散的技術帳戶AEM使用者。

服務憑證及其產生的存取權杖，以及本機開發存取權杖，都應保密。 這三者皆能用來取得各自AEM as a Cloud Service環境的存取權。

## 產生服務認證

服務認證產生分為兩個步驟：

1. 由Adobe IMS組織管理員建立的一次性技術帳戶
1. 下載及使用技術帳戶的服務認證JSON

### 建立技術帳戶

服務認證（不同於本機開發存取權杖）需要技術帳戶由Adobe組織IMS管理員建立，才能下載。 應為需要程式化存取AEM的每個使用者端建立獨立技術帳戶。

![建立技術帳戶](assets/service-credentials/initialize-service-credentials.png)

技術帳戶只建立一次，不過隨著時間推移，可管理使用私密金鑰來管理與技術帳戶相關聯的服務認證。 例如，新的私密金鑰/服務認證必須在目前私密金鑰到期之前產生，以允許服務認證的使用者不受中斷地存取。

1. 確定您是以下列身分登入：
   + __Adobe IMS組織的系統管理員__
   + __AEM作者__&#x200B;上的&#x200B;__AEM管理員__ IMS產品設定檔的成員
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM as a Cloud Service環境的程式，以整合設定的服務認證
1. 點選&#x200B;__環境__&#x200B;區段中環境旁的省略符號，然後選取&#x200B;__Developer Console__
1. 點選「__整合__」標籤
1. 點選&#x200B;__技術帳戶__&#x200B;標籤
1. 點選&#x200B;__建立新的技術帳戶__&#x200B;按鈕
1. 技術帳戶的服務認證已初始化並顯示為JSON

![AEM Developer Console — 整合 — 取得服務認證](./assets/service-credentials/developer-console.png)

初始化AEM as Cloud Service環境的服務憑證後，您Adobe IMS組織中的其他AEM開發人員就可以下載這些憑證。

### 下載服務認證

![下載服務認證](assets/service-credentials/download-service-credentials.png)

下載服務憑證的步驟與初始化類似。

1. 確定您是以下列身分登入：
   + __Adobe IMS組織管理員__
   + __AEM作者__&#x200B;上的&#x200B;__AEM管理員__ IMS產品設定檔的成員
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM as a Cloud Service環境的程式以與整合
1. 點選&#x200B;__環境__&#x200B;區段中環境旁的省略符號，然後選取&#x200B;__Developer Console__
1. 點選「__整合__」標籤
1. 點選&#x200B;__技術帳戶__&#x200B;標籤
1. 展開要使用的&#x200B;__技術帳戶__
1. 展開將下載其服務認證的&#x200B;__私密金鑰__，並確認狀態為&#x200B;__使用中__
1. 點選與&#x200B;__私密金鑰__&#x200B;相關聯的&#x200B;__...__ > __檢視__，這會顯示服務認證JSON
1. 點選左上角的下載按鈕以下載包含服務憑證值的JSON檔案，並將檔案儲存到安全位置

## 安裝服務認證

服務憑證提供產生JWT所需的詳細資訊，此資訊會交換用來向AEM as a Cloud Service驗證的存取權杖。 服務認證必須儲存在安全位置，可供外部應用程式、系統或服務存取AEM。 每個客戶管理服務認證的方式和位置都是唯一的。

為簡化起見，本教學課程透過命令列傳入服務認證。 然而，請與您的IT安全團隊合作，瞭解如何根據貴組織的安全方針，儲存及存取這些認證。

1. 將[下載的服務認證JSON](#download-service-credentials)複製到專案根目錄中名為`service_token.json`的檔案
   + 請記住，絕對不要將&#x200B;_任何認證_&#x200B;認可給Git！

## 使用服務認證

服務憑證是完整格式的JSON物件，與JWT或存取權杖不同。 而是使用服務認證（包含私密金鑰）來產生JWT，再與Adobe IMS API交換存取權杖。

![服務認證 — 外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 將服務認證從AEM Developer Console下載至安全位置
1. 外部應用程式需要以程式設計方式與AEM as a Cloud Service環境互動
1. 外部應用程式會從安全位置讀取服務證明資料
1. 外部應用程式會使用服務憑證中的資訊來建構JWT權杖
1. JWT權杖會傳送至Adobe IMS以交換存取權杖
1. Adobe IMS傳回可用來存取AEM as a Cloud Service的存取權杖
   + 存取權杖無法變更到期時間。
1. 外部應用程式會向AEM as a Cloud Service發出HTTP請求，將存取權杖作為持有人權杖新增到HTTP請求的授權標頭
1. AEM as a Cloud Service會收到HTTP要求、驗證要求，並執行HTTP要求所要求的工作，然後將HTTP回應傳回至外部應用程式

### 外部應用程式的更新

若要使用服務憑證存取AEM as a Cloud Service，外部應用程式必須透過三種方式更新：

1. 讀取服務認證

+ 為簡化起見，服務憑證會從下載的JSON檔案中讀取，但在實際使用情況下，服務憑證必須根據您組織的安全方針安全儲存

1. 從服務憑證產生JWT
1. 交換JWT以取得存取權杖

+ 當存在服務憑證時，外部應用程式在存取AEM as a Cloud Service時會使用此存取權杖，而不是本機開發存取權杖

在本教學課程中，Adobe的`@adobe/jwt-auth` npm模組是用來在單一函式呼叫中，(1)從服務憑證產生JWT，以及(2)交換它以取得存取權杖。 如果您的應用程式不是以JavaScript為基礎，請檢閱其他語言的[範常式式碼](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)，瞭解如何從服務憑證建立JWT，並與Adobe IMS交換存取權杖。

## 讀取服務認證

請檢閱`getCommandLineParams()`，瞭解如何使用讀取本機開發存取權杖JSON時所使用的相同程式碼來讀取服務認證JSON檔案。

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## 建立JWT並交換存取權杖

讀取服務認證後，即會使用它們產生JWT，然後與Adobe IMS API交換存取權杖。 接著，您就可以使用此存取權杖來存取AEM as a Cloud Service。

此範例應用程式是以Node.js為基礎，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組來促進(1) JWT的產生和(20)與Adobe IMS的交換。 如果您的應用程式是使用其他語言開發的，請檢閱[適當的程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)，瞭解如何使用其他程式語言建構向Adobe IMS傳送的HTTP要求。

1. 更新`getAccessToken(..)`以檢查JSON檔案內容，並判斷其是否代表本機開發存取權杖或服務認證。 這可透過檢查`.accessToken`屬性的存在來輕鬆達成，該屬性僅存在於本機開發存取權杖JSON中。

   如果提供服務認證，應用程式會產生JWT並與Adobe IMS交換存取權杖。 使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函式，該函式會產生JWT並在單一函式呼叫中將它交換為存取權杖。 `auth(..)`方法的引數是[JSON物件，包含服務認證JSON提供的特定資訊](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)，如下列程式碼所述。

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    現在，根據哪個JSON檔案（本機開發存取權杖JSON或服務認證JSON）是透過&#39;file&#39;命令列引數傳入，應用程式將會衍生存取權杖。
    
    請記住，雖然服務認證每365天過期一次，但JWT和對應的存取權杖經常過期，而且需要在過期前重新整理。 使用[Adobe IMS提供的](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)的&#39;refresh_token&#39;即可完成這項作業。

1. 完成這些變更後，服務認證JSON已從AEM Developer Console下載，為簡化起見，將儲存為`service_token.json`，並儲存於與此`index.js`相同的資料夾中。 現在，讓我們執行應用程式，將命令列引數`file`取代為`service_token.json`，並將`propertyValue`更新為新值，以便在AEM中顯示效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   輸出到終端機的方式如下：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行表示對AEM as a Cloud Service的HTTP API呼叫發生錯誤。 嘗試更新資產的中繼資料時，會發生這403個禁止錯誤。

   原因在於服務憑證衍生的存取權杖會使用自動建立的技術帳戶AEM使用者（預設情況下只有讀取存取權）來驗證對AEM的請求。 若要提供應用程式對AEM的寫入存取權，與存取權杖相關聯的技術帳戶AEM使用者必須在AEM中取得許可權。

## 在AEM中設定存取權

服務認證衍生的存取權杖使用的技術帳戶AEM使用者擁有&#x200B;__貢獻者__ AEM使用者群組的成員資格。

![服務認證 — 技術帳戶AEM使用者](./assets/service-credentials/technical-account-user.png)

一旦技術帳戶AEM使用者存在於AEM中（具有存取權杖的第一個HTTP請求之後），就可以像管理其他AEM使用者一樣管理此AEM使用者的許可權。

1. 首先，開啟從AEM Developer Console下載的服務認證JSON以找出技術帳戶的AEM登入名稱，然後找出應類似於`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`的`integration.email`值。
1. 以AEM管理員身分登入對應AEM環境的作者服務
1. 瀏覽至&#x200B;__工具__ > __安全性__ > __使用者__
1. 找到在步驟1中識別具有&#x200B;__登入名稱__&#x200B;的AEM使用者，並開啟其&#x200B;__屬性__
1. 導覽至「__群組__」標籤，然後新增&#x200B;__DAM使用者__&#x200B;群組（他們可做為資產的寫入存取權）
   + [檢視AEM提供的使用者群組清單](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=zh-Hant#built-in-users-and-groups)，以將服務使用者新增至以獲得最佳許可權。 如果AEM提供的使用者群組都不夠用，請建立您自己的使用者群組，並新增適當的許可權。
1. 點選&#x200B;__儲存並關閉__

在AEM允許技術帳戶擁有資產的寫入許可權的情況下，請重新執行應用程式：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

輸出到終端機的方式如下：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 驗證變更

1. 登入已更新的AEM as a Cloud Service環境（使用`aem`命令列引數中提供的相同主機名稱）
1. 導覽至&#x200B;__Assets__ > __檔案__
1. 瀏覽至`folder`命令列引數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __Napa品酒會__
1. 開啟資料夾中任何資產的&#x200B;__屬性__
1. 導覽至&#x200B;__進階__&#x200B;標籤
1. 檢閱更新屬性的值，例如&#x200B;__Copyright__，其對應到更新的`metadata/dc:rights` JCR屬性，現在會反映`propertyValue`引數中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND限制的使用中繼資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已使用本機開發存取權杖和生產就緒服務對服務存取權杖，以程式設計方式存取AEM as a Cloud Service！
