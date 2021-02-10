---
title: 服務憑據
description: AEM服務認證可用來協助外部應用程式、系統和服務，以程式設計方式透過HTTP與AEM Author或Publish服務互動。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: 0b1150cd7ca32382cfaa880f9f956b55bfb65a33
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---


# 服務憑據

與AEM的Cloud Service整合必須能夠安全地向AEM驗證。 AEM的Developer Console授與「服務認證」的存取權，這些認證用來協助外部應用程式、系統和服務透過HTTP以程式設計方式與AEM Author或Publish服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

「服務憑證」可能會以類似的[「本機開發存取Token」(a1/>)顯示，但以下列幾種主要方式不同：](./local-development-access-token.md)

+ 服務憑證是&#x200B;_不是_&#x200B;存取Token，而是用於&#x200B;_取得_&#x200B;存取Token的憑證。
+ 「服務認證」會更為永久（每365天過期），除非撤銷，否則不會變更，而「本機開發存取權杖」則會每日到期。
+ AEM的Cloud Service環境服務認證會對應至單一AEM技術帳戶使用者，而本機開發存取Token會驗證為產生存取Token的AEM使用者。

服務憑證及其產生的存取Token，以及本機開發存取Token，都應保持機密，因為這三個Token都可用來以雲端服務環境的身分存取其各自的AEM

## 生成服務憑據

生成服務憑據分為兩個步驟：

1. Adobe IMS組織管理員的一次性服務認證初始化
1. 下載及使用服務認證JSON

### 服務憑據初始化

與本機開發存取Token不同，「服務認證」需要Adobe組織IMS管理員進行一次性初始化&#x200B;_，才能下載。_

![初始化服務憑據](assets/service-credentials/initialize-service-credentials.png)

__這是每個AEM一次初始化，作為雲端服務環境__

1. 確定您是以Adobe IMS組織的管理員身分登入
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM為雲端服務環境的方案，以整合設定
1. 在&#x200B;__Environments__&#x200B;區段中點選環境旁的省略號，然後選擇&#x200B;__Developer Console__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得服務憑證__&#x200B;按鈕
1. 服務認證將會初始化並顯示為JSON

![AEM Developer Console —— 整合——取得服務認證](./assets/service-credentials/developer-console.png)

一旦初始化AEM作為雲端服務環境的「服務認證」後，您Adobe IMS組織中的其他AEM開發人員就可以下載這些認證。

### 下載服務認證

![下載服務認證](assets/service-credentials/download-service-credentials.png)

下載服務憑據的步驟與初始化步驟相同。 如果尚未進行初始化，則會點選&#x200B;__取得服務憑證__&#x200B;按鈕，顯示使用者錯誤。

1. 確定您是&#x200B;__Cloud Manager - Developer__ IMS產品設定檔（授與AEM Developer Console的存取權）的成員
   + 將AEM當做雲端服務環境執行時，只需要&#x200B;__AEM管理員__&#x200B;或&#x200B;__AEM使用者__&#x200B;產品設定檔的會籍
1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 開啟包含AEM做為雲端服務環境的方案，以與
1. 在&#x200B;__Environments__&#x200B;區段中點選環境旁的省略號，然後選擇&#x200B;__Developer Console__
1. 在&#x200B;__Integrations__&#x200B;標籤中點選
1. 點選&#x200B;__取得服務憑證__&#x200B;按鈕
1. 點選左上角的下載按鈕，下載包含「服務認證」值的JSON檔案，並將檔案儲存至安全位置。
   + _如果服務認證受到危害，請立即聯絡Adobe支援以撤銷其認證_

## 安裝服務憑據

「服務認證」提供產生JWT所需的詳細資訊，此JWT會交換為存取Token，用來以AEM作為雲端服務進行驗證。 「服務認證」必須儲存在可供使用其來存取AEM的外部應用程式、系統或服務存取的安全位置。 每個客戶對服務認證的管理方式和位置都是獨一無二的。

為簡單起見，本教學課程會透過命令列傳遞服務認證，但請與您的IT安全團隊合作，瞭解如何根據貴組織的安全性准則儲存及存取這些認證。

