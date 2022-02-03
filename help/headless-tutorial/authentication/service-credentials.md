---
title: 服務憑據
description: 服AEM務憑據用於幫助外部應用程式、系統和服務以寫程式方式與AEM作者或通過HTTP發佈服務交互。
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
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: ef4579a44c1c940a3b7441e336db3790a0c7afd7
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 0%

---

# 服務憑據

與AEMas a Cloud Service的整合必須能夠安全地驗證到AEM。 開發AEM人員控制台授予對服務憑據的訪問權限，該服務憑據用於幫助外部應用程式、系統和服務以寫程式方式與AEM作者或通過HTTP發佈服務進行交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服務憑據可能顯示類似 [本地開發訪問令牌](./local-development-access-token.md) 但在幾個關鍵方面卻有所不同：

+ 服務憑據為 _不_ 訪問令牌，而是用於 _獲取_ 訪問令牌。
+ 服務憑據更為永久（每365天過期一次），除非撤消，否則不會更改，而本地開發訪問令牌每天過期。
+ as a Cloud Service環境的AEM服務憑據映射到AEM單個技術帳戶用戶，而本地開發訪問令牌作為生成訪問令牌AEM的用戶進行身份驗證。
+ 一個AEMas a Cloud Service環境有一個映射到一個技術帳戶用戶的服AEM務憑據。 服務憑據不能用於與不同技術帳戶用戶AEM在同一as a Cloud Service環境中進行AEM身份驗證。

服務憑據和它們生成的訪問令牌以及本地開發訪問令牌都應保密，因為這三個令牌都可用於訪問其各自的AEMas a Cloud Service環境

## 生成服務憑據

服務憑據生成分為兩個步驟：

1. 由Adobe IMS組織管理員進行的一次性服務憑據初始化
1. 服務憑據JSON的下載和使用

### 服務憑據初始化

與本地開發訪問令牌不同，服務憑據需要 _一次性初始化_ 由Adobe組織IMS管理員下載。

![初始化服務憑據](assets/service-credentials/initialize-service-credentials.png)

__這是每個as a Cloud Service環境一次性AEM初始化__

1. 確保您登錄身份：
   + Adobe IMS組織的管理員
   + 成員 __雲管理器 — 開發人員__ IMS產品配置檔案
   + 成員 __用AEM戶__ 或 __管AEM理員__ IMS產品配置檔案 __AEM作者__
