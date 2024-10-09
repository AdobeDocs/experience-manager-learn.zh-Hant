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
duration: 136
source-git-commit: f1b13bba9e83ac1d25f2af23ff2673554726eb19
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# AEM Headless SPA部署

AEM Headless單頁應用程式(SPA)部署涉及使用React或Vue等架構建立的JavaScript型應用程式，這些架構會以Headless方式使用並與AEM中的內容互動。

部署以Headless方式與AEM互動的SPA時，需要託管SPA並使其可透過網頁瀏覽器存取。

## 託管SPA

SPA由原生網頁資源的集合所組成： **HTML、CSS和JavaScript**。 這些資源會在&#x200B;_組建_&#x200B;程式（例如`npm run build`）期間產生，並部署至主機，以供一般使用者使用。

根據您組織的需求，有各種&#x200B;**託管**&#x200B;選項：

1. **雲端提供者**，例如&#x200B;**Azure**&#x200B;或&#x200B;**AWS**。

2. **在公司**&#x200B;資料中心&#x200B;**內部部署**&#x200B;主控

3. **前端主控平台**，例如&#x200B;**AWS Amplify**、**Azure應用程式服務**、**Netlify**、**Heroku**、**Vercel**&#x200B;等。

## 部署設定

託管與AEM Headless互動的SPA時，主要考量事項為透過AEM網域（或主機）存取SPA，或是在其他網域上存取。  原因是SPA是在網頁瀏覽器中執行的網頁應用程式，因此受網頁瀏覽器安全性原則的約束。

### 共用網域

SPA和AEM會共用網域，當兩者都是由來自相同網域的一般使用者存取時。 例如：

+ AEM存取： `https://wknd.site/`
+ 已透過`https://wknd.site/spa`存取SPA

由於AEM和SPA都可從相同網域存取，網頁瀏覽器可讓SPA以不需要CORS的方式對AEM Headless端點執行XHR，並允許共用HTTP Cookie (例如AEM `login-token` Cookie)。

SPA和AEM流量在共用網域上的路由方式取決於您：具有多種來源的CDN、具有反向Proxy的HTTP伺服器、直接在AEM中託管SPA等等。

以下是SPA生產部署所需的部署設定(在與AEM相同的網域上託管)。

| SPA連線到→ | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| AEM主機 | ✘ | ✘ | ✘ |

### 不同網域

當來自不同網域的一般使用者存取SPA和AEM時，這兩個網域會有所不同。 例如：

+ AEM存取： `https://wknd.site/`
+ 已透過`https://wknd-app.site/`存取SPA

由於AEM和SPA可從不同的網域存取，網頁瀏覽器會強制實行安全性原則(例如[跨原始資源共用(CORS)](./configurations/cors.md))，並防止共用HTTP Cookie (例如AEM的`login-token` Cookie)。

以下是SPA生產部署所需的部署設定(當託管在不同於AEM的網域時)。

| SPA連線到→ | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨原始資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同網域上的SPA部署範例

在此範例中，SPA部署至Netlify網域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)，而SPA會使用來自AEM Publish網域(`https://publish-p65804-e666805.adobeaemcloud.com`)的AEM GraphQL API。 以下熒幕擷取畫面會強調顯示CORS需求。

1. SPA是從Netlify網域提供，但會向不同網域上的AEM GraphQL API發出XHR呼叫。 此跨網站要求必須在AEM上設定[CORS](./configurations/cors.md)，以允許來自Netlify網域的要求存取其內容。

   從SPA和SPA主機提供的![AEM要求](assets/spa/cors-requirement.png)

2. 檢查對AEM GraphQL API的XHR要求時，`Access-Control-Allow-Origin`會出現，向網頁瀏覽器表示AEM允許來自此Netlify網域的要求存取其內容。

   如果AEM [CORS](./configurations/cors.md)遺失或不包含Netlify網域，網頁瀏覽器會導致XHR要求失敗，並報告CORS錯誤。

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
