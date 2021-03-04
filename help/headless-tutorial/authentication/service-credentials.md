---
title: 服務憑據
description: 服務AEM認證可用來協助外部應用程式、系統和服務透過HTTP以程式設計方式與AEM Author或Publish服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: 「無頭、整合」
role: 開發人員
level: '"中級，經驗豐富"'
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# 服務憑據

與作為AEMCloud Service的整合必須能夠安全地驗證AEM。 Developer AEM Console授與「服務認證」的存取權，此服務認證可協助外部應用程式、系統和服務透過HTTP以程式設計方式與AEM Author或Publish服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

「服務憑證」可能會以類似的[「本機開發存取Token」(a1/>)顯示，但以下列幾種主要方式不同：](./local-development-access-token.md)

+ 服務憑證是&#x200B;_不是_&#x200B;存取Token，而是用於&#x200B;_取得_&#x200B;存取Token的憑證。
+ 「服務認證」會更為永久（每365天過期），除非撤銷，否則不會變更，而「本機開發存取權杖」則會每日到期。
+ 作為Cloud Service環AEM境的服務認證對應至單一技術AEM帳戶使用者，而本機開發存取Token則會驗AEM證為產生存取Token的使用者。

服務憑證及其產生的存取Token以及本機開發存取Token都應保密，因為這三個Token都可用來存取其個別的AEMCloud Service環境

## 生成服務憑據

生成服務憑據分為兩個步驟：

1. 由AdobeIMS組織管理員進行一次性服務憑證初始化
1. 下載及使用服務認證JSON

### 服務憑據初始化

與本機開發存取Token不同，服務認證需要您的Adobe組織IMS管理員進行一次性初始化&#x200B;_，才能下載。_

![初始化服務憑據](assets/service-credentials/initialize-service-credentials.png)

__這是作為Cloud Service環境的每AEM次一次初始化__

