---
title: 部署SPA for AEM GraphQL
description: 了解單頁應用程式(SPA)AEM無頭部署的部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 1%

---


# AEM無頭SPA部署

AEM無頭式單頁應用程式(SPA)部署涉及使用React或Vue等架構建置的JavaScript型應用程式，以無頭式方式使用AEM中的內容並與之互動。

部署以無頭方式與AEM互動的SPA，需要托管SPA，並透過網頁瀏覽器存取。

## 托管SPA

SPA由原生Web資源的集合組成： **HTML、CSS和JavaScript**. 這些資源會在 _建置_ 程式(例如， `npm run build`)並部署至主機，供一般使用者使用。

有各種各樣的 **托管** 選項（視貴組織的需求而定）:

1. **雲端提供者** 例如 **Azure** 或 **AWS**.

2. **內部部署** 在公司中托管 **資料中心**

3. **前端托管平台** 例如 **AWS Amplify**, **Azure應用程式服務**, **Netlify**, **赫羅庫**, **韋爾塞爾**、等

## 部署配置

托管與AEM無頭互動的SPA時，主要考量是透過AEM網域（或主機）或其他網域存取SPA。  原因是SPA是在網頁瀏覽器中執行的網頁應用程式，因此受網頁瀏覽器安全性原則的限制。

### 共用網域

當使用者同時從相同網域存取時，SPA和AEM會共用網域。 例如：

+ AEM可透過： `https://wknd.site/`
+ SPA可透過 `https://wknd.site/spa`

由於AEM和SPA都可從相同網域存取，因此網頁瀏覽器可讓SPA製作XHR至AEM無頭端點，而不需CORS，並允許共用HTTP Cookie(例如AEM) `login-token` cookie)。

SPA和AEM流量在共用網域上的路由方式取決於您：具有多個原始項的CDN、具有反向Proxy的HTTP伺服器、直接在AEM中托管SPA等。

以下是SPA生產部署(托管於與AEM相同的網域)所需的部署設定。

| SPA連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| AEM主機 | ✘ | ✘ | ✘ |

### 不同網域

使用者從不同網域存取SPA和AEM時，兩者會有不同的網域。 例如：

+ AEM可透過： `https://wknd.site/`
+ SPA可透過 `https://wknd-app.site/`

由於AEM和SPA是從不同網域存取，因此網頁瀏覽器會強制執行安全性原則，例如 [跨原始資源共用(CORS)](./configurations/cors.md)，並防止共用HTTP Cookie(例如AEM `login-token` cookie)。

以下是SPA生產部署(托管於AEM以外的其他網域)所需的部署設定。

| SPA連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨原始資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同網域上的SPA部署範例

在此範例中，SPA部署至Netlify網域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)，而SPA會從AEM發佈網域(`https://publish-p65804-e666805.adobeaemcloud.com`)。 以下螢幕擷取畫面強調CORS要求。

1. SPA會從Netlify網域提供，但會在不同網域上對AEM GraphQL API進行XHR呼叫。 此跨網站請求需要 [CORS](./configurations/cors.md) 設定在AEM上，以允許來自Netlify網域的請求存取其內容。

   ![SPA從SPA和AEM主機提供的請求 ](assets/spa/cors-requirement.png)

2. 檢查AEM GraphQL API的XHR請求， `Access-Control-Allow-Origin` 存在，表示AEM允許來自此Netlify網域的請求存取其內容。

   若AEM [CORS](./configurations/cors.md) 遺失或未包含Netlify網域、網頁瀏覽器會失敗XHR請求，並回報CORS錯誤。

   ![CORS回應標題AEM GraphQL API](assets/spa/cors-response-headers.png)

## 單頁應用程式範例

Adobe提供React中編碼的單頁應用程式範例。

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React應用程式">React應用程式</a></p>
               <p class="is-size-6">以React撰寫的單頁應用程式範例，會從AEM無周邊GraphQL API中取用內容。</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Next.js應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Next.js應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Next.js應用程式">Next.js應用程式</a></p>
               <p class="is-size-6">以Next.js撰寫的單頁應用程式範例，會從AEM無周邊GraphQL API中取用內容。</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
