---
title: 服務憑據
description: AEM服務認證可協助外部應用程式、系統和服務以程式設計方式與AEM製作或透過HTTP發佈服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: 無頭式整合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1827'
ht-degree: 0%

---


# 服務憑據

與AEM as aCloud Service的整合必須能夠安全地驗證AEM。 AEM Developer Console授與服務憑證的存取權，這些憑證可協助外部應用程式、系統和服務以程式設計方式與AEM製作或透過HTTP發佈服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服務憑證可能會出現類似的[本機開發存取權杖](./local-development-access-token.md) ，但有幾種主要方式不同：

+ 服務憑證是&#x200B;_不是_&#x200B;存取權杖，而是用於&#x200B;_取得_&#x200B;存取權杖的憑證。
+ 服務憑證會更為永久（每365天過期），除非撤銷，否則不會變更，而本機開發存取權杖會每天過期。
+ 作為Cloud Service環境的AEM的服務憑證對應至單一AEM技術帳戶使用者，而本機開發存取權杖則驗證為產生存取權杖的AEM使用者。

服務憑證及其產生的存取權杖，以及本機開發存取權杖，都應保持機密，因為這三個權杖都可用來取得存取其作為Cloud Service環境的AEM

## 生成服務憑據

服務憑證產生分為兩個步驟：

1. 由AdobeIMS組織管理員初始化一次性服務憑證
1. 下載及使用服務憑證JSON

### 服務憑據初始化

與本機開發存取權杖不同，Adobe組織IMS管理員必須先執行&#x200B;_一次性初始化_，服務憑證才能下載。

![初始化服務憑據](assets/service-credentials/initialize-service-credentials.png)

__這是每個AEM作為Cloud Service環境的一次性初始化__

