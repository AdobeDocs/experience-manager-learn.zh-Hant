---
title: AEM API概述
description: 瞭解Adobe Experience Manager (AEM)中的各種API，並瞭解為您的整合選擇哪種API。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 13%

---

# AEM API概述{#aem-apis-overview}

瞭解Adobe Experience Manager (AEM)中的各種API，並瞭解為您的整合選擇哪種API。

若要在AEM中建立、讀取、更新及刪除內容、資產和表單，開發人員可使用各式各樣的API。 這些API可讓開發人員建立與AEM互動的自訂應用程式。

讓我們探索AEM中的不同API型別，並瞭解為您的整合選擇哪種API。

## AEM API的型別{#types-of-aem-apis}

AEM提供下列API，以便與其作者和發佈服務型別互動。

| AEM API型別 | 說明 | 可用性 | 使用案例 | API範例 |
| --- | --- | --- | --- | --- |
| 基於 OpenAPI 的 AEM API | 適用於Assets、Sites和Forms的標準化機器可讀API。 | **僅限AEM as a Cloud Service** | API優先開發，現代應用程式 | [Assets作者API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)、[資料夾API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)、[AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)、[Forms檔案服務API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)及其他 |
| RESTful API | 與AEM資源互動的傳統REST端點。 | AEM 6.X、AEM as a Cloud Service | CRUD作業，現代應用程式 | [Assets HTTP API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)、[工作流程REST API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api)、[內容服務的JSON匯出工具](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)及其他 |
| GRAPHQL API | 透過彈性的查詢，針對有效擷取結構化內容進行最佳化。 | AEM 6.X、AEM as a Cloud Service | Headless CMS、SPA、行動應用程式 | [GraphQL API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| 傳統（非RESTful） API | JCR、Sling模型、查詢產生器等較舊的API。 | AEM 6.X、AEM as a Cloud Service | 舊版整合、回溯相容性 | [查詢產生器API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api)及其他 |

如需詳細資訊，請參閱[Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/)頁面。

## 要選擇哪個API{#which-api-to-choose}

為您的整合選取API時，請考慮下列因素：

- **使用案例**：判斷AEM API是否支援您的使用案例。 儘可能使用OpenAPI式AEM API _，因為這些API提供與AEM互動的標準化現代方法。_&#x200B;如果無法使用OpenAPI型API，請考慮使用RESTful API或GraphQL API，作為最後的手段，使用傳統API。

- **相容性**：確定選取的API與您的AEM版本相容。 例如，_OpenAPI型AEM API是AEM as a Cloud Service_&#x200B;的專屬專案，無法在AEM 6.X中使用。

- **AEM服務型別： Author與Publish**：API的選擇也取決於它是在Author或Publish服務上執行，因為它們的存取模型不同。 AEM Author服務是用於內容建立，一律需要驗證。 AEM Publish服務用於內容傳送，且可能不需要驗證，端視使用案例而定。

- **驗證**：確認API支援您打算使用的驗證方法。 例如：
   - **以OpenAPI為基礎的AEM API**：支援OAuth 2.0驗證，包括使用者端認證（伺服器對伺服器）、授權代碼（網頁應用程式）以及代碼交換（單頁應用程式）授權型別的證明金鑰。 其他AEM API不支援OAuth 2.0驗證。
   - **RESTful API**：支援JSON Web權杖(JWT)驗證，也稱為權杖型驗證。

## JSON Web權杖(JWT)和OAuth 2.0之間的差異{#difference-between-jwt-and-oauth}

讓我們比較JSON Web權杖(JWT)和OAuth 2.0，AEM API中使用的兩種常見驗證機制：

| 功能 | JSON Web權杖(JWT) | OAuth 2.0 |
| --- | --- | --- |
| 使用位置 | RESTful API | 以OpenAPI為基礎的AEM API （RESTful或其他API不支援） |
| 用途 | 服務驗證 | 使用者或服務驗證 |
| 使用者互動 | 不需要使用者互動 | 授權代碼和單頁應用程式授予型別所需的使用者互動 |
| 最適合 | 伺服器對伺服器API呼叫 | 安全且許可的應用程式和使用者存取 |
| 必要資訊 | 簽署JWT的私密金鑰 | OAuth 2.0的使用者端ID和使用者端密碼 |
| 權杖到期 | 期限短，通常需要重新整理 | 存取權杖為短期。 refresh-token是長效的，用於取得新的存取 — token |
| 認證管理 | [AEM Developer Console](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [Adobe Developer Console](https://developer.adobe.com/developer-console/) |

## 基於 OpenAPI 的 AEM API

在[OpenAPI-based AEM API](./openapis/overview.md)指南中進一步瞭解OpenAPI-based AEM API和存取Adobe API的重要概念。

### 使用案例

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png} 
* ./openapis/use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using OAuth Single Page App}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./openapis/assets/spa/OAuth-SPA.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="使用伺服器對伺服器驗證叫用 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="使用伺服器對伺服器驗證叫用 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="使用伺服器對伺服器驗證叫用 API">使用伺服器對伺服器驗證叫用 API</a>
                    </p>
                    <p class="is-size-6">了解如何使用 OAuth 伺服器對伺服器驗證，從自訂 NodeJS 應用程式叫用基於 OpenAPI 的 AEM API。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="使用網頁應用程式驗證叫用 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="使用網頁應用程式驗證叫用 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="使用網頁應用程式驗證叫用 API">使用網頁應用程式驗證叫用 API</a>
                    </p>
                    <p class="is-size-6">了解如何使用 OAuth 網頁應用程式驗證從自訂網頁應用程式叫用基於 OpenAPI 的 AEM API。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using OAuth Single Page App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" title="使用OAuth單頁應用程式叫用API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/spa/OAuth-SPA.png" alt="使用OAuth單頁應用程式叫用API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="使用OAuth單頁應用程式叫用API">使用OAuth單頁應用程式叫用API</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用OAuth 2.0 PKCE流程，從自訂單頁應用程式(SPA)叫用OpenAPI型AEM API。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## GraphQL API — 範例

在[AEM Headless快速入門 — GraphQL](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)中進一步瞭解GraphQL API以及如何使用

### 使用案例

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="單頁應用程式(SPA)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="單頁應用程式(SPA)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="單頁應用程式(SPA)">單頁應用程式(SPA)</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用GraphQL API建立可從AEM擷取內容的單頁應用程式(SPA)。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="行動應用程式" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="行動應用程式"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="行動應用程式">行動應用程式</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用GraphQL API建立從AEM擷取內容的行動應用程式。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Web 元件" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Web 元件"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Web 元件">網頁元件</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用GraphQL API建置可從AEM擷取內容的網頁元件。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## RESTful API — 範例

深入瞭解RESTful API，例如[Assets HTTP API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)和[JSON匯出工具](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)。

### 使用案例

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Using Content Services for Headless App}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Using Content Services for Headless App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="針對Headless應用程式使用Content Services" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="針對Headless應用程式使用Content Services"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="針對Headless應用程式使用Content Services">使用Headless應用程式的內容服務</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用Content Services RESTful API建立可從AEM擷取內容的原生行動應用程式。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="RESTful API的權杖型驗證" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="RESTful API的權杖型驗證"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        RESTful API的<a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="RESTful API的權杖型驗證">權杖型驗證</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用JSON Web權杖(JWT)驗證來叫用RESTful API。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


