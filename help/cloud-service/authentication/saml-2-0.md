---
title: AEM as a Cloud Service上的SAML 2.0
description: 了解如何在AEMas a Cloud Service發佈服務上設定SAML 2.0驗證。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: 343040.jpeg
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '2815'
ht-degree: 1%

---

# SAML 2.0驗證{#saml-2-0-authentication}

了解如何針對您選擇的SAML 2.0相容IDP，設定和驗證一般使用者(而非AEM作者)。

## 什麼AEM適用的SAMLas a Cloud Service?

SAML 2.0與AEM Publish（或預覽）的整合，可讓AEM型網頁體驗的一般使用者驗證非AdobeIDP（身分提供者），並以具名的授權使用者身分存取AEM。

|  | AEM 作者 | AEM 發佈 |
|-----------------------|:----------:|:-----------:|
| SAML 2.0支援 | ✘ | ✔ |

+++ 使用AEM了解SAML 2.0流程

AEM Publish SAML整合的一般流程如下：

1. 使用者向AEM發佈請求表示需要驗證。
   + 用戶請求受CUG/ACL保護的資源。
   + 用戶請求受身份驗證要求約束的資源。
   + 使用者會依循AEM登入端點的連結(即 `/system/sling/login`)明確要求登入動作。
1. AEM向IDP提出AuthnRequest，要求IDP開始驗證程式。
1. 使用者向IDP驗證。
   + IDP會提示使用者取得認證。
   + 使用者已獲得IDP的驗證，不需要提供進一步的認證。
1. IDP會產生包含使用者資料的SAML聲明，並使用IDP的私人憑證簽署。
1. IDP會透過HTTPPOST，透過使用者的網頁瀏覽器，將SAML斷言傳送至AEM Publish。
1. AEM Publish會收到SAML聲明，並使用IDP公開憑證來驗證SAML聲明的完整性和真實性。
1. AEM Publish會根據SAML 2.0 OSGi設定和SAML斷言的內容來管理AEM使用者記錄。
   + 建立用戶
   + 同步使用者屬性
   + 更新AEM使用者群組成員資格
1. AEM Publish會設定AEM `login-token` HTTP回應上的cookie，用於驗證後續對AEM Publish的要求。
1. AEM發佈會依 `saml_request_path` cookie。

+++

## 配置逐步說明