1. 確定您是以AdobeIMS組織的管理員身分登入
1. 登入[Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含作AEM為Cloud Service環境的程式，以整合設定
1. 在&#x200B;__Environments__&#x200B;區段中點選環境旁的省略號，然後選擇&#x200B;__Developer Console__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得服務憑證__&#x200B;按鈕
1. 服務認證將會初始化並顯示為JSON

![開AEM發人員主控台——整合——取得服務認證](./assets/service-credentials/developer-console.png)

一旦初始AEM化Cloud Service環境的「服務認證」後，您的AEMAdobeIMS組織中的其他開發人員就可以下載這些認證。

### 下載服務認證

![下載服務認證](assets/service-credentials/download-service-credentials.png)

下載服務憑據的步驟與初始化步驟相同。 如果尚未進行初始化，則會點選&#x200B;__取得服務憑證__&#x200B;按鈕，顯示使用者錯誤。

1. 確定您是&#x200B;__雲端管理員——開發人員__ IMS產品設定檔的成員(此設定檔授與開發人員主控台AEM的存取權)
   + 沙AEM盒作為Cloud Service環境，只需要&#x200B;__AEM Administrators__&#x200B;或&#x200B;__AEM Users__&#x200B;產品設定檔的會籍
1. 登入[Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含的方AEM案做為Cloud Service環境，以整合
1. 在&#x200B;__Environments__&#x200B;區段中點選環境旁的省略號，然後選擇&#x200B;__Developer Console__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得服務憑證__&#x200B;按鈕
1. 點選左上角的下載按鈕，下載包含「服務認證」值的JSON檔案，並將檔案儲存至安全位置。
   + _如果服務認證受損，請立即聯絡Adobe支援部門，讓其被撤銷_

## 安裝服務憑據

服務憑證提供產生JWT所需的詳細資訊，此JWT會交換為存取Token，用來以Cloud ServiceAEM身分驗證。 服務憑證必須儲存在可供使用其進行存取的外部應用程式、系統或服務存取的安全位置AEM。 每個客戶對服務認證的管理方式和位置都是獨一無二的。

為簡單起見，本教學課程會透過命令列傳遞服務認證，但請與您的IT安全團隊合作，瞭解如何根據貴組織的安全性准則儲存及存取這些認證。

1. 將下載的[服務認證JSON](#download-service-credentials)複製至專案根目錄中名為`service_token.json`的檔案
   + 但請記住，千萬不要向Git提交任何認證！

## 使用服務憑據

服務認證是完整格式的JSON物件，與JWT或存取Token不相同。 而是使用「服務憑證」（包含私密金鑰）來產生JWT，該JWT會與AdobeIMS API交換以取得存取Token。

![服務憑據——外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 從Developer Console下載服務AEM認證至安全位置
1. 外部應用程式需要以程式設計方式與AEMCloud Service環境互動
1. 外部應用程式從安全位置讀取服務憑據
1. 外部應用程式使用服務憑據中的資訊來構建JWT Token
1. JWT Token被發送到AdobeIMS以交換訪問Token
1. AdobeIMS傳回可用來存取的存取Token,AEM做為Cloud Service
   + 存取Token可以要求過期。 最好是讓存取Token的生命週期保持短，並視需要重新整理。
1. 外部應用程式會將HTTP請求當做AEMCloud Service，將存取Token新增為HTTP請求的授權標題中的承載Token
1. 當AEMCloud Service收到HTTP請求時，驗證請求並執行HTTP請求所要求的工作，並將HTTP回應傳回外部應用程式

### 外部應用程式的更新

若要使AEM用「服務認證」以Cloud Service形式存取，外部應用程式必須以3種方式更新：

1. 閱讀服務認證
   + 為簡單起見，我們會從下載的JSON檔案中讀取這些檔案，但是在實際使用案例中，服務認證必鬚根據貴組織的安全性准則安全地儲存
1. 從服務憑據生成JWT
1. 將JWT交換為存取Token
   + 當「服務憑證」存在時，我們的外部應用程式會使用此存取Token，而非本機開發存取Token，以AEMCloud Service形式存取

在本教程中，Adobe的`@adobe/jwt-auth` npm模組用於兩者，(1)從服務憑據生成JWT,(2)在單個函式調用中將其交換為訪問令牌。 如果您的應用程式並非以JavaScript為基礎，請檢閱其他語言的[范常式式碼](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，瞭解如何從「服務認證」建立JWT，並以AdobeIMS交換它以取用Token。

## 閱讀服務認證

請檢閱`getCommandLineParams()`，並查看我們可以使用本機開發存取Token JSON中用來讀取的相同程式碼，在「服務認證JSON」檔案中讀取。

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

## 建立JWT並交換存取Token

一旦讀取服務憑證，它們就用來產生JWT，然後與AdobeIMS API交換以取得存取Token，然後以Cloud ServiceAEM形式存取。

此範例應用程式是以Node.js為基礎，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組來促進(1)JWT產生和(20與AdobeIMS交換)。 如果您的應用程式是使用其他語言開發，請檢閱[適當的程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，以瞭解如何使用其他程式設計語言來建構HTTP要求以AdobeIMS。

1. 更新`getAccessToken(..)`以檢查JSON檔案內容，並判斷其代表本機開發存取Token或服務憑證。 這可透過檢查`.accessToken`屬性是否存在而輕鬆實現，此屬性僅存在於本機開發存取Token JSON。

   如果提供服務認證，應用程式生成JWT，並與AdobeIMS交換該JWT以獲取訪問令牌。 我們將使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函式，此函式會產生JWT，並在單一函式呼叫中將它交換為存取Token。  `auth(..)`的參數是[JSON物件，由「服務認證JSON」中可用的特定資訊](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)組成，如程式碼中所述。

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

   現在，視透過`file`命令列參數傳入的JSON檔案（本機開發存取Token JSON或服務認證JSON）而定，應用程式將衍生存取Token。

   請記住，當服務憑證未過期時，JWT和對應的存取Token會過期，而且必須在過期前重新整理。 這可以通過使用AdobeIMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)提供的`refresh_token`[來完成。

1. 在進行這些變更後，以及從AEMDeveloper Console下載的服務認證JSON（為簡單起見，儲存為`service_token.json`與此`index.js`相同的資料夾），執行應用程式，將命令列參數`file`取代為`service_token.json`，並將`propertyValue`更新為新值，以便效果明顯AEM。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   到終端的輸出如下所示：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行會將HTTP API呼叫中的錯誤指AEM示為Cloud Service。 嘗試更新資產的中繼資料時，會發生這403個「禁止」錯誤。

   原因是服務憑證衍生的存取Token會驗證使用自動建立的技術帳AEM戶使用者的要AEM求，依預設只有讀取存取權。 要向應用程式提供寫訪問權AEM，必須授AEM予與訪問Token關聯的技術帳戶用戶的權AEM限。

## 設定存取權AEM

「服務憑證」衍生的存取Token使用「參與者」使AEM用者群組成員資AEM格的技術帳戶使用者。

![服務認證——技術帳戶使AEM用者](./assets/service-credentials/technical-account-user.png)

一旦技術帳戶AEM使用者存在AEM（第一次使用存取Token的HTTP要求後），就可以像其他使用者AEM一樣管理該使用者的AEM權限。

1. 首先，開啟從Developer AEM Console下載的服務認證JSON，找出技術帳戶的登入名稱，並找出`integration.email`值，其外觀應類似：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以管理員身份登AEM錄到相應環境的Author服AEM務
1. 導覽至「__工具__ > __安全性__ > __使用者__」
1. 找到具AEM有步驟1中標識的&#x200B;__登錄名__&#x200B;的用戶，並開啟其&#x200B;__屬性__
1. 導覽至&#x200B;__群組__&#x200B;標籤，並新增&#x200B;__DAM使用者__&#x200B;群組（以資產的寫入存取權）
1. 點選&#x200B;__儲存並關閉__

使用中授予的技術帳AEM戶對資產擁有寫權限，請重新執行應用程式：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

到終端的輸出如下所示：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 驗證更改

1. 以已更AEM新的Cloud Service環境身份登錄（使用`aem`命令行參數中提供的相同主機名）
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽`folder`命令列參數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟資料夾中任何資產的&#x200B;__屬性__
1. 導覽至&#x200B;__Advanced__&#x200B;標籤
1. 檢視更新屬性的值，例如映射至更新的`metadata/dc:rights` JCR屬性的&#x200B;__Copyright__，現在會反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND限制使用中繼資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已使用本機開AEM發存取Token以及生產就緒型服務對服務存取Token，以程式設計方式存取為Cloud Service!

