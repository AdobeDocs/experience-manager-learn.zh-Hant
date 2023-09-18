---
title: 服務認證
description: 瞭解如何使用服務認證，這些認證可用來協助外部應用程式、系統和服務，以程式設計方式透過HTTP與作者或發佈服務互動。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: 65d8fd58f421a186e3624918c70cc5d79ec23700
workflow-type: tm+mt
source-wordcount: '1967'
ht-degree: 0%

---

# 服務認證

與Adobe Experience Manager (AEM)as a Cloud Service的整合必須能夠安全地向AEM服務進行驗證。 AEM開發人員控制檯授予對服務憑證的存取權，這些憑證用於促進外部應用程式、系統和服務以程式設計方式透過HTTP與AEM作者或發佈服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

服務認證可能顯示類似 [本機開發存取權杖](./local-development-access-token.md) 但在幾個關鍵方面不同：

+ 服務認證與技術帳戶相關聯。 技術帳戶可使用多個服務認證。
+ 服務認證為 _非_ 存取權杖，而不是用於以下用途的認證： _取得_ 存取權杖。
+ 服務憑證更具永久性（其憑證每365天過期一次），且除非被撤銷，否則不會變更，而本機開發存取權杖每天都會過期。
+ AEMas a Cloud Service環境的服務認證對應至單一AEM技術帳戶使用者，而本機開發存取權杖會針對產生存取權杖的AEM使用者進行驗證。
+ AEMas a Cloud Service環境最多可以有十個技術帳戶，每個帳戶都有自己的服務憑證，每個憑證都與分散的技術帳戶AEM使用者對應。

服務憑證及其產生的存取權杖，以及本機開發存取權杖，都應保密。 這三者皆可取得，因此可存取其各自的AEMas a Cloud Service環境。

## 產生服務認證

服務認證產生分為兩個步驟：

1. 由Adobe IMS組織管理員建立的一次性技術帳戶
1. 下載及使用技術帳戶的服務認證JSON

### 建立技術帳戶

服務憑證與本機開發存取權杖不同，需要技術帳戶先由Adobe組織IMS管理員建立，才能下載。 應為需要程式化存取AEM的每個使用者端建立獨立技術帳戶。

![建立技術帳戶](assets/service-credentials/initialize-service-credentials.png)

技術帳戶只建立一次，不過隨著時間推移，可管理使用私密金鑰來管理與技術帳戶相關聯的服務認證。 例如，新的私密金鑰/服務認證必須在目前私密金鑰到期之前產生，以允許服務認證的使用者不受中斷地存取。

1. 確定您是以下列身分登入：
   + __Adobe IMS組織的系統管理員__
   + 成員隸屬於 __AEM管理員__ 上的IMS產品設定檔 __AEM作者__
1. 登入 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEMas a Cloud Service環境的程式，以整合設定的服務認證
1. 點選中環境旁的省略符號 __環境__ 部分，然後選取 __開發人員主控台__
1. 點選至 __整合__ 標籤
1. 點選 __技術帳戶__ 標籤
1. 點選 __建立新的技術帳戶__ 按鈕
1. 技術帳戶的服務認證已初始化並顯示為JSON

![AEM開發人員主控台 — 整合 — 取得服務認證](./assets/service-credentials/developer-console.png)

初始化AEM as Cloud Service環境的服務憑證後，您Adobe IMS組織中的其他AEM開發人員就可以下載這些憑證。

### 下載服務認證

![下載服務認證](assets/service-credentials/download-service-credentials.png)

下載服務憑證的步驟與初始化類似。

1. 確定您是以下列身分登入：
   + __Adobe IMS組織管理員__
   + 成員隸屬於 __AEM管理員__ 上的IMS產品設定檔 __AEM作者__