1. 確認您是以AdobeIMS組織的管理員身分登入
1. 登入[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM作為Cloud Service環境的程式，以整合
1. 點選&#x200B;__Environments__&#x200B;區段中環境旁的省略號，然後選取&#x200B;__Developer Console__
1. 點選&#x200B;__整合__&#x200B;標籤
1. 點選「__取得服務憑證__」按鈕
1. 服務憑證將會初始化，並顯示為JSON

![AEM開發人員主控台 — 整合 — 取得服務憑證](./assets/service-credentials/developer-console.png)

初始化AEM作為Cloud Service環境的服務憑證後，您AdobeIMS組織中的其他AEM開發人員可以下載。

### 下載服務憑據

![下載服務憑據](assets/service-credentials/download-service-credentials.png)

下載服務憑據的步驟與初始化步驟相同。 如果尚未進行初始化，則點選&#x200B;__Get Service Credentials__&#x200B;按鈕時將顯示錯誤。

1. 確認您是&#x200B;__Cloud Manager - Developer__ IMS產品設定檔的成員(授與AEM Developer Console的存取權)
   + 將AEM作為Cloud Service環境時，只需要&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__&#x200B;產品設定檔的成員資格
1. 登入[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM作為Cloud Service環境的程式，以便與
1. 點選&#x200B;__Environments__&#x200B;區段中環境旁的省略號，然後選取&#x200B;__Developer Console__
1. 點選&#x200B;__整合__&#x200B;標籤
1. 點選「__取得服務憑證__」按鈕
1. 點選左上角的下載按鈕，下載包含「服務憑證」值的JSON檔案，並將檔案儲存至安全位置。
   + _如果服務憑證遭到破壞，請立即聯絡Adobe支援，要求撤銷這些憑證_

## 安裝服務憑據

服務憑證提供產生JWT所需的詳細資訊，JWT會交換來取得存取權杖，以便以AEM作為Cloud Service進行驗證。 服務憑證必須儲存在安全位置，外部應用程式、系統或服務可使用該位置來存取AEM。 每個客戶管理服務憑證的方式和位置將是唯一的。

為了簡單起見，本教學課程會透過命令列傳遞中的服務憑證，但請與IT安全團隊合作，了解如何根據貴組織的安全性准則儲存和存取這些憑證。

1. 將下載的服務憑證JSON](#download-service-credentials)複製到專案根目錄中名為`service_token.json`的檔案[
   + 但請記住，絕不要向Git提交任何憑證！

## 使用服務憑據

服務憑證（完整格式的JSON物件）與JWT或存取權杖不同。 而是使用服務憑證（包含私密金鑰）來產生JWT，並與AdobeIMS API交換以取得存取權杖。

![服務憑據 — 外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 從AEM Developer Console下載服務憑證至安全位置
1. 外部應用程式需要以程式設計方式與AEM互動，作為Cloud Service環境
1. 外部應用程式從安全位置讀取服務憑據
1. 外部應用程式使用服務憑證中的資訊來建構JWT代號
1. JWT代號會傳送至AdobeIMS以交換存取代號
1. AdobeIMS會傳回存取權杖，可用來以Cloud Service形式存取AEM
   + 存取權杖可以要求過期。 最好將存取權杖的生命週期縮短，並視需要重新整理。
1. 外部應用程式會以Cloud Service的形式向AEM提出HTTP要求，並將存取權杖新增為HTTP要求的授權標題中的承載權杖
1. AEM as aCloud Service會接收HTTP要求、驗證要求、執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 外部應用程式的更新

若要使用服務憑證存取AEM as aCloud Service，外部應用程式必須以3種方式更新：

1. 閱讀服務憑據
   + 為了簡單起見，我們會從下載的JSON檔案中閱讀這些檔案，但在實際使用案例中，服務憑證必須依照貴組織的安全性准則安全地儲存
1. 從服務憑證產生JWT
1. 將JWT交換為存取權杖
   + 存取AEM作為Cloud Service時，我們的外部應用程式會使用此存取權杖，而非本機開發存取權杖

在本教學課程中，Adobe的`@adobe/jwt-auth` npm模組可用於兩者，(1)從服務憑證產生JWT，以及(2)在單一函式呼叫中以存取權杖的形式交換JWT。 如果您的應用程式不是以JavaScript為基礎，請檢閱其他語言的[范常式式碼](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，了解如何從服務憑證建立JWT，並與AdobeIMS交換JWT以取得存取權杖。

## 閱讀服務憑據

檢閱`getCommandLineParams()`，並查看我們可以使用本機開發存取權杖JSON中用來讀取的相同程式碼，讀取服務憑證JSON檔案。

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

讀取服務憑證後，會用來產生JWT，然後與AdobeIMS API交換以取得存取權杖，接著再以Cloud Service存取AEM。

此範例應用程式以Node.js為基礎，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組來促進(1)JWT產生和(20)與AdobeIMS交換。 如果您的應用程式是使用其他語言開發的，請查看[適當的程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，以了解如何使用其他程式設計語言來建構HTTP要求以AdobeIMS。

1. 更新`getAccessToken(..)`以檢查JSON檔案內容，並判斷其是否代表本機開發存取權杖或服務憑證。 檢查`.accessToken`屬性是否存在，以輕鬆達成此目標，此屬性僅存於本機開發存取權杖JSON。

   若已提供服務憑證，應用程式會產生JWT並與AdobeIMS交換以取得存取權杖。 我們將使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函式，此函式會產生JWT，並在單一函式呼叫中交換JWT以取得存取權杖。  `auth(..)`的參數是[JSON物件，由服務憑證JSON提供的特定資訊](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)組成，如下列程式碼所述。

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

   現在，根據透過`file`命令列參數傳入的JSON檔案（本機開發存取權杖JSON或服務憑證JSON），應用程式將衍生存取權杖。

   請記住，雖然服務憑證不會過期，但JWT和對應的存取權杖會過期，且必須在過期前重新整理。 您可以使用AdobeIMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)提供的`refresh_token` [來完成此操作。

1. 完成這些更改後，從AEM Developer Console下載的服務憑據JSON（為了簡單起見，將`service_token.json`保存為與此`index.js`相同的資料夾），執行將命令行參數`file`替換為`service_token.json`的應用程式，並將`propertyValue`更新為新值，以便在AEM中顯現效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   輸出至終端的內容如下：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行，表示以Cloud Service形式向AEM發出的HTTP API呼叫中發生錯誤。 嘗試更新資產的中繼資料時，會發生這些403禁止錯誤。

   原因在於服務憑證衍生的存取權杖，會使用自動建立的技術帳戶AEM使用者驗證向AEM的要求，而依預設，該使用者僅具有讀取存取權。 若要提供AEM的應用程式寫入存取權，與存取權杖相關聯的技術帳戶AEM使用者必須在AEM中獲得權限。

## 在AEM中設定存取權

服務憑證衍生的存取權杖使用的技術帳戶是AEM使用者，其成員是貢獻者AEM使用者群組的成員。

![服務憑證 — 技術帳戶AEM使用者](./assets/service-credentials/technical-account-user.png)

一旦AEM中存在技術帳戶AEM使用者（在具有存取權杖的第一個HTTP要求之後），此AEM使用者的權限便可與其他AEM使用者管理相同。

1. 首先，開啟從AEM開發人員控制台下載的服務憑證JSON，找出技術帳戶的AEM登入名稱，然後找出`integration.email`值，其看起來應類似：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以AEM管理員身分登入對應AEM環境的製作服務
1. 導覽至&#x200B;__工具__ > __安全性__ > __使用者__
1. 找到步驟1中識別的&#x200B;__登入名稱__&#x200B;的AEM使用者，並開啟其&#x200B;__屬性__
1. 導覽至&#x200B;__群組__&#x200B;標籤，然後新增&#x200B;__DAM使用者__&#x200B;群組（以資產的寫入存取權）
1. 點選&#x200B;__儲存並關閉__

在AEM中擁有技術帳戶權限以擁有資產的寫入權限後，請重新執行應用程式：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

輸出至終端的內容如下：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 驗證變更

1. 以已更新的Cloud Service環境登入AEM（使用`aem`命令列參數中提供的相同主機名稱）
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽至`folder`命令列參數所指定的資產資料夾，例如&#x200B;__WKND__ > __English__ > __Adventures__ > __Napa品酒會__
1. 開啟資料夾中任何資產的&#x200B;__屬性__
1. 導覽至&#x200B;__Advanced__&#x200B;標籤
1. 查看更新屬性的值，例如映射到更新的`metadata/dc:rights` JCR屬性的&#x200B;__Copyright__，該值現在反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND限制使用元資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已透過本機開發存取權杖，以及生產就緒服務對服務存取權杖，以程式設計方式存取AEM作為Cloud Service!