1. 將下載的[服務認證JSON](#download-service-credentials)複製至專案根目錄中名為`service_token.json`的檔案
   + 但請記住，千萬不要向Git提交任何認證！

## 使用服務憑據

服務認證是完整格式的JSON物件，與JWT或存取Token不相同。 而是使用「服務認證」（包含私密金鑰）來產生JWT,JWT會與Adobe IMS API交換以取得存取Token。

![服務憑據——外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 從AEM Developer Console下載服務認證至安全位置
1. 外部應用程式需要以程式設計方式與AEM互動，做為雲端服務環境
1. 外部應用程式從安全位置讀取服務憑據
1. 外部應用程式使用服務憑據中的資訊來構建JWT Token
1. JWT Token會傳送至Adobe IMS以交換存取Token
1. Adobe IMS會傳回存取Token，可用來將AEM當做雲端服務存取
   + 存取Token可以要求過期。 最好是讓存取Token的生命週期保持短，並視需要重新整理。
1. 外部應用程式會以雲端服務的形式對AEM進行HTTP要求，並將存取Token新增為HTTP要求的授權標題中的承載Token
1. AEM作為雲端服務，會接收HTTP要求、驗證要求並執行HTTP要求所要求的工作，並將HTTP回應傳回外部應用程式

### 外部應用程式的更新

若要使用「服務認證」存取AEM做為雲端服務，我們的外部應用程式必須以3種方式更新：

1. 閱讀服務認證
   + 為簡單起見，我們會從下載的JSON檔案中讀取這些檔案，但是在實際使用案例中，服務認證必鬚根據貴組織的安全性准則安全地儲存
1. 從服務憑據生成JWT
1. 將JWT交換為存取Token
   + 當「服務認證」存在時，當以雲端服務的身分存取AEM時，我們的外部應用程式會使用此存取Token，而非本機開發存取Token

在本教學課程中，Adobe的`@adobe/jwt-auth` npm模組會用於兩者，(1)從「服務認證」產生JWT，並(2)在單一函式呼叫中以存取Token交換JWT。 如果您的應用程式並非以JavaScript為基礎，請檢閱其他語言的[范常式式碼](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，瞭解如何從「服務認證」建立JWT，並與Adobe IMS交換它以取用Token。

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

一旦讀取「服務認證」，就會使用它們產生JWT，然後與Adobe IMS API交換以取得存取Token，接著再以雲端服務的形式存取AEM。

此範例應用程式是以Node.js為基礎，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組來協助(1)產生JWT並（20與Adobe IMS交換）。 如果您的應用程式是使用其他語言開發，請檢閱[適當的程式碼範例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，瞭解如何使用其他程式設計語言來建構Adobe IMS的HTTP要求。

1. 更新`getAccessToken(..)`以檢查JSON檔案內容，並判斷其代表本機開發存取Token或服務憑證。 這可透過檢查`.accessToken`屬性是否存在而輕鬆實現，此屬性僅存在於本機開發存取Token JSON。

   如果提供「服務認證」，應用程式會產生JWT，並與Adobe IMS交換JWT以取得存取Token。 我們將使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函式，此函式會產生JWT，並在單一函式呼叫中將它交換為存取Token。  `auth(..)`的參數是[JSON物件，由「服務認證JSON」中可用的特定資訊](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)組成，如程式碼中所述。

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

   請記住，當服務憑證未過期時，JWT和對應的存取Token會過期，而且必須在過期前重新整理。 這可使用Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)提供的`refresh_token` [來完成。

1. 在進行這些變更後，以及從AEM Developer Console下載的「服務認證JSON」（而且為簡單起見，會儲存為`service_token.json`與此`index.js`相同的檔案夾），執行應用程式，將命令列參數`file`取代為`service_token.json`，並將`propertyValue`更新為新值，以便在AEM中顯現效果。

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

   __403 - Forbidden__&#x200B;行會指出AEM的HTTP API呼叫中的錯誤，稱為雲端服務。 嘗試更新資產的中繼資料時，會發生這403個「禁止」錯誤。

   原因是「服務憑證衍生的存取Token」會使用自動建立的技術帳戶AEM使用者來驗證對AEM的要求，依預設，該使用者僅具有讀取存取權。 若要提供AEM的應用程式寫入存取權，必須在AEM中授與存取Token相關的技術帳戶AEM使用者權限。

## 在AEM中設定存取權

「服務憑證衍生的存取Token」會使用技術帳戶AEM使用者，該使用者具有「參與者AEM」使用者群組的會籍。

![服務認證——技術帳戶AEM使用者](./assets/service-credentials/technical-account-user.png)

一旦技術帳戶AEM使用者存在於AEM中（第一個具有存取Token的HTTP要求之後），就可以像管理其他AEM使用者一樣管理此AEM使用者的權限。

1. 首先，開啟從AEM Developer Console下載的「服務認證JSON」，找出技術帳戶的AEM登入名稱，並找出`integration.email`值，其外觀應類似：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以AEM管理員身分登入對應AEM環境的「作者」服務
1. 導覽至「__工具__ > __安全性__ > __使用者__」
1. 找出具有步驟1中識別之&#x200B;__登入名稱__&#x200B;的AEM使用者，並開啟其&#x200B;__屬性__
1. 導覽至&#x200B;__群組__&#x200B;標籤，並新增&#x200B;__DAM使用者__&#x200B;群組（以資產的寫入存取權）
1. 點選&#x200B;__儲存並關閉__

在AEM中授權技術帳戶擁有資產的寫入權限後，請重新執行應用程式：

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

1. 以已更新的雲端服務環境身分登入AEM（使用`aem`命令列參數中提供的相同主機名稱）
1. 導覽至&#x200B;__Assets__ > __Files__
1. 導覽`folder`命令列參數指定的資產資料夾，例如&#x200B;__WKND__ > __英文__ > __冒險__ > __納帕品酒會__
1. 開啟資料夾中任何資產的&#x200B;__屬性__
1. 導覽至&#x200B;__Advanced__&#x200B;標籤
1. 檢視更新屬性的值，例如映射至更新的`metadata/dc:rights` JCR屬性的&#x200B;__Copyright__，現在會反映`propertyValue`參數中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND限制使用中繼資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已透過程式設計方式，使用本機開發存取Token以及生產就緒服務對服務存取Token，將AEM存取為雲端服務！