1. 登入 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEMas a Cloud Service環境的程式以與整合
1. 點選中環境旁的省略符號 __環境__ 部分，然後選取 __開發人員主控台__
1. 點選至 __整合__ 標籤
1. 點選 __技術帳戶__ 標籤
1. 展開 __技術帳戶__ 將使用
1. 展開 __私密金鑰__ 將下載其服務認證，並確認狀態為 __作用中__
1. 點選 __...__ > __檢視__ 與 __私密金鑰__，會顯示服務認證JSON
1. 點選左上角的下載按鈕以下載包含服務憑證值的JSON檔案，並將檔案儲存到安全位置

## 安裝服務認證

服務憑證提供產生JWT所需的詳細資訊，此資訊會交換用來向AEMas a Cloud Service驗證的存取權杖。 服務認證必須儲存在安全位置，可供外部應用程式、系統或使用此認證來存取AEM的服務存取。 每個客戶管理服務認證的方式和位置都是唯一的。

為簡化起見，本教學課程透過命令列傳入服務認證。 然而，請與您的IT安全團隊合作，瞭解如何根據貴組織的安全方針，儲存及存取這些認證。

1. 複製 [已下載服務認證JSON](#download-service-credentials) 至名為的檔案 `service_token.json` 在專案的根目錄中
   + 記住，永不認可 _任何認證_ 前往Git！

## 使用服務認證

服務憑證是完整格式的JSON物件，與JWT或存取權杖不同。 而是使用服務認證（包含私密金鑰）來產生JWT，再與Adobe IMS API交換存取權杖。

![服務認證 — 外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 從AEM開發人員控制檯下載服務憑證到安全位置
1. 外部應用程式需要以程式設計方式與AEMas a Cloud Service環境互動
1. 外部應用程式會從安全位置讀取服務證明資料
1. 外部應用程式會使用服務憑證中的資訊來建構JWT權杖
1. JWT權杖會傳送至Adobe IMS以交換存取權杖
1. Adobe IMS傳回可用於存取AEMas a Cloud Service的存取權杖
   + 存取權杖無法變更到期時間。
1. 外部應用程式會向AEM發出as a Cloud Service的HTTP請求，將存取權杖作為持有人權杖新增到HTTP請求的授權標頭
1. AEMas a Cloud Service會接收HTTP要求、驗證要求，並執行HTTP要求所要求的工作，然後將HTTP回應傳回給外部應用程式

### 外部應用程式的更新

若要使用「服務認證」存取AEMas a Cloud Service，外部應用程式必須以下列三種方式更新：

1. 讀取服務認證

+ 為簡化起見，服務憑證會從下載的JSON檔案中讀取，但在實際使用情況下，服務憑證必須根據您組織的安全方針安全儲存

1. 從服務憑證產生JWT
1. 交換JWT以取得存取權杖

+ 當存在服務憑證時，外部應用程式在存取AEMas a Cloud Service時會使用此存取權杖，而不是本機開發存取權杖

在本教學課程中，Adobe的 `@adobe/jwt-auth` npm模組可用於兩種：(1)從服務憑證產生JWT，以及(2)在單一函式呼叫中將它交換為存取權杖。 如果您的應用程式不是JavaScript，請檢閱 [其他語言的程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) 瞭解如何從服務憑證建立JWT，並與Adobe IMS交換存取權杖。

## 讀取服務認證

檢閱 `getCommandLineParams()` 請參閱如何使用讀取本機開發存取權杖JSON時所用的相同程式碼來讀取服務認證JSON檔案。

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

讀取服務認證後，即會使用它們產生JWT，然後與Adobe IMS API交換存取權杖。 然後，就可以使用此存取權杖來存取AEMas a Cloud Service。

此範例應用程式以Node.js為基礎，因此最好使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組以利於(1) JWT的產生和(20)與Adobe IMS的交換。 如果您的應用程式是使用其他語言開發的，請檢閱 [適當的程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) ，瞭解如何使用其他程式設計語言來建立向Adobe IMS傳送的HTTP要求。

1. 更新 `getAccessToken(..)` 檢查JSON檔案內容，並判斷其是否代表本機開發存取權杖或服務認證。 這可透過檢查是否存在 `.accessToken` 屬性，僅適用於本機開發存取權杖JSON。

   如果提供服務認證，應用程式會產生JWT並與Adobe IMS交換存取權杖。 使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的 `auth(...)` 函式產生JWT並將其交換為單一函式呼叫中的存取權杖。 引數至 `auth(..)` 方法是 [JSON物件包含特定資訊](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) 可從服務認證JSON取得，如下面的程式碼所述。

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

    現在，視透過該「檔案」命令列引數傳入的JSON檔案（本機開發存取權杖JSON或服務認證JSON）而定，應用程式將會衍生存取權杖。
    
    請記住，雖然服務憑證每365天過期一次，但JWT和對應的存取權杖會頻繁過期，而且需要在過期前重新整理。 使用「refresh_token」[由Adobe IMS提供](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)即可完成此操作。

1. 完成這些變更後，已從AEM開發人員控制檯下載服務認證JSON，且為了簡化，已儲存為 `service_token.json` 在與此相同的資料夾中 `index.js`. 現在，讓我們執行應用程式來取代命令列引數 `file` 替換為 `service_token.json`，並更新 `propertyValue` 變更為新值，因此效果在AEM中更為明顯。

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

   此 __403 — 禁止__ 行，指出對AEMas a Cloud Service的HTTP API呼叫中的錯誤。 嘗試更新資產的中繼資料時，會發生這403個禁止錯誤。

   原因在於服務認證衍生的存取權杖會使用自動建立的技術帳戶AEM使用者（預設情況下只有讀取存取權）來驗證AEM的要求。 若要提供應用程式對AEM的寫入存取權，與存取權杖相關聯的技術帳戶AEM使用者必須在AEM中取得許可權。

## 在AEM中設定存取權

服務憑證衍生的存取權杖使用的技術帳戶AEM User在 __貢獻者__ AEM使用者群組。

![服務認證 — 技術帳戶AEM使用者](./assets/service-credentials/technical-account-user.png)

一旦技術帳戶AEM使用者存在於AEM中（具有存取權杖的第一個HTTP請求之後），就可以像管理其他AEM使用者一樣管理此AEM使用者的許可權。

1. 首先，開啟從AEM開發人員控制檯下載的服務憑證JSON ，找到技術帳戶的AEM登入名稱，然後找到 `integration.email` 值，看起來應類似於： `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 以AEM管理員身分登入對應AEM環境的作者服務
1. 瀏覽至 __工具__ > __安全性__ > __使用者__
1. 找到具有的AEM使用者 __登入名稱__ 已在步驟1中識別，並開啟其 __屬性__
1. 導覽至 __群組__ 標籤，然後新增 __DAM使用者__ 群組（以資產寫入存取權的使用者）
   + [請參閱AEM提供的使用者群組清單](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html#built-in-users-and-groups) 將服務使用者新增至，以獲得最佳許可權。 如果AEM提供的使用者群組都不夠用，請建立您自己的使用者群組，然後新增適當的許可權。
1. 點選 __儲存並關閉__

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

1. 登入已更新的AEMas a Cloud Service環境（使用中提供的相同主機名稱） `aem` 命令列引數)
1. 導覽至 __資產__ > __檔案__
1. 瀏覽至所指定的資產資料夾。 `folder` 命令列引數，例如 __WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟 __屬性__ 資料夾中的任何資產
1. 導覽至 __進階__ 標籤
1. 檢閱更新屬性的值，例如 __版權__ ，已對應至更新的 `metadata/dc:rights` JCR屬性，現在會反映 `propertyValue` 引數，例如 __WKND限制使用__

![WKND受限制的使用中繼資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已使用本機開發存取權杖和產品就緒的服務對服務存取權杖，以程式設計方式存取AEMas a Cloud Service！
