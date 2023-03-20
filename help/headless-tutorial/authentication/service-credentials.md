---
title: 服務憑據
description: 了解如何使用可促進外部應用程式、系統和服務，以程式設計方式與作者或透過HTTP發佈服務互動的服務憑證。
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
source-git-commit: 1401710c19ae6ee6a2822ae06286bef4f92cda45
workflow-type: tm+mt
source-wordcount: '1937'
ht-degree: 0%

---

# 服務憑據

與Adobe Experience Manager(AEM)as a Cloud Service的整合必須能夠安全地驗證AEM服務。 AEM Developer Console授與服務憑證的存取權，這些憑證可協助外部應用程式、系統和服務以程式設計方式與AEM製作或透過HTTP發佈服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服務憑據可能顯示類似 [本機開發存取權杖](./local-development-access-token.md) 但在幾個關鍵方面卻有所不同：

+ 服務憑據與技術帳戶相關聯。 一個技術帳戶可以有多個服務憑據。
+ 服務憑據為 _not_ 存取權杖，而非用來 _取得_ 存取權杖。
+ 服務憑證會更為永久（其憑證每365天過期），除非撤銷，否則不會變更，但本機開發存取權杖會每天過期。
+ AEMas a Cloud Service環境的服務憑證對應至單一AEM技術帳戶使用者，而本機開發存取權杖會驗證為產生存取權杖的AEM使用者。
+ AEMas a Cloud Service環境最多可以有10個技術帳戶，每個帳戶都有其自己的服務憑證，每個帳戶都對應至個別的技術帳戶AEM使用者。

服務憑證及其產生的存取權杖，以及本機開發存取權杖，都應保持機密。 因為這三者皆可用來取得，所以可存取其各自的AEMas a Cloud Service環境。

## 生成服務憑據

服務憑證產生分為兩個步驟：

1. 由Adobe IMS組織管理員建立一次性技術帳戶
1. 下載及使用技術帳戶的服務憑證JSON

### 建立技術帳戶

與本機開發存取權杖不同，服務憑證需先由Adobe組織IMS管理員建立技術帳戶，才能下載。 應為需要程式化存取AEM的每個用戶端建立獨立技術帳戶。

![建立技術帳戶](assets/service-credentials/initialize-service-credentials.png)

技術帳戶建立一次，但隨著時間推移，可用於管理與技術帳戶相關聯的服務憑證的私密金鑰也可管理。 例如，必須在當前私鑰過期之前生成新的私鑰/服務憑據，以便用戶能夠不間斷地訪問服務憑據。

1. 請確定您是以下列方式登入：
   + __Adobe IMS組織的管理員__
   + 會員 __AEM管理員__ IMS產品設定檔位於 __AEM作者__
1. 登入 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEMas a Cloud Service環境的程式，以整合以
1. 點選 __環境__ 部分，然後選擇 __開發人員控制台__
1. 點選 __整合__ 標籤
1. 點選 __技術帳戶__ 標籤
1. 點選 __建立新技術帳戶__ 按鈕
1. 技術帳戶的服務憑證會初始化，並顯示為JSON

![AEM開發人員主控台 — 整合 — 取得服務憑證](./assets/service-credentials/developer-console.png)

初始化AEM作為Cloud Service環境的服務憑證後，您Adobe IMS組織中的其他AEM開發人員可以下載。

### 下載服務憑據

![下載服務憑據](assets/service-credentials/download-service-credentials.png)

下載服務憑證遵循與初始化類似的步驟。

1. 請確定您是以下列方式登入：
   + __Adobe IMS組織的管理員__
   + 會員 __AEM管理員__ IMS產品設定檔位於 __AEM作者__
1. 登入 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEMas a Cloud Service環境的程式以與
1. 點選 __環境__ 部分，然後選擇 __開發人員控制台__
1. 點選 __整合__ 標籤
1. 點選 __技術帳戶__ 標籤
1. 展開 __技術帳戶__ 使用
1. 展開 __私密金鑰__ 將下載其服務憑據，並確認狀態為 __作用中__
1. 點選 __...__ > __檢視__ 與 __私密金鑰__，其中顯示服務憑證JSON
1. 點選左上角的下載按鈕，下載包含「服務憑證」值的JSON檔案，並將檔案儲存至安全位置

## 安裝服務憑據

服務憑證提供產生JWT所需的詳細資訊，JWT會交換以取得存取權杖，以便透過AEMas a Cloud Service驗證。 服務憑證必須儲存在安全位置，外部應用程式、系統或服務可使用該位置來存取AEM。 每個客戶管理服務憑證的方式和位置都是唯一的。

為了簡單起見，本教學課程會透過命令列傳遞中的服務憑證。 不過，請與您的IT安全性團隊合作，了解如何根據貴組織的安全性准則儲存及存取這些憑證。