1. 登錄到 [Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含as a Cloud ServiceAEM環境的程式，以整合為
1. 點擊中環境旁邊的省略號 __環境__ ，然後選擇 __開發人員控制台__
1. 點擊 __整合__ 頁籤
1. 點擊 __獲取服務憑據__ 按鈕
1. 服務憑據將被初始化並顯示為JSON

![開AEM發人員控制台 — 整合 — 獲取服務憑據](./assets/service-credentials/developer-console.png)

一旦作AEM為Cloud Service環境的服務憑據已初始化，Adobe IMS組AEM織中的其他開發人員可以下載這些憑據。

### 下載服務憑據

![下載服務憑據](assets/service-credentials/download-service-credentials.png)

下載服務憑據的步驟與初始化步驟相同。 如果尚未初始化，則用戶將在點擊 __獲取服務憑據__ 按鈕

1. 確保您以以下方式登錄：
   + 成員 __雲管理器 — 開發人員__ IMS產品配置檔案(允許訪問開發AEM人員控制台)
      + 沙AEM盒as a Cloud Service環境不需要  __雲管理器 — 開發人員__ 成員
   + 成員 __用AEM戶__ 或 __管AEM理員__ IMS產品配置檔案 __AEM作者__
1. 登錄到 [Adobe雲管理器](https://my.cloudmanager.adobe.com)
1. 開啟包含要與整合AEM的as a Cloud Service環境的程式
1. 點擊中環境旁邊的省略號 __環境__ ，然後選擇 __開發人員控制台__
1. 點擊 __整合__ 頁籤
1. 點擊 __獲取服務憑據__ 按鈕
1. 點擊左上角的下載按鈕，下載包含「服務憑據」值的JSON檔案，並將檔案保存到安全位置。
   + _如果服務憑據受損，請立即與Adobe支援聯繫以撤銷服務憑據_

## 安裝服務憑據

服務憑據提供生成JWT所需的詳細資訊，JWT用訪問令牌交換，用於通過AEMas a Cloud Service進行驗證。 服務憑據必須儲存在外部應用程式、系統或服務可訪問的安全位置AEM。 對服務憑據的管理方式和管理位置將是每個客戶的唯一選項。

為簡單起見，本教程通過命令行將服務憑據傳遞給您，但是，請與您的IT安全團隊協作，瞭解如何根據您組織的安全准則儲存和訪問這些憑據。

1. 複製 [已下載服務憑據JSON](#download-service-credentials) 到名為 `service_token.json` 在項目的根部
   + 但請記住，千萬不要向Git提交任何憑據！

## 使用服務憑據

服務憑據（完全形式的JSON對象）與JWT和訪問令牌不相同。 而是使用服務憑據（包含私鑰）來生成JWT，該JWT與Adobe IMS API交換以獲取訪問令牌。

![服務憑據 — 外部應用程式](assets/service-credentials/service-credentials-external-application.png)

1. 將服務憑據從開發AEM人員控制台下載到安全位置
1. 外部應用程式需要以寫程式方式與as a Cloud Service環境AEM交互
1. 外部應用程式從安全位置讀取服務憑據
1. 外部應用程式使用來自服務憑據的資訊來構造JWT令牌
1. JWT令牌被發送到Adobe IMS以交換訪問令牌
1. Adobe IMS返回可用於訪問as a Cloud Service的訪問令牌AEM
   + 訪問令牌可以請求到期。 最好保持訪問令牌的生命週期短，並在需要時刷新。
1. 外部應用程式將HTTP請求AEMas a Cloud Service，將訪問令牌作為承載令牌添加到HTTP請求的授權標頭
1. AEMas a Cloud Service接收HTTP請求，驗證請求，並執行HTTP請求所請求的工作，並將HTTP響應返回到外部應用程式

### 對外部應用程式的更新

要使用AEM服務憑據訪問as a Cloud Service，我們的外部應用程式必須通過三種方式進行更新：

1. 在服務憑據中讀取
   + 為簡單起見，我們將從下載的JSON檔案中讀取這些內容，但是在實際使用情形中，必鬚根據您組織的安全准則安全地儲存服務憑據
1. 從服務憑據生成JWT
1. 將JWT交換為訪問令牌
   + 當存在服務憑據時，外部應用程式在訪問as a Cloud Service時使用此訪問令牌而不是本地開發訪問令牌AEM

在本教程中，Adobe `@adobe/jwt-auth` npm模組用於兩者，(1)從服務憑據生成JWT,(2)在單個函式調用中將其交換為訪問令牌。 如果應用程式不基於JavaScript，請查看 [其他語言的示例代碼](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) 如何從服務憑據建立JWT，並將其與Adobe IMS交換為訪問令牌。

## 閱讀服務憑據

查看 `getCommandLineParams()` 並查看我們可以使用本地開發訪問令牌JSON中用於讀取的相同代碼來讀取服務憑據JSON檔案。

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

## 建立JWT並交換訪問令牌

一旦讀取了服務憑據，它們就用於生成JWT，然後與Adobe IMS API交換該JWT以獲得訪問令牌，然後該訪問令牌可用於訪AEM問as a Cloud Service。

此示例應用程式基於Node.js，因此最好使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模組，用於促進(1)JWT的生成和(20)與Adobe IMS的交換。 如果您的應用程式是使用其他語言開發的，請查看 [適當的代碼樣本](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) 如何使用其他寫程式語言構建Adobe IMS的HTTP請求。

1. 更新 `getAccessToken(..)` 檢查JSON檔案內容，並確定它是否表示本地開發訪問令牌或服務憑據。 通過檢查是否存在 `.accessToken` 屬性，它只存在於本地開發訪問令牌JSON。

   如果提供了「服務憑據」，則應用程式生成JWT並與Adobe IMS交換JWT以獲取訪問令牌。 我們用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)`s `auth(...)` 函式，該函式生成JWT並在單個函式調用中將其交換為訪問令牌。  到的參數 `auth(..)` 是 [由特定資訊組成的JSON對象](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) 可從服務憑據JSON獲得，如代碼中的下面所述。

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

   現在，根據通過該檔案傳入的JSON檔案（本地開發訪問令牌JSON或服務憑據JSON） `file` 命令行參數，應用程式將派生訪問令牌。

   請記住，儘管服務憑據每365天過期一次，但JWT和相應的訪問令牌經常過期，並且需要在過期之前刷新。 這可以通過使用 `refresh_token` [Adobe IMS提供](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)。

1. 隨著這些更改的到位，並且從開發人員控制台下載的AEM服務憑據JSON(為簡單起見，另存為 `service_token.json` 與此資料夾相同 `index.js`)，執行應用程式以替換命令行參數 `file` 與 `service_token.json`，並更新 `propertyValue` 到新值，因此效果在中顯AEM著。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   到終端的輸出將如下所示：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   的 __403 — 禁止__ 行，指示HTTP API調用到AEMas a Cloud Service時出錯 嘗試更新資產的元資料時會出現這些403禁止錯誤。

   原因是服務憑據派生的訪問令牌使用自動建立的技術帳戶AEM用戶驗證請求，AEM預設情況下，該用戶只具有讀訪問權限。 要向應用程式提供寫訪問權AEM限，必AEM須授予與訪問令牌關聯的技術帳戶用戶的AEM權限。

## 在中配置訪AEM問

服務憑據派生的訪問令牌使用在「AEM參與者」用戶組中具有成AEM員資格的技術帳戶用戶。

![服務憑據 — 技術帳戶用AEM戶](./assets/service-credentials/technical-account-user.png)

一旦技術帳AEM戶用戶存AEM在（第一個具有訪問令牌的HTTP請求後），則該AEM用戶的權限可以與其他用戶AEM相同。

1. 首先，開啟從開發人員控制台AEM下載的服務憑據JSON，找到技術帳戶的登AEM錄名，然後找到 `integration.email` 值，它應看起來類似： `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以管理員身份登AEM錄到相應環境的Author服AEM務
1. 導航到 __工具__ > __安全__ > __用戶__
1. 找到AEM用戶 __登錄名__ 在步驟1中標識，並開啟 __屬性__
1. 導航到 __組__ ，然後添加 __DAM用戶__ 組（作為對資產的寫權限）
1. 點擊 __保存並關閉__

通過授予技術帳戶AEM對資產具有寫權限的權限，請重新運行應用程式：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

到終端的輸出將如下所示：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 驗證更改

1. 登錄到已AEM更新的as a Cloud Service環境（使用中提供的相同主機名） `aem` 命令行參數
1. 導航到 __資產__ > __檔案__
1. 導航它由 `folder` 命令行參數，例如 __WKND__ > __英語__ > __冒險__ > __納帕品酒會__
1. 開啟 __屬性__ 資料夾中的任何資產
1. 導航到 __高級__ 頁籤
1. 查看已更新屬性的值，例如 __版權__ 映射到已更新 `metadata/dc:rights` JCR屬性，它現在反映在 `propertyValue` 參數，例如 __WKND限制使用__

![WKND限制使用元資料更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

現在，我們已使用本AEM地開發訪問令牌和生產就緒服務到服務訪問令牌以寫程式方式訪問as a Cloud Service!