>[!VIDEO](https://video.tv.adobe.com/v/343040/?quality=12&learn=on)

這部影片會逐步說明如何設定與AEMas a Cloud Service發佈服務的SAML 2.0整合，以及使用Okta做為IDP。

## 必備條件

設定SAML 2.0驗證時，需要下列項目：

+ Deployment Manager對Cloud Manager的訪問
+ AEM管理員對AEMas a Cloud Service環境的存取
+ 管理員訪問IDP
+ （可選）存取用於加密SAML裝載的公開/私用金鑰組

SAML 2.0僅支援驗證AEM發佈或預覽的使用。 若要使用和IDP管理AEM作者的驗證， [整合IDP與Adobe IMS。](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html).


## 在AEM上安裝IDP公開憑證

IDP的公開憑證會新增至AEM Global Trust Store，並用來驗證IDP傳送的SAML聲明是否有效。

+++SAML斷言簽署流程

![SAML 2.0 - IDP SAML聲明簽署](./assets/saml-2-0/idp-signing-diagram.png)

1. 使用者向IDP驗證。
1. IDP會產生包含使用者資料的SAML斷言。
1. IDP使用IDP的私人證書籤署SAML聲明。
1. IDP會向AEM Publish的SAML端點(`.../saml_login`)，包含簽署的SAML斷言。
1. AEM Publish會收到包含已簽署SAML斷言的HTTPPOST，可使用IDP公開憑證來驗證簽名。

+++

![將IDP公開憑證新增至全球信託存放區](./assets/saml-2-0/global-trust-store.png)

1. 取得 __公開憑證__ IDP的檔案。 此憑證可讓AEM驗證IDP提供給AEM的SAML斷言。

   憑證為PEM格式，應類似：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以AEM管理員身分登入AEM Author。
1. 導覽至 __「工具」>「安全性」>「信任儲存」__.
1. 建立或開啟全局信任儲存。 如果建立全局信任儲存，請將密碼儲存在安全的某個位置。
1. 展開 __從CER檔案添加證書__.
1. 選擇 __選擇證書檔案__，並上傳IDP提供的憑證檔案。
1. 離開 __將憑證對應至使用者__ 空白。
1. 選擇 __提交__.
1. 新新增的憑證會顯示在 __從CRT檔案添加證書__ 區段。
1. 請注意 __別名__，因為此值用於 [SAML 2.0驗證處理常式OSGi設定](#saml-2-0-authentication-handler-osgi-configuration).
1. 選擇 __儲存並關閉__.

全域信任存放區是在AEM作者上使用IDP的公開憑證來設定，但由於SAML僅用於AEM發佈，因此必須將全域信任存放區複製到AEM Publish,IDP公開憑證才能在該處存取。

![將全域信任存放區復寫至AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. 導覽至 __工具>部署>套件__.
1. 建立套件
   + 包名稱： `Global Trust Store`
   + 版本: `1.0.0`
   + 群組: `com.your.company`
1. 編輯新 __全局信任儲存__ 包。
1. 選取 __篩選器__ 頁簽，並為根路徑添加篩選器 `/etc/truststore`.
1. 選擇 __完成__ 然後 __儲存__.
1. 選取 __建置__ 按鈕 __全局信任儲存__ 包。
1. 建置後，選擇 __更多__ > __複製__ 激活全局信任儲存節點(`/etc/truststore`)發佈。

## 安裝AEM公開/私密金鑰組{#install-aem-public-private-key-pair}

_安裝AEM公開/私密金鑰組為選用_

AEM Publish可設定為簽署AuthnRequests（對IDP），以及加密SAML聲明(對AEM)。 這是透過為AEM Publish提供私密金鑰來達成，且與IDP相符的公開金鑰。

+++ 了解AuthnRequest簽署流程（選用）

AEM Publish可簽署AuthnRequest（發起登入程式的AEM Publish向IDP提出的請求）。 為此，AEM Publish會使用私密金鑰簽署AuthnRequest，讓IDP使用公開金鑰驗證簽名。 這可保證IDP AuthnRequest是由AEM Publish所啟動、要求，而非惡意第三方。

![SAML 2.0 - SP AuthnRequest簽名](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 使用者向AEM Publish發出HTTP要求，導致向IDP發出SAML驗證要求。
1. AEM Publish會產生SAML要求，以傳送至IDP。
1. AEM Publish使用AEM私密金鑰簽署SAML請求。
1. AEM Publish會起始AuthnRequest，此為包含已簽署SAML要求的HTTP用戶端重新導向至IDP。
1. IDP會收到AuthnRequest，並使用AEM公開金鑰驗證簽名，以保證AEM Publish已起始AuthnRequest。
1. 然後，AEM Publish會使用IDP公開憑證，驗證解密後SAML斷言的完整性和真實性。

+++

+++ 了解SAML斷言加密流程（選用）

IDP與AEM Publish之間的所有HTTP通訊都應透過HTTPS，因此預設為安全。 但是，如有需要，除了HTTPS提供的機密性外，還需要額外的機密性，SAML斷言就可以加密。 為此，IDP會使用私密金鑰加密SAML斷言資料，而AEM Publish會使用私密金鑰解密SAML斷言。

![SAML 2.0 - SP SAML斷言加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 使用者向IDP驗證。
1. IDP會產生包含使用者資料的SAML聲明，並使用IDP的私人憑證簽署。
1. IDP接著會使用AEM公開金鑰加密SAML斷言，而這需要AEM私密金鑰解密。
1. 加密的SAML斷言會透過使用者的網頁瀏覽器傳送至AEM Publish。
1. AEM Publish會接收SAML斷言，並使用AEM私密金鑰解密。
1. IDP會提示使用者進行驗證。

+++

AuthnRequest簽名和SAML斷言加密都是選用的，不過都已啟用，使用 [SAML 2.0驗證處理常式OSGi配置屬性 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)，表示不能同時使用或兩者皆不使用。

![AEM authentication-service金鑰存放區](./assets/saml-2-0/authentication-service-key-store.png)

1. 取得用來簽署AuthnRequest及加密SAML斷言的公開金鑰、私密金鑰（PKCS#8,DER格式）和憑證鏈式檔案（可能是公開金鑰）。 金鑰通常由IT組織的安全團隊提供。

   + 可使用 __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 將公開金鑰上傳至IDP。
   + 使用 `openssl` 方法，公開金鑰為 `aem-public.crt` 檔案。
1. 以AEM管理員身分登入AEM Author，以上傳私密金鑰。
1. 導覽至 __「工具」>「安全性」>「信任儲存」__，然後選取 __authentication-service__ 用戶，然後選擇 __屬性__ 從頂端動作列。
1. 導覽至 __「工具」>「安全性」>「使用者」__，然後選取 __authentication-service__ 用戶，然後選擇 __屬性__ 從頂端動作列。
1. 選取 __金鑰存放區__ 標籤。
1. 建立或開啟金鑰存放區。 如果建立金鑰存放區，請確保密碼安全。
1. 選擇 __從DER檔案添加私鑰__，並將私密金鑰和連結檔案新增至AEM:
   + __別名__:提供有意義的名稱，通常是國內流離失所者的名稱。
   + __私密金鑰檔案__:上傳私密金鑰檔案（DER格式的PKCS#8）。
      + 使用 `openssl` 方法，這是 `aem-private-pkcs8.der` 檔案
   + __選擇證書鏈檔案__:上傳隨附的連結檔案（可能是公開金鑰）。
      + 使用 `openssl` 方法，這是 `aem-public.crt` 檔案
   + 選擇 __提交__
1. 新新增的憑證會顯示在 __從CRT檔案添加證書__ 區段。
   + 請注意 __別名__ 因為此 [SAML 2.0驗證處理常式OSGi設定](#saml-20-authentication-handler-osgi-configuration)
1. 選擇 __儲存並關閉__.
1. 選擇 __authentication-service__ 用戶，然後選擇 __啟動__ 從頂端動作列。

## 配置SAML 2.0驗證處理程式{#configure-saml-2-0-authentication-handler}

AEM SAML設定是透過 __AdobeGranite SAML 2.0驗證處理常式__ OSGi配置。
設定是OSGi工廠設定，這表示單一AEMas a Cloud Service發佈服務可能有多個SAML設定，涵蓋存放庫的個別資源樹狀結構；這對於多網站AEM部署很實用。

+++ SAML 2.0驗證處理常式OSGi設定字彙表

### AdobeGranite SAML 2.0驗證處理常式OSGi設定{#configure-saml-2-0-authentication-handler-osgi-configuration}

|  | OSGi屬性 | 必要 | 值格式 | 預設值 | 說明 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路徑 | `path` | ✔ | 字串陣列 | `/` | AEM路徑此驗證處理常式用於。 |
| IDP URL | `idpUrl` | ✔ | 字串 |  | 傳送SAML驗證請求的IDP URL。 |
| IDP憑證別名 | `idpCertAlias` | ✔ | 字串 |  | AEM全域信任存放區中找到的IDP憑證別名 |
| IDP HTTP重新導向 | `idpHttpRedirect` | ✘ | 布林值 | `false` | 指出是否有HTTP重新導向至IDP URL，而非傳送AuthnRequest。 設為 `true` IDP啟動的驗證。 |
| IDP識別碼 | `idpIdentifier` | ✘ | 字串 |  | 唯一IDP ID，可確保AEM使用者和群組的獨特性。 如果空白，則 `serviceProviderEntityId` 的值。 |
| 斷言使用者服務URL | `assertionConsumerServiceURL` | ✘ | 字串 |  | 此 `AssertionConsumerServiceURL` AuthnRequest中的URL屬性，指定 `<Response>` 訊息必須傳送至AEM。 |
| SP實體ID | `serviceProviderEntityId` | ✔ | 字串 |  | 為IDP唯一識別AEM;通常是AEM主機名稱。 |
| SP加密 | `useEncryption` | ✘ | 布林值 | `true` | 指出IDP是否加密SAML斷言。 需要 `spPrivateKeyAlias` 和 `keyStorePassword` 設定。 |
| SP私鑰別名 | `spPrivateKeyAlias` | ✘ | 字串 |  | 中私密金鑰的別名 `authentication-service` 使用者的金鑰存放區。 若 `useEncryption` 設為 `true`. |
| SP密鑰儲存密碼 | `keyStorePassword` | ✘ | 字串 |  | 「authentication-service」用戶密鑰儲存的密碼。 若 `useEncryption` 設為 `true`. |
| 預設重新導向 | `defaultRedirectUrl` | ✘ | 字串 | `/` | 成功驗證後的預設重新導向URL。 可相對於AEM主機(例如 `/content/wknd/us/en/html`)。 |
| 使用者Id屬性 | `userIDAttribute` | ✘ | 字串 | `uid` | 包含AEM使用者使用者ID的SAML斷言屬性名稱。 保留為空以使用 `Subject:NameId`. |
| 自動建立AEM使用者 | `createUser` | ✘ | 布林值 | `true` | 指出AEM使用者是否在成功驗證時建立。 |
| AEM使用者中繼路徑 | `userIntermediatePath` | ✘ | 字串 |  | 建立AEM使用者時，此值會作為中間路徑(例如 `/home/users/<userIntermediatePath>/jane@wknd.com`)。 需要 `createUser` 設為 `true`. |
| AEM使用者屬性 | `synchronizeAttributes` | ✘ | 字串陣列 |  | 要在AEM使用者上儲存的SAML屬性對應清單，格式為 `[ "saml-attribute-name=path/relative/to/user/node" ]` (例如， `[ "firstName=profile/givenName" ]`)。 請參閱 [原生AEM屬性的完整清單](#aem-user-attributes). |
| 新增使用者至AEM群組 | `addGroupMemberships` | ✘ | 布林值 | `true` | 指出AEM使用者在成功驗證後是否自動新增至AEM使用者群組。 |
| AEM群組成員資格屬性 | `groupMembershipAttribute` | ✘ | 字串 | `groupMembership` | SAML斷言屬性的名稱，包含應新增使用者的AEM使用者群組清單。 需要 `addGroupMemberships` 設為 `true`. |
| 預設AEM群組 | `defaultGroups` | ✘ | 字串陣列 |  | 已驗證的使用者一律會新增至AEM使用者群組清單(例如 `[ "wknd-user" ]`)。 需要 `addGroupMemberships` 設為 `true`. |
| NameIDPolicy格式 | `nameIdFormat` | ✘ | 字串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 要在AuthnRequest消息中發送的NameIDPolicy格式參數的值。 |
| 儲存SAML回應 | `storeSAMLResponse` | ✘ | 布林值 | `false` | 指出 `samlResponse` 值儲存在AEM上 `cq:User` 節點。 |
| 處理註銷 | `handleLogout` | ✘ | 布林值 | `false` | 指示註銷請求是否由此SAML驗證處理程式處理。 需要 `logoutUrl` 設定。 |
| 註銷URL | `logoutUrl` | ✘ | 字串 |  | 傳送SAML登出請求的IDP URL。 若 `handleLogout` 設為 `true`. |
| 時鐘容限 | `clockTolerance` | ✘ | 整數 | `60` | 驗證SAML斷言時，IDP和AEM(SP)時鐘偏差容限。 |
| 摘要方法 | `digestMethod` | ✘ | 字串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP在簽署SAML訊息時使用的摘要演算法。 |
| 簽名方法 | `signatureMethod` | ✘ | 字串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP在簽署SAML訊息時使用的簽名演算法。 |
| 身分同步類型 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 請勿變更 `from` 預設為AEMas a Cloud Service。 |
| 服務排名 | `service.ranking` | ✘ | 整數 | `5002` | 同樣，建議使用更高的排名配置 `path`. |

### AEM使用者屬性{#aem-user-attributes}

AEM使用下列使用者屬性，可透過 `synchronizeAttributes` AdobeGranite SAML 2.0 Authentication Handler OSGi設定中的屬性。  任何IDP屬性皆可同步至任何AEM使用者屬性，但對應至AEM使用屬性（如下所列）可讓AEM自然使用。

| 使用者屬性 | 相對屬性路徑來自 `rep:User` 節點 |
|--------------------------------|--------------------------|
| 標題(例如 `Mrs`) | `profile/title` |
| 指定名稱（即名字） | `profile/givenName` |
| 姓氏（即姓氏） | `profile/familyName` |
| 職務 | `profile/jobTitle` |
| 電子郵件地址 | `profile/email` |
| 街道地址 | `profile/street` |
| 城市 | `profile/city` |
| 郵遞區號 | `profile/postalCode` |
| 國家/地區 | `profile/country` |
| 電話號碼 | `profile/phoneNumber` |
| 關於我 | `profile/aboutMe` |

+++

1. 在您的專案中建立OSGi設定檔案，網址為 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 在IDE中開啟。
   + 變更 `/wknd-examples/` 至 `/<project name>/`
   + 之後的識別碼 `~` 檔案名稱中應唯一識別此設定，因此可能是IDP的名稱，例如 `...~okta.cfg.json`. 值應為英數字元，帶有連字型大小。
1. 將下列JSON貼入 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` 檔案，並更新 `wknd` 視需要參考。

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. 視專案需要更新值。 請參閱 __SAML 2.0驗證處理常式OSGi設定字彙表__ 設定屬性說明的上方
1. 建議使用OSGi環境變數和機密，當值可能與發行週期不同步，或當類似環境類型/服務層之間的值不同時，則建議使用，但不是必要。 預設值可使用 `$[env:..;default=the-default-value]"` 語法，如上所示。

每個環境的OSGi配置(`config.publish.dev`, `config.publish.stage`，和 `config.publish.prod`)，則當SAML組態因環境而異時，可以使用特定屬性來定義。

### 使用加密

當 [加密AuthnRequest和SAML聲明](#encrypting-the-authnrequest-and-saml-assertion)，需要下列屬性： `useEncryption`, `spPrivateKeyAlias`，和 `keyStorePassword`. 此 `keyStorePassword` 包含密碼，因此值不得儲存在OSGi設定檔案中，而是使用 [密碼配置值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++（可選）更新OSGi配置以使用加密

1. 開啟 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 在IDE中。
1. 新增三個屬性 `useEncryption`, `spPrivateKeyAlias`，和 `keyStorePassword` 如下所示。

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. 加密所需的三個OSGi配置屬性是：

+ `useEncryption` 設為 `true`
+ `spPrivateKeyAlias` 包含SAML整合所使用之私密金鑰的金鑰存放區項目別名。
+ `keyStorePassword` 包含 [OSGi密碼組態變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) 包含 `authentication-service` 用戶密鑰庫的密碼。

+++

## 設定反向連結篩選

在SAML驗證程式期間，IDP會為AEM Publish的發起用戶端HTTPPOST `.../saml_login` 終點。 如果IDP和AEM Publish位於不同的來源，則AEM Publish的 __反向連結篩選__ 是透過OSGi設定來設定，以允許來自IDP來源的HTTP POST。

1. 在您的專案中建立（或編輯）OSGi設定檔案，位置為 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + 變更 `/wknd-examples/` 至 `/<project name>/`
1. 確保 `allow.empty` 值設為 `true`, `allow.hosts` (或者，如果你願意， `allow.hosts.regexp`)包含IDP的來源，以及 `filter.methods` 包括 `POST`. OSGi設定應類似於：

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish支援單一反向連結篩選器設定，因此可將SAML設定需求與任何現有設定合併。

每個環境的OSGi配置(`config.publish.dev`, `config.publish.stage`，和 `config.publish.prod`)，則可以使用特定屬性來定義 `allow.hosts` (或 `allow.hosts.regex`)因環境而異。

## 設定跨原始資源共用(CORS)

在SAML驗證程式期間，IDP會為AEM Publish的發起用戶端HTTPPOST `.../saml_login` 終點。 如果IDP和AEM Publish存在於不同的主機/網域上，則AEM Publish的 __CRoss-Origin資源共用(CORS)__ 必須設定為允許來自IDP主機/網域的HTTP POST。

此HTTPPOST請求的 `Origin` 標題的值通常與AEM發佈主機不同，因此需要CORS設定。

在本機AEM SDK上測試SAML驗證時(`localhost:4503`),IDP可將 `Origin` 標題至 `null`. 如果是，請新增 `"null"` 到 `alloworigin` 清單。

1. 在您的專案中建立OSGi設定檔案，網址為 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 變更 `/wknd-examples/` 至專案名稱
   + 之後的識別碼 `~` 檔案名稱中應唯一識別此設定，因此可能是IDP的名稱，例如 `...CORSPolicyImpl~okta.cfg.json`. 值應為英數字元，帶有連字型大小。
1. 將下列JSON貼入 `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 檔案。

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

每個環境的OSGi配置(`config.publish.dev`, `config.publish.stage`，和 `config.publish.prod`)，則可以使用特定屬性來定義 `alloworigin` 和 `allowedpaths` 因環境而異。

## 設定AEM Dispatcher以允許SAML HTTP POST

IDP在成功驗證IDP後，將協調HTTPPOST，返回AEM註冊 `/saml_login` 端點（在IDP中設定）。 此HTTPPOST `/saml_login` 在Dispatcher預設會封鎖，因此必須使用下列Dispatcher規則明確允許：

1. 開啟 `dispatcher/src/conf.dispatcher.d/filters/filters.any` 在IDE中。
1. 新增至檔案底部，此為結尾為的HTTP POST至URL的允許規則 `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

如果已配置Apache Webserver上的URL重寫(`dispatcher/src/conf.d/rewrites/rewrite.rules`)，請確定 `.../saml_login` 端點不會意外損壞。

## 啟用資料同步並封裝Token

一旦SAML驗證流程在AEM Publish中建立使用者，AEM使用者節點就能在AEM Publish服務層級進行驗證。
這需要 [資料同步](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization) 和 [封裝的Token](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#sticky-sessions-and-encapsulated-tokens) Adobe支援啟用。

傳送要求給Adobe客戶支援(透過 [AdminConsole](https://adminconsole.adobe.com) >支援)請求：

> 針對Program X和環境Y，在AEM Publish服務上啟用資料同步和封裝的Token。

## 部署SAML設定

OSGi設定必須提交至Git，並使用Cloud Manager部署至AEMas a Cloud Service。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

部署目標Cloud Manager Git分支(在此範例中， `develop`)，使用完整堆棧部署管道。
