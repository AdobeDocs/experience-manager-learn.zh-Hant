---
title: OpenAPI型AEM API
description: 瞭解OpenAPI型AEM API，包括驗證支援、重要概念，以及如何存取Adobe API。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 58ae9e503bd278479d78d4df6ffe39356d5ec59b
workflow-type: tm+mt
source-wordcount: '1100'
ht-degree: 1%

---

# OpenAPI型AEM API

>[!IMPORTANT]
>
>以OpenAPI為基礎的AEM API僅可在AEM as a Cloud Service中使用，且與AEM 6.X不相容。

瞭解OpenAPI型AEM API，包括驗證支援、重要概念，以及如何存取Adobe API。

[OpenAPI規格](https://swagger.io/specification/) （先前稱為Swagger）是廣泛使用的RESTful API定義標準。 AEM as a Cloud Service提供數種OpenAPI規格型API (或只是OpenAPI型AEM API)，可讓您更輕鬆地建立與AEM的作者或發佈服務型別互動的自訂應用程式。 以下是一些範例：

**個網站**

- [網站API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)：使用內容片段的API。

**Assets**

- [資料夾API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：用於處理資料夾的API，例如建立、列出和刪除資料夾。

- [Assets作者API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)：使用資產及其中繼資料的API。

**Forms**

- [Forms Communications API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)：處理表單與檔案的API。

在未來版本中，將新增更多以OpenAPI為基礎的AEM API，以支援其他使用案例。

## 驗證支援{#authentication-support}

以OpenAPI為基礎的AEM API支援OAuth 2.0驗證，包括下列授權型別：

- **OAuth伺服器對伺服器認證**：適用於需要API存取而不需使用者互動的後端服務。 它使用&#x200B;_client_credentials_&#x200B;授權型別，在伺服器層級啟用安全存取管理。 如需詳細資訊，請參閱[OAuth伺服器對伺服器認證](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)。

- **OAuth Web App認證**：適用於具有代表使用者存取AEM API的前端和&#x200B;_後端_&#x200B;元件的網頁應用程式。 它使用&#x200B;_authorization_code_&#x200B;授權型別，後端伺服器可在此安全地管理密碼和權杖。 如需詳細資訊，請參閱[OAuth Web App認證](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential)。

- **OAuth Single Page App認證**：專為瀏覽器中執行的SPA所設計，需要代表沒有後端伺服器的使用者存取API。 它使用&#x200B;_authorization_code_&#x200B;授權型別，並依賴使用PKCE （代碼交換的證明金鑰）的使用者端安全性機制來保護授權代碼流程。 如需詳細資訊，請參閱[OAuth單頁應用程式認證](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential)。

## 要使用哪種驗證方法{#auth-method-decision}

在決定使用哪種驗證方法時，請考慮下列事項：

![要使用哪個驗證方法？](./assets/overview/which-authentication-method-to-use.png)

每當涉及AEM使用者內容時，使用者驗證（網頁應用程式或單頁應用程式）應為預設選項。 這可確儲存放庫中的所有動作都已正確歸屬於已驗證的使用者，並且使用者限製為僅擁有其有權使用的許可權。
使用伺服器對伺服器（或技術系統帳戶）代表個別使用者執行動作，會略過安全性模型，並帶來許可權提升和不正確稽核等風險。

## OAuth伺服器對伺服器與Web應用程式與單頁應用程式憑證的差異{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

下表總結以OpenAPI為基礎的AEM API所支援的三種OAuth驗證方法之間的差異：

|  | OAuth伺服器對伺服器 | OAuth網頁應用程式 | OAuth單頁應用程式(SPA) |
| --- | --- | --- | --- |
| **驗證目的** | 專為機器對機器互動所設計。 | 專為具有&#x200B;_後端_&#x200B;的網頁應用程式中使用者導向互動所設計。 | 專為&#x200B;_使用者端JavaScript應用程式_&#x200B;中的使用者導向互動所設計。 |
| **權杖行為** | 代表使用者端應用程式本身的存取權杖發生問題。 | 透過後端&#x200B;_為已驗證的使用者_&#x200B;發出存取權杖。 | 透過僅前端流程&#x200B;_為已驗證的使用者_&#x200B;發出存取權杖。 |
| **使用案例** | 需要API存取而不需使用者互動的後端服務。 | 具有代表使用者存取API的前端和後端元件的網頁應用程式。 | 純粹的前端(JavaScript)應用程式代表沒有後端的使用者存取API。 |
| **安全性考量** | 在後端系統中安全地儲存機密認證(`client_id`， `client_secret`)。 | 使用者驗證之後，會透過後端呼叫&#x200B;_授予他們自己的_&#x200B;暫時存取權杖。 在後端系統中安全地儲存機密認證(`client_id`， `client_secret`)，以交換存取權杖的授權碼。 | 使用者驗證之後，會透過前端呼叫&#x200B;_授予他們自己的_&#x200B;暫時存取Token。 不使用`client_secret`，因為存放在前端App是不安全的。 仰賴PKCE交換存取權杖的授權代碼。 |
| **授與型別** | _client_credentials_ | _authorization_code_ | 具有&#x200B;**PKCE**&#x200B;的&#x200B;_authorization_code_ |
| **Adobe Developer Console認證型別** | OAuth伺服器對伺服器 | OAuth網頁應用程式 | OAuth單頁應用程式 |
| **教學課程** | [使用伺服器對伺服器驗證啟動API](./use-cases/invoke-api-using-oauth-s2s.md) | [使用網頁應用程式驗證啟動API](./use-cases/invoke-api-using-oauth-web-app.md) | [使用單頁應用程式驗證啟動API](./use-cases/invoke-api-using-oauth-single-page-app.md) |

## 存取Adobe API和相關概念{#accessing-adobe-apis-and-related-concepts}

在存取Adobe API之前，您必須瞭解下列關鍵建構：

- **[Adobe Developer Console](https://developer.adobe.com/)**：存取Adobe API、SDK、即時事件、無伺服器函式等的開發人員中心。 請注意，它不同於用來偵錯AEM應用程式的&#x200B;_AEM_ Developer Console。

- **[Adobe Developer Console專案](https://developer.adobe.com/developer-console/docs/guides/projects/)**：管理API整合、事件和執行階段函式的中心位置。 在這裡，您可以設定API、設定驗證並產生所需的認證。

- **[產品設定檔](https://helpx.adobe.com/tw/enterprise/using/manage-product-profiles.html)**：產品設定檔提供許可權預設集，可讓您控制使用者或應用程式對Adobe產品(例如AEM、Adobe Target、Adobe Analytics等)的存取權。 每個Adobe產品都有與其相關聯的預定義產品設定檔。

- **服務**：服務會定義實際許可權，並與產品設定檔相關聯。 若要減少或增加許可權預設集，您可以取消選取或選取與產品設定檔關聯的服務。 因此，可讓您控制產品及其API的存取層級。 在AEM as a Cloud Service中，服務代表具有存放庫節點預先定義存取控制清單(ACL)的使用者群組，允許精細的許可權管理。

## 開始

瞭解如何設定您的AEM as a Cloud Service環境和Adobe Developer Console專案，以啟用OpenAPI型AEM API的存取權。 也可以使用瀏覽器存取AEM API，以驗證設定並檢閱要求和回應。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="設定OpenAPI型AEM API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="設定OpenAPI型AEM API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="設定OpenAPI型AEM API">設定OpenAPI型AEM API</a>
                    </p>
                    <p class="is-size-6">瞭解如何設定您的AEM as a Cloud Service環境，以啟用對OpenAPI型AEM API的存取權。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## API教學課程

瞭解如何使用不同的OAuth驗證方法以OpenAPI為基礎的AEM API：

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="使用伺服器對伺服器驗證叫用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="使用伺服器對伺服器驗證叫用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="使用伺服器對伺服器驗證叫用API">使用伺服器對伺服器驗證啟動API</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用OAuth伺服器對伺服器驗證，從自訂NodeJS應用程式叫用OpenAPI型AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="使用網頁應用程式驗證叫用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="使用網頁應用程式驗證叫用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="使用網頁應用程式驗證叫用API">使用網頁應用程式驗證啟動API</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用OAuth網頁應用程式驗證，從自訂網頁應用程式叫用OpenAPI型AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="使用單頁應用程式驗證叫用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="使用單頁應用程式驗證叫用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="使用單頁應用程式驗證叫用API">使用單頁應用程式驗證啟動API</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用OAuth單頁應用程式驗證，從自訂單頁應用程式(SPA)叫用OpenAPI型AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