1. 複製 [已下載服務憑證JSON](#download-service-credentials) 檔案名為 `service_token.json` 在項目根目錄中
   + 記住，永遠不要提交 _任何憑據_ Git!

## 使用服務憑據

服務憑證（完整格式的JSON物件）與JWT或存取權杖不同。 系統會改用服務憑證（包含私密金鑰）來產生JWT，並與Adobe IMS API交換以取得存取權杖。

![服務憑據 — 外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 從AEM Developer Console下載服務憑證至安全位置
1. 外部應用程式需要以程式設計方式與AEMas a Cloud Service環境互動
1. 外部應用程式從安全位置讀取服務憑據
1. 外部應用程式使用服務憑證中的資訊來建構JWT代號
1. JWT代號會傳送至Adobe IMS以交換存取代號
1. Adobe IMS傳回存取Token，可用來存取AEMas a Cloud Service
   + 存取權杖可以要求過期。 最好將存取權杖的生命週期縮短，並視需要重新整理。
1. 外部應用程式會對AEM發出HTTP要求，並將存取權杖新增為HTTP要求的授權標題中的承載權杖
1. AEMas a Cloud Service會接收HTTP要求、驗證要求，並執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 外部應用程式的更新

若要使用服務憑證存取AEMas a Cloud Service，外部應用程式必須以三種方式更新：

1. 閱讀服務憑據

+ 為了簡單起見，服務憑證會從下載的JSON檔案中讀取，但在實際使用案例中，服務憑證必須依照貴組織的安全性准則安全地儲存

1. 從服務憑證產生JWT
1. 將JWT交換為存取權杖

+ 存取AEMas a Cloud Service時，外部應用程式會使用此存取權杖，而非本機開發存取權杖

在本教學課程中，Adobe `@adobe/jwt-auth` npm模組用於兩者，(1)從服務憑證產生JWT，以及(2)以單一函式呼叫將其交換為存取權杖。 如果您的應用程式不是以JavaScript為基礎，請檢閱 [其他語言的范常式式碼](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) 以了解如何從服務憑證建立JWT，並與Adobe IMS交換以取得存取權杖。

## 閱讀服務憑據

檢閱 `getCommandLineParams()` 請參閱使用本機開發存取權杖JSON中用來讀取的相同程式碼來讀取服務憑證JSON檔案的方式。

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

讀取服務憑證後，系統會使用這些憑證產生JWT，然後與Adobe IMS API交換以取得存取權杖。 然後，此存取權杖便可用來存取AEMas a Cloud Service。

此範例應用程式以Node.js為基礎，因此最好使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組，以方便(1)產生JWT，以及(20)與Adobe IMS交換。 如果您的應用程式是使用其他語言開發的，請查看 [適當的程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) 了解如何使用其他程式設計語言，向Adobe IMS建構HTTP要求。

1. 更新 `getAccessToken(..)` 檢查JSON檔案內容，並判斷其代表本機開發存取權杖或服務憑證。 這可借由檢查 `.accessToken` 屬性，僅存在於本機開發存取權杖JSON。

   若已提供服務憑證，應用程式會產生JWT並與Adobe IMS交換JWT以取得存取權杖。 使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` 函式，其產生JWT並在單一函式呼叫中將其交換為存取權杖。 參數 `auth(..)` 方法是 [JSON物件，由特定資訊組成](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) 可從服務憑證JSON取得，如下文的程式碼中所述。

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

    現在，根據透過&#39;file&#39;命令列參數傳入的JSON檔案（本機開發存取權杖JSON或服務憑證JSON），應用程式將衍生存取權杖。
    
    請記住，雖然服務憑證每365天過期，但JWT和對應的存取權杖會經常過期，且必須在過期前重新整理。 您可以使用「refresh_token」[由Adobe IMS提供](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)來完成此操作。

1. 完成這些變更後，服務憑證JSON便從AEM開發人員控制台下載，且為簡單起見，另存新檔 `service_token.json` 與此資料夾 `index.js`. 現在，讓我們執行替換命令行參數的應用程式 `file` with `service_token.json`，以及更新 `propertyValue` 新值，以便在AEM中顯現效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   輸出到終端的內容如下：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   此 __403 — 禁止__ 行，表示對AEMas a Cloud Service的HTTP API呼叫中發生錯誤。 嘗試更新資產的中繼資料時，會發生這些403禁止錯誤。

   原因在於服務憑證衍生的存取權杖，會使用自動建立的技術帳戶AEM使用者驗證向AEM的要求，而依預設，該使用者僅具有讀取存取權。 若要提供AEM的應用程式寫入存取權，與存取權杖相關聯的技術帳戶AEM使用者必須在AEM中獲得權限。

## 在AEM中設定存取權

服務憑證衍生的存取權杖使用的是具有 __貢獻者__ AEM使用者群組。

![服務憑證 — 技術帳戶AEM使用者](./assets/service-credentials/technical-account-user.png)

一旦AEM中存在技術帳戶AEM使用者（在具有存取權杖的第一個HTTP要求之後），此AEM使用者的權限便可與其他AEM使用者管理相同。

1. 首先，開啟從AEM開發人員控制台下載的服務憑證JSON，找出技術帳戶的AEM登入名稱，然後找出 `integration.email` 值，看起來應類似： `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 以AEM管理員身分登入對應AEM環境的製作服務
1. 導覽至 __工具__ > __安全性__ > __使用者__
1. 找出AEM使用者，並搭配 __登入名稱__ 在步驟1中識別，並開啟 __屬性__
1. 導覽至 __群組__ ，然後新增 __DAM使用者__ 群組（以寫入資產的方式存取）
1. 點選 __儲存並關閉__

在AEM中允許技術帳戶擁有資產寫入權限的情況下，重新執行應用程式：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

輸出到終端的內容如下：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 驗證變更

1. 登入已更新的AEMas a Cloud Service環境(使用 `aem` 命令行參數)
1. 導覽至 __資產__ > __檔案__
1. 導覽至 `folder` 命令列參數，例如 __WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟 __屬性__ 針對資料夾中的任何資產
1. 導覽至 __進階__ 標籤
1. 檢閱已更新屬性的值，例如 __版權__ 已對應至已更新 `metadata/dc:rights` JCR屬性，現在會反映 `propertyValue` 參數，例如 __WKND限制使用__

![WKND限制使用元資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已透過本機開發存取權杖，以及生產就緒服務對服務存取權杖，以程式設計方式存取AEMas a Cloud Service!
