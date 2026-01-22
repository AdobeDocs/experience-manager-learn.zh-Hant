---
title: AEM as a Cloud Service上的SAML 2.0
description: 瞭解如何在AEM as a Cloud Service Publish服務上設定SAML 2.0驗證。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# SAML 2.0驗證{#saml-2-0-authentication}

學習如何設定並認證終端使用者（非 AEM 作者）至您選擇的 SAML 2.0 相容 IDP。

## AEM 作為雲端服務的 SAML 是什麼？

SAML 2.0 與 AEM Publish（或 Preview）整合，允許基於 AEM 的網頁體驗終端使用者驗證非 Adobe IDP（身份提供者），並以指定授權使用者身份存取 AEM。

|                       | AEM 作者 | AEM Publish |
|-----------------------|:----------:|:-----------:|
| SAML 2.0支援 | ✘ | ✔ |

+++ 透過AEM瞭解SAML 2.0流程

AEM Publish SAML 整合的典型流程如下：

1. 使用者向 AEM Publish 提出請求，表示需要認證。
   + 使用者請求 CUGs/ACL 保護資源。
   + 使用者請求一個受認證要求約束的資源。
   + 使用者會跟著一個連結前往 AEM 的登入端點（例如 `/system/sling/login`），該連結明確請求登入操作。
1. AEM 向 IDP 發出 AuthnRequest，請求 IDP 開始認證程序。
1. 使用者會向 IDP 進行認證。
   + 使用者會被 IDP 提示輸入憑證。
   + 使用者已透過 IDP 認證，無需提供更多憑證。
1. IDP 會產生包含使用者資料的 SAML 斷言，並使用 IDP 的私有憑證進行簽署。
1. IDP 透過使用者的網頁瀏覽器（RESPECTIVE_PROTECTED_PATH/saml_login）透過 HTTP POST 將 SAML 斷言傳送至 AEM Publish。
1. AEM Publish 接收 SAML 斷言，並使用 IDP 公開憑證驗證 SAML 斷言的完整性與真實性。
1. AEM Publish 根據 SAML 2.0 OSGi 設定管理 AEM 使用者紀錄，以及 SAML 聲明的內容。
   + 建立使用者
   + 同步使用者屬性
   + 更新 AEM 使用者群組成員資格
1. AEM Publish 會在 HTTP 回應上設定 AEM `login-token` Cookie，用來驗證後續對 AEM Publish 的請求。
1. AEM Publish 會依照 cookie 指定的 `saml_request_path` URL 將使用者重新導向 AEM Publish 上的網址。

+++

## 配置流程

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

這支影片會介紹如何設定 SAML 2.0 與 AEM 整合為雲端服務發佈服務，並使用 Okta 作為 IDP。

## 先決條件

設定 SAML 2.0 認證時，以下條件是必要的：

