---
title: 部署SPA for AEM GraphQL
description: 瞭解單頁應用程式(SPA) AEM Headless部署的部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 1%

---

# AEM Headless SPA部署

AEM Headless單頁應用程式(SPA)部署涉及使用架構（例如React或Vue）建置的JavaScript型應用程式，這些架構會以Headless方式使用並與AEM中的內容互動。

部署以Headless方式與AEM互動的SPA時，需要託管SPA並使其可透過網頁瀏覽器存取。

## 託管SPA

SPA由一組原生網頁資源組成： **HTML、CSS和JavaScript**. 這些資源會在以下期間產生： _版本編號_ 程式(例如， `npm run build`)並部署至主機，以供一般使用者使用。

有各種 **託管** 選項取決於貴組織的需求：

1. **雲端服務供應商** 例如 **Azure** 或 **AWS**.

2. **內部部署** 在公司內託管 **資料中心**

3. **前端託管平台** 例如 **AWS擴充**， **Azure應用程式服務**， **淨化**， **赫羅庫**， **Vercel**&#x200B;等

## 部署設定

託管與AEM Headless互動的SPA時，主要考量事項為透過AEM網域（或主機）存取SPA，或是在其他網域上存取。  原因是SPA是在網頁瀏覽器中執行的網頁應用程式，因此受網頁瀏覽器安全性原則的約束。

### 共用網域

SPA和AEM會共用網域，當兩者都是由來自相同網域的一般使用者存取時。 例如：

+ AEM的存取方式如下： `https://wknd.site/`
+ SPA存取方式： `https://wknd.site/spa`

由於AEM和SPA都可從相同網域存取，網頁瀏覽器可讓SPA無需CORS即可對AEM Headless端點執行XHR，並可共用HTTP Cookie (例如AEM) `login-token` Cookie)。

SPA和AEM流量在共用網域上的路由方式取決於您：具有多種來源的CDN、具有反向Proxy的HTTP伺服器、直接在AEM中託管SPA等等。

以下是SPA生產部署所需的部署設定(在與AEM相同的網域上託管)。

| SPA連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| AEM主機 | ✘ | ✘ | ✘ |

### 不同網域

當來自不同網域的一般使用者存取SPA和AEM時，這兩個網域會有所不同。 例如：

+ AEM的存取方式如下： `https://wknd.site/`
+ SPA存取方式： `https://wknd-app.site/`

由於AEM和SPA可從不同網域存取，因此網頁瀏覽器會強制實施安全性原則，例如 [跨原始資源共用(CORS)](./configurations/cors.md)，並防止共用HTTP Cookie (例如AEM `login-token` Cookie)。

以下是SPA生產部署所需的部署設定(當託管在不同於AEM的網域時)。

| SPA連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨原始資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同網域上的SPA部署範例

在此範例中，SPA會部署至Netlify網域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)和SPA會取用來自AEMGraphQL AEM發佈網域(`https://publish-p65804-e666805.adobeaemcloud.com`)。 以下熒幕擷取畫面會強調顯示CORS需求。

1. SPA是從Netlify網域提供，但會向不同網域上的AEM GraphQL API發出XHR呼叫。 此跨網站要求需要 [CORS](./configurations/cors.md) AEM ，以允許來自Netlify網域的請求存取其內容。

   ![從SPA和AEM主機提供的SPA要求 ](assets/spa/cors-requirement.png)

2. 檢查對AEM GraphQL API的XHR請求， `Access-Control-Allow-Origin` 存在，向網頁瀏覽器表示AEM允許來自此Netlify網域的請求存取其內容。

   如果AEM [CORS](./configurations/cors.md) 遺失或不包含Netlify網域，網頁瀏覽器會導致XHR請求失敗，並回報CORS錯誤。

   ![CORS回應標題AEM GraphQL API](assets/spa/cors-response-headers.png)

## 單頁應用程式範例

Adobe提供React編碼的單頁應用程式範例。

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
               <p class="is-size-6">以React撰寫的範例單頁應用程式，會使用AEM Headless GraphQL API的內容。</p>
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
               <p class="is-size-6">以Next.js撰寫的範例單頁應用程式，會使用AEM Headless GraphQL API的內容。</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