+ 部署管理員對Cloud Manager的存取權
+ AEM 管理員存取 AEM 作為雲端服務環境
+ IDP 管理員存取權
+ 可選擇存取用於加密 SAML 有效載荷的公私鑰對
+ AEM 網站頁面（或頁面樹），發佈至 AEM Publish，並 [由封閉使用者群組（CUG）保護](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 僅支援用於驗證 AEM Publish 或 Preview 的使用。 要管理使用 AEM Author 和 IDP 的認證，請 [將 IDP 與 Adobe IMS](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html) 整合。


## 在AEM上安裝IDP公開憑證

IDP的公開憑證會新增至AEM的全域信任存放區，並用來驗證IDP傳送的SAML宣告是否有效。

+++SAML 斷言簽署流程

![SAML 2.0 - IDP SAML 聲明簽署](./assets/saml-2-0/idp-signing-diagram.png)

1. 使用者會向 IDP 進行認證。
1. IDP 會產生包含使用者資料的 SAML 斷言。
1. IDP 使用其私有憑證簽署 SAML 聲明。
1. IDP向AEM Publish的SAML端點(`.../saml_login`)起始使用者端HTTP POST，其中包含已簽署的SAML宣告。
1. AEM Publish會收到包含已簽署SAML宣告的HTTP POST，可以使用IDP公開憑證驗證簽名。

+++

![將IDP公開憑證新增至全域信任存放區](./assets/saml-2-0/global-trust-store.png)

1. 從IDP取得 __公開證書__ 檔案。 此憑證允許 AEM 驗證 IDP 提供給 AEM 的 SAML 斷言。

   該證書為PEM格式，內容應如下：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以 AEM 管理員身份登入 AEM Author。
1. 前往 __Tools > Security > Trust Store__。
1. 創建或開設全球信託商店。 如果建立全球信任儲存庫，請將密碼存放在安全的地方。
1. 展開 __從 CER 檔案__&#x200B;新增憑證。
1. 選擇 __「選擇憑證檔案__」，並上傳由 IDP 提供的憑證檔案。
1. 將地圖憑證留 __給使用者__ 空白。
1. 選擇 __提交__。
1. 新增的憑證顯示在「 __從 CRT 檔案__ 新增憑證」部分之上。
1. 記下&#x200B;__別名__，因為此值用於[SAML 2.0驗證處理常式OSGi設定](#saml-2-0-authentication-handler-osgi-configuration)。
1. 選取「__儲存並關閉__」。

全域信任存放區在AEM Author上設定了IDP的公開憑證，但由於SAML僅用於AEM Publish，因此必須將全域信任存放區復寫到AEM Publish，才能在那裡存取IDP公開憑證。

![將全域信任存放區復寫到AEM發佈](./assets/saml-2-0/global-trust-store-replicate.png)

1. 導覽至 __工具>部署>套件__。
1. 建立套件
   + 套件名稱： `Global Trust Store`
   + 版本： `1.0.0`
   + 小組： `com.your.company`
1. 編輯新的&#x200B;__全域信任存放區__&#x200B;封裝。
1. 選取&#x200B;__篩選器__&#x200B;索引標籤，並為根路徑`/etc/truststore`新增篩選器。
1. 選擇 __完成__ ，然後 __儲存__。
1. 選擇 __Global Trust Store__ 套件的&#x200B;__建置__&#x200B;按鈕。
1. 建置完成後，選擇 __更多__ > __複製__ 以啟用全域信任儲存節點（`/etc/truststore`）以啟動 AEM 發佈。

## 建立認證服務金鑰儲存{#authentication-service-keystore}

_當 [SAML 2.0 認證處理程序的 OSGi 設定屬性`handleLogout`被設定為 或`true`](#saml-20-authenticationsaml-2-0-authentication) [需要 AuthnRequest 簽署/SAML 斷言加密](#install-aem-public-private-key-pair)時，就需要建立認證服務的金鑰儲存庫_

1. 以 AEM 管理員身份登入 AEM Author，上傳私鑰。
1. 請前往 __「工具>安全>使用者__」，選擇 __認證服務__ 使用者，並從上方動作列選擇 __屬性__ 。
1. 選擇 __Keystore__ 標籤。
1. 建立或開啟鑰匙庫。 如果建立金鑰儲存，請妥善保管密碼。
   + [只有在需要 AuthnRequest 簽署/SAML 斷言加密時，才會在此金鑰庫](#install-aem-public-private-key-pair)中安裝公私密金鑰庫。
   + 如果這個 SAML 整合支援登出，但不支援 AuthnRequest 簽署/SAML 斷言，那麼空的金鑰庫就足夠了。
1. 選取「__儲存並關閉__」。
1. 建立包含更新後 __認證服務__ 使用者的套件。

   _Use以下使用套件的臨時解決方法&#x200B;:_

   1. 導覽至 __工具>部署>套件__。
   1. 建立套件
      + 套件名稱： `Authentication Service`
      + 版本： `1.0.0`
      + 小組： `com.your.company`
   1. 編輯新的 __認證服務金鑰儲存__ 套件。
   1. 選擇 __篩選器__ 標籤，並為根路徑 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加一個篩選器。
      + `<AUTHENTICATION SERVICE UUID>`可透過導覽至&#x200B;__「安全工具>使用者__」，並選擇&#x200B;__「認證服務__&#x200B;使用者」>找到。UUID 是 URL 的最後部分。
   1. 選擇 __完成__ ，然後 __儲存__。
   1. 選擇&#x200B;__驗證服務金鑰儲存__&#x200B;套件的&#x200B;__建置__&#x200B;按鈕。
   1. 建置完成後，選擇 __更多__ > __複製__ 以啟用認證服務金鑰儲存以啟用 AEM 發佈。

## 安裝 AEM 公私密金鑰對{#install-aem-public-private-key-pair}

_安裝 AEM 公私密金鑰對是可選的_

AEM Publish可設定為簽署AuthnRequests (to IDP)及加密SAML宣告(to AEM)。 這是透過提供私密金鑰給AEM Publish並且比對公開金鑰給IDP來達成。

+++ 瞭解AuthnRequest簽署流程（選用）

AuthnRequest（由 AEM Publish 向 IDP 發出並啟動登入流程的請求）可由 AEM Publish 簽署。 為此，AEM Publish 會用私鑰簽署 AuthnRequest，然後 IDP 用公鑰驗證簽章。 這能保證 IDP 是 AEM Publish 發起並請求的，而非惡意第三方。

![SAML 2.0 - SP AuthnRequest 簽署](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 使用者向 AEM Publish 提出 HTTP 請求，進而向 IDP 發出 SAML 認證請求。
1. AEM Publish 會產生 SAML 請求，發送給 IDP。
1. AEM Publish使用AEM的私密金鑰簽署SAML請求。
1. AEM發佈會起始AuthnRequest，此HTTP使用者端重新導向至包含已簽署SAML請求的IDP。
1. IDP會收到AuthnRequest，並使用AEM的公開金鑰驗證簽名，保證AEM Publish已起始AuthnRequest。
1. AEM Publish 接著使用 IDP 公開憑證驗證解密後 SAML 斷言的完整性與真實性。

+++

+++ 了解 SAML 斷言加密流程（可選）

所有 IDP 與 AEM Publish 之間的 HTTP 通訊都應該透過 HTTPS 進行，因此預設是安全的。 然而，依需求，若需要額外的保密性，SAML 斷言可加密，以超越 HTTPS 的保密。 為此，IDP 使用私鑰加密 SAML 斷言資料，AEM Publish 則利用私密金鑰解密 SAML 斷言。

![SAML 2.0 - SP SAML 斷言加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 使用者會向 IDP 進行認證。
1. IDP 會產生包含使用者資料的 SAML 斷言，並使用 IDP 的私有憑證進行簽署。
1. IDP 接著用 AEM 的公鑰加密 SAML 斷言，解密時需 AEM 私鑰。
1. 加密的 SAML 斷言會透過使用者的網頁瀏覽器傳送至 AEM Publish。
1. AEM Publish 會接收 SAML 斷言，並使用 AEM 的私鑰將其解密。
1. IDP 會提示使用者進行驗證。

+++

AuthnRequest簽署和SAML宣告加密都是選用專案，但是兩者都是使用[SAML 2.0驗證處理常式OSGi組態屬性`useEncryption`](#saml-20-authenticationsaml-2-0-authentication)啟用的，這表示兩者都無法使用或都不使用。

![AEM驗證服務金鑰存放區](./assets/saml-2-0/authentication-service-key-store.png)

1. 取得用來簽署AuthnRequest以及加密SAML宣告的公開金鑰、私密金鑰（DER格式為PKCS#8）及憑證鏈結檔案（這有可能是公開金鑰）。 這些金鑰通常由IT組織的安全團隊提供。

   + 可使用 __openssl__ 產生自簽金鑰對：

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 將公鑰上傳到 IDP。
   + 使用`openssl`上述方法，公鑰即為檔案。`aem-public.crt`
1. 以 AEM 管理員身份登入 AEM Author，上傳私鑰。
1. 前往 __信任儲存__>>安全工具，選擇 __認證服務__ 使用者，並從上方動作列選擇 __屬性__ 。
1. 導覽至&#x200B;__工具>安全性>使用者__，然後選取&#x200B;__驗證服務__&#x200B;使用者，並從頂端動作列選取&#x200B;__屬性__。
1. 選取&#x200B;__金鑰存放區__&#x200B;索引標籤。
1. 建立或開啟金鑰存放區。 如果建立金鑰儲存，請妥善保管密碼。
1. 選擇 __從 DER 檔案__&#x200B;新增私鑰，並將私鑰與鏈檔案加入 AEM：
   + __別名__：提供一個有意義的名字，通常是流離失所者的名字。
   + __私鑰檔案__：上傳私鑰檔案（DER 格式為 PKCS#8）。
      + 用`openssl`上述方法，這就是檔案`aem-private-pkcs8.der`
   + __選擇憑證鏈檔__：上傳隨附的鏈式檔案（這可能是公鑰）。
      + 用`openssl`上述方法，這就是檔案`aem-public.crt`
   + 選取&#x200B;__提交__
1. 新增的憑證出現在&#x200B;__從CRT檔案__&#x200B;新增憑證區段上方。
   + 記下&#x200B;__別名__，因為它用於[SAML 2.0驗證處理常式OSGi設定](#saml-20-authentication-handler-osgi-configuration)
1. 選取「__儲存並關閉__」。
1. 建立包含更新後 __認證服務__ 使用者的套件。

   _Use以下使用套件的臨時解決方法&#x200B;:_

   1. 導覽至 __工具>部署>套件__。
   1. 建立套件
      + 封裝名稱： `Authentication Service`
      + 版本： `1.0.0`
      + 小組： `com.your.company`
   1. 編輯新的 __認證服務金鑰儲存__ 套件。
   1. 選擇 __篩選器__ 標籤，並為根路徑 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加一個篩選器。
      + `<AUTHENTICATION SERVICE UUID>`可透過導覽至&#x200B;__「安全工具>使用者__」，並選擇&#x200B;__「認證服務__&#x200B;使用者」>找到。UUID 是 URL 的最後部分。
   1. 選擇 __完成__ ，然後 __儲存__。
   1. 選擇&#x200B;__驗證服務金鑰儲存__&#x200B;套件的&#x200B;__建置__&#x200B;按鈕。
   1. 建置完成後，選擇 __更多__ > __複製__ 以啟用認證服務金鑰儲存以啟用 AEM 發佈。

## 設定 SAML 2.0 認證處理器{#configure-saml-2-0-authentication-handler}

AEM 的 SAML 設定是透過 __Adobe Granite SAML 2.0 認證處理程序__ OSGi 配置來執行的。該組態為 OSGi 工廠設定，意即單一 AEM 作為雲端服務發佈服務，可能包含多個 SAML 配置，涵蓋資料庫的離散資源樹;這對於多站點 AEM 部署非常有用。

+++ SAML 2.0 認證處理程式 OSGi 設定詞彙表

### Adobe Granite SAML 2.0驗證處理常式OSGi設定{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi屬性 | 必要 | 價值格式 | 預設值 | 說明 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路徑 | `path` | ✔ | 字串陣列 | `/` | 此驗證處理常式用於的AEM路徑。 |
| IDP URL | `idpUrl` | ✔ | 字串 |                           | IDP URL 會傳送 SAML 認證請求。 |
| IDP 憑證別名 | `idpCertAlias` | ✔ | 字串 |                           | 在 AEM 全球信任儲存庫中找到的 IDP 證書別名 |
| IDP HTTP 重定向 | `idpHttpRedirect` | ✘ | 布林值 | `false` | 指示是否是 HTTP 重定向到 IDP URL，而非發送 AuthnRequest。 設定為 以便 `true` IDP 發起驗證。 |
| IDP 識別碼 | `idpIdentifier` | ✘ | 字串 |                           | 唯一的 IDP ID 以確保 AEM 使用者與群組的獨特性。 若為空，則改用 。`serviceProviderEntityId` |
| 判斷提示消費者服務URL | `assertionConsumerServiceURL` | ✘ | 字串 |                           | `AssertionConsumerServiceURL` AuthnRequest 中的 URL 屬性，指定訊息必須傳送至 AEM 的地點`<Response>`。 |
| SP實體 Id | `serviceProviderEntityId` | ✔ | 字串 |                           | 唯一識別 AEM 與 IDP 的關係;通常是 AEM 主機名稱。 |
| SP 加密 | `useEncryption` | ✘ | 布林值 | `true` | 指示IDP是否加密SAML宣告。 需要設定`spPrivateKeyAlias`和`keyStorePassword`。 |
| SP 私鑰別名 | `spPrivateKeyAlias` | ✘ | 字串 |                           | 私鑰 `authentication-service` 在使用者金鑰庫中的別名。 若 `useEncryption` 設定為 `true`，則為 必要。 |
| SP 金鑰儲存密碼 | `keyStorePassword` | ✘ | 字串 |                           | &#39;authentication-service&#39;使用者金鑰存放區的密碼。 如果`useEncryption`設定為`true`則為必要。 |
| 預設重新導向 | `defaultRedirectUrl` | ✘ | 字串 | `/` | 成功驗證後的預設重新導向URL。 可相對於AEM主機（例如`/content/wknd/us/en/html`）。 |
| 使用者 ID 屬性 | `userIDAttribute` | ✘ | 字串 | `uid` | SAML 斷言屬性名稱，包含 AEM 使用者的使用者 ID。 留空以便使用 `Subject:NameId`. |
| 自動建立 AEM 使用者 | `createUser` | ✘ | 布林值 | `true` | 顯示是否會在成功驗證時建立AEM使用者。 |
| AEM使用者中間路徑 | `userIntermediatePath` | ✘ | 字串 |                           | 在建立 AEM 使用者時，此值作為中間路徑（例如， `/home/users/<userIntermediatePath>/jane@wknd.com`）。 需要 `createUser` 設定為 `true`。 |
| AEM 使用者屬性 | `synchronizeAttributes` | ✘ | 字串陣列 |                           | 可在 AEM 使用者上儲存的 SAML 屬性映射列表，格式如下 `[ "saml-attribute-name=path/relative/to/user/node" ]` （例如， `[ "firstName=profile/givenName" ]`）。 請參閱 [完整的原生 AEM 屬性](#aem-user-attributes)清單。 |
| 將使用者加入 AEM 群組 | `addGroupMemberships` | ✘ | 布林值 | `true` | 顯示 AEM 使用者在成功認證後是否自動加入 AEM 使用者群組。 |
| AEM 群組成員屬性 | `groupMembershipAttribute` | ✘ | 字串 | `groupMembership` | SAML宣告屬性的名稱，該屬性包含使用者應新增至的AEM使用者群組清單。 需要`addGroupMemberships`設定為`true`。 |
| 預設AEM群組 | `defaultGroups` | ✘ | 字串陣列 |                           | AEM 用戶群組的認證使用者清單會被加入（例如， `[ "wknd-user" ]`）。 需要 `addGroupMemberships` 設定為 `true`。 |
| NameIDPolicy 格式 | `nameIdFormat` | ✘ | 字串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 要在AuthnRequest訊息中傳送的NameIDPolicy格式引數值。 |
| 儲存SAML回應 | `storeSAMLResponse` | ✘ | 布林值 | `false` | 指示`samlResponse`值是否儲存在AEM `cq:User`節點上。 |
| 處理登出 | `handleLogout` | ✘ | 布林值 | `false` | 表示登出請求是否由此 SAML 認證處理程序處理。 需要 `logoutUrl` 設定。 |
| 登出網址 | `logoutUrl` | ✘ | 字串 |                           | SAML 登出請求傳送至 IDP 的網址。 若 `handleLogout` 設定為 `true`，則為 必要。 |
| 時鐘公差 | `clockTolerance` | ✘ | 整數 | `60` | 在驗證 SAML 斷言時，IDP 與 AEM（SP）時鐘偏移容忍度。 |
| 消化法 | `digestMethod` | ✘ | 字串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP 在簽署 SAML 訊息時所使用的摘要演算法。 |
| 簽名法 | `signatureMethod` | ✘ | 字串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP 簽署 SAML 訊息時所使用的簽章演算法。 |
| 身份同步類型 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 請勿變更AEM as a Cloud Service的`from`預設值。 |
| 服務排名 | `service.ranking` | ✘ | 整數 | `5002` | 較高排名的配置偏好於相同 `path`。 |

### AEM 使用者屬性{#aem-user-attributes}

AEM 使用以下使用者屬性，這些屬性可透過 `synchronizeAttributes` Adobe Granite SAML 2.0 認證處理程序 OSGi 設定中的屬性填充。  任何 IDP 屬性都可以同步到任何 AEM 使用者屬性，但映射到 AEM 的 use attribute 屬性（如下列出）後，AEM 就能自然使用這些屬性。

| 使用者屬性 | 來自`rep:User`節點的相對屬性路徑 |
|--------------------------------|--------------------------|
| 標題（例如，`Mrs`） | `profile/title` |
| 名字（即名字） | `profile/givenName` |
| 姓氏 | `profile/familyName` |
| 職稱 | `profile/jobTitle` |
| 電子郵件地址 | `profile/email` |
| 街道地址 | `profile/street` |
| 城市 | `profile/city` |
| 郵遞區號 | `profile/postalCode` |
| 國家/地區 | `profile/country` |
| 電話號碼 | `profile/phoneNumber` |
| 關於我 | `profile/aboutMe` |

+++

1. 在`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json`的專案中建立OSGi設定檔，並在IDE中開啟。
   + 改變 `/wknd-examples/` 到你的 `/<project name>/`
   + 檔案名稱中 的識別 `~` 碼應能唯一識別此組態，因此可能是 IDP 的名稱，例如 `...~okta.cfg.json`。 數值應該是帶有連字符的字母數字。
1. 將以下 JSON 貼上檔案， `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` 並視需要更新 `wknd` 參考資料。

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

1. 依照專案需求更新數值。 請參閱 __上方 SAML 2.0 認證處理程式 OSGi 組態詞彙表__ 以了解組態屬性的說明。 這些 `path` 內容樹應包含由封閉使用者群組（CUG）保護且需認證的內容，而此認證處理程序應負責保護。
1. 建議但非強制，當值可能與發布週期不同步，或相似環境類型/服務層級間值不同時，使用 OSGi 環境變數與秘密。 預設值可用上述語法設定 `$[env:..;default=the-default-value]"` 。

如果 SAML 設定在不同環境間有差異，OSGi 配置`config.publish.dev`可依特定屬性定義（、 `config.publish.stage` `config.publish.prod`、 和 ）。

### 使用加密技術

在加密 AuthnRequest 與 SAML 斷言[時](#encrypting-the-authnrequest-and-saml-assertion)，需具備以下屬性：`useEncryption`、、 `spPrivateKeyAlias` `keyStorePassword`和 。因此，該 `keyStorePassword` 值不應儲存在 OSGi 設定檔中，而是透過 [秘密設定值注入](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=zh-Hant#secret-configuration-values)

+++可選擇更新OSGi設定以使用加密

1. 在IDE中開啟`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json`。
1. 將三個性質 `useEncryption`、 `spPrivateKeyAlias`、 和 `keyStorePassword` 相加如下所示。

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

1. 加密所需的三個 OSGi 設定屬性為：

+ `useEncryption` 設定為 `true`
+ `spPrivateKeyAlias` 包含用於 SAML 整合的私鑰儲存項目別名。
+ `keyStorePassword` 包含 [一個 OSGi 秘密設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=zh-Hant#secret-configuration-values) ，內含 `authentication-service` 使用者金鑰儲存的密碼。

+++

## 設定 Referrer 濾波器

在SAML驗證程式期間，IDP會向AEM Publish的`.../saml_login`端點起始使用者端HTTP POST。 如果 IDP 與 AEM Publish 存在於不同來源地，AEM Publish __的 Referrer Filter__ 會透過 OSGi 設定，允許來自 IDP 來源的 HTTP POST。

1. 在您的專案中建立（或編輯）OSGi 設定檔。`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`
   + 改變 `/wknd-examples/` 到你的 `/<project name>/`
1. 確保 `allow.empty` 值設為 `true`，（ `allow.hosts` 或你喜歡，） `allow.hosts.regexp`包含 IDP 的來源，且 `filter.methods` 包含 `POST`。 OSGi 配置應類似：

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

AEM Publish 支援單一的 Referrer 篩選器配置，因此將 SAML 設定需求與現有設定合併。

每個環境（`config.publish.dev`、 `config.publish.stage`和 `config.publish.prod`）的 OSGi 配置可依特定屬性定義，若 `allow.hosts` （或 `allow.hosts.regex`） 在不同環境中有所不同。

## 配置跨來源資源共享（CORS）

在 SAML 認證過程中，IDP 會向 AEM Publish `.../saml_login` 端點發起客戶端 HTTP POST。 若 IDP 與 AEM Publish 存在於不同的主機/網域，AEM Publish __的 CRoss-Origin 資源共享（CORS）__ 必須設定為允許來自 IDP 主機/網域的 HTTP POST。

此 HTTP POST 請求的 `Origin` 標頭通常與 AEM 發佈主機的值不同，因此需要 CORS 設定。

在測試本地 AEM SDK 的`localhost:4503` SAML 認證時，IDP 可能會將標頭設 `Origin` 為 `null`。 如果是，請加入`"null"` `alloworigin`清單。

1. 在你的專案中建立一個 OSGi 設定檔 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 更改 `/wknd-examples/` 您的專案名稱
   + 檔案名稱中 的識別 `~` 碼應能唯一識別此組態，因此可能是 IDP 的名稱，例如 `...CORSPolicyImpl~okta.cfg.json`。 數值應該是帶有連字符的字母數字。
1. 將以下 JSON 貼上到檔案中 `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 。

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

每個環境（`config.publish.dev`、 `config.publish.stage`和 `config.publish.prod`）的 OSGi 設定可依特定屬性定義，若 與 `alloworigin` `allowedpaths` 在不同環境中有所不同。

## 設定 AEM Dispatcher 以允許 SAML HTTP POSTs

成功認證 IDP 後，IDP 將協調 HTTP POST 回 AEM 註冊 `/saml_login` 端點（在 IDP 中設定）。 此 HTTP POST 在 `/saml_login` Dispatcher 預設被阻擋，因此必須透過以下 Dispatcher 規則明確允許：

1. 在你的 IDE 裡開啟 `dispatcher/src/conf.dispatcher.d/filters/filters.any` 。
1. 在檔案底部新增允許規則，將HTTP POST新增至結尾為`/saml_login`的URL。

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>在AEM中針對各種受保護路徑和不同IDP端點部署多個SAML設定時，請確保IDP張貼到RESPONSIVE_PROTECTED_PATH/saml_login端點以在AEM端選取適當的SAML設定。 如果同一受保護路徑有重複的SAML設定，則會隨機選取SAML設定。

如果在 Apache 網頁伺服器設定了 URL`dispatcher/src/conf.d/rewrites/rewrite.rules` 重寫（），確保發送到 `.../saml_login` 端點的請求不會被誤搞。

## 動態團體成員制

動態群組成員資格是 Apache Jackrabbit Oak[&#x200B; 中的](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html)一項功能，能提升群組評估與配置的效能。本節說明啟用此功能後，使用者與群組如何儲存，以及如何修改 SAML 認證處理程序的設定以啟用新舊環境。

### 如何在新環境中為 SAML 使用者啟用動態群組成員資格

為了大幅增強新AEM as a Cloud Service環境中的群組評估效能，建議在新環境中啟用動態群組成員資格功能。
這也是在啟動資料同步時的必要步驟。 更多詳細資料[在此](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier)。
若要這麼做，請將下列屬性新增至OSGI設定檔：

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

使用此組態，使用者和群組會建立為[Oak外部使用者](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html?lang=zh-Hant)。 在AEM中，外部使用者和群組有由`rep:principalName`或`[user name];[idp]`組成的預設`[group name];[idp]`。
指出存取控制清單(ACL)與使用者或群組的PrincipalName相關聯。
在先前未指定`identitySyncType`或設為`default`的現有部署中部署此設定時，將會建立新的使用者和群組，且必須將ACL套用至這些新使用者和群組。 請注意，外部群組不能包含本機使用者。 [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html)可用來建立SAML外部群組的ACL，即使這些群組僅在使用者執行登入時才會建立。
為避免在ACL上重構此功能，已實作標準[移轉功能](#automatic-migration-to-dynamic-group-membership-for-existing-environments)。

### 會員資格如何儲存在本地與外部群組中，並具備動態群組成員資格

在本地群組中，群組成員會儲存在 oak 屬性中： `rep:members`。 屬性包含群組中每個成員的 uid 清單。 更多詳情可在此[查閱](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository)。範例：

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

具有動態群組成員的外部群組不會在群組條目中儲存任何成員。群組成員身份則儲存在使用者的條目中。 其他檔案可在[這裡](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html)找到。 例如，這是群組的 OAK 節點：

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

這是該群組之使用者成員的節點：

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### 如何在現有環境中為 SAML 使用者啟用動態群組成員資格

如前節所述，外部使用者與群組的格式與本地使用者與群組略有不同。 你可以為外部群組定義新的 ACL 並配置新的外部使用者，或使用以下描述的遷移工具。

#### 啟用現有外部使用者環境的動態群組成員資格

當指定以下屬性時，SAML 認證處理器會建立外部使用者： `"identitySyncType": "idp"`。 在這種情況下，可以透過修改此性質來啟用動態群組成員資格： `"identitySyncType": "idp_dynamic"`。 不需要遷移。

#### 自動遷移至現有環境的動態群組成員，並有本地使用者

當指定以下屬性時，SAML 認證處理器會建立本地使用者： `"identitySyncType": "default"`。 當該屬性未被指定時，這也是預設值。 本節說明自動遷移程序所執行的步驟。

當此遷移啟用時，會在使用者驗證過程中執行，包含以下步驟：
1. 本機使用者會移轉至外部使用者，同時保留原始使用者名稱。 這表示已移轉的本機使用者（現在為外部使用者）會保留其原始使用者名稱，而非遵循上一節中所述的命名語法。 將新增一個額外的屬性，稱為： `rep:externalId`，其值為`[user name];[idp]`。 未修改使用者`PrincipalName`。
2. 每收到一個 SAML 斷言中的外部群組，都會建立一個外部群組。 若存在對應的局部群，則外部群會作為成員加入該本地群。
3. 使用者會被加入外部群組。
4. 當地使用者隨後會被移除出他所屬的所有 Saml 本地群組。 Saml 局部群由 OAK 屬性識別： `rep:managedByIdp`。 當 `syncType` 屬性未指定或未設為 `default`時，Saml 認證處理程序會設定此屬性。

例如，若遷移前 `user1` 是本地使用者，且是本地群組 `group1`成員，遷移後將發生以下變更：
`user1` 成為外部使用者。 屬性`rep:externalId`已新增至他的設定檔。
`user1`成為外部群組的成員： `group1;idp`
`user1`不再是本機群組的直接成員： `group1`
`group1;idp`是本機群組的成員： `group1`。
然後`user1`會透過繼承成為本機群組`group1`的成員

外部群組的成員資格會儲存在屬性中的使用者設定檔中 `rep:externalPrincipalNames`

### 如何設定自動遷移至動態群組成員

1. 請在 SAML OSGi 設定檔中啟用此屬性 `"identitySyncType": "idp_dynamic_simplified_id"` ： `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` ：
2. 以出廠 PID 從以下 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`字母開始配置新的 OSGi 服務： 。 例如，PID 可以是： `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`。 設定以下屬性：

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

要遷移多個 SAML 配置，必須建立多個 的 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` OSGi 出廠設定，每個設定指定遷移目標 `idpIdentifier` 。

## 部署 SAML 配置

OSGi 設定必須提交到 Git 中，並透過 Cloud Manager 部署至 AEM 的雲端服務。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

使用全端部署管線部署目標的 Cloud Manager Git 分支（此例 `develop`為 ），。

## 呼叫 SAML 認證

SAML 認證流程可從 AEM 網站網頁啟動，透過建立特別製作的連結或按鈕。 以下描述的參數可依需求程式設定，例如登入按鈕可設定 `saml_request_path`，使用者在成功 SAML 認證後會被帶到不同的 AEM 頁面，依據按鈕的上下文。

## 使用 SAML 時的安全快取

在 AEM 發佈實例中，大多數頁面通常會被快取。 然而，對於受 SAML 保護的路徑，快取應停用或啟用安全快取，並使用 auth_checker 配置。 欲了解更多資訊，請參閱此處提供的 [詳細資訊](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

請注意，若您在未啟用auth_checker的情況下緩存受保護路徑，可能會遇到不可預測的行為。

### GET 請求

SAML 認證可透過建立格式為以下格式的 HTTP GET 請求來啟動：

`HTTP GET /system/sling/login`

並提供查詢參數：

| 查詢參數名稱 | 查詢參數值 |
|----------------------|-----------------------|
| `resource` | 如[Adobe Granite SAML 2.0 Authentication Handler OSGi設定的](#configure-saml-2-0-authentication-handler) `path`屬性中所定義，任何SAML驗證處理常式監聽的JCR路徑或子路徑。 |
| `saml_request_path` | 成功SAML驗證後，使用者應該前往的URL路徑。 |

例如，這個 HTML 連結會觸發 SAML 登入流程，成功後會帶使用者前往 `/content/wknd/us/en/protected/page.html`。 您可以視需要以程式設計方式設定這些查詢引數。

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST 請求

SAML 認證可透過建立格式為 HTTP POST 請求來啟動：

`HTTP POST /system/sling/login`

並提供表單資料：

| 表單資料名稱 | 表單資料值 |
|----------------------|-----------------------|
| `resource` | 如[Adobe Granite SAML 2.0 Authentication Handler OSGi設定的](#configure-saml-2-0-authentication-handler) `path`屬性中所定義，任何SAML驗證處理常式監聽的JCR路徑或子路徑。 |
| `saml_request_path` | 成功SAML驗證後，使用者應該前往的URL路徑。 |


例如，此HTML按鈕將使用HTTP POST來觸發SAML登入流程，並在成功後將使用者帶至`/content/wknd/us/en/protected/page.html`。 您可以視需要以程式設計方式設定這些表單資料引數。

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher 設定

HTTP GET 與 POST 方法都需要用戶端存取 AEM 端 `/system/sling/login` 點，因此必須透過 AEM Dispatcher 允許。

允許根據是否使用 GET 或 POST 來設定必要的 URL 模式

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
