---
title: 為AEM GraphQL部署SPA
description: 瞭解單頁應用程式(SPA) AEM Headless部署的部署考量事項。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 5%

---

# AEM Headless SPA部署

AEM Headless單頁應用程式(SPA)部署涉及使用React或Vue等架構建立的JavaScript型應用程式，這些架構可在AEM中以無頭方式使用內容並與內容互動。

部署SPA以無周邊方式與AEM互動，包括託管SPA並讓其可透過網頁瀏覽器存取。

## 託管SPA

SPA由原生網頁資源的集合所組成： **HTML、CSS和JavaScript**。 這些資源會在&#x200B;_組建_&#x200B;程式（例如`npm run build`）期間產生，並部署至主機，以供一般使用者使用。

根據您組織的需求，有各種&#x200B;**託管**&#x200B;選項：

1. **雲端提供者**，例如&#x200B;**Azure**&#x200B;或&#x200B;**AWS**。

2. **在公司**&#x200B;資料中心&#x200B;**內部部署**&#x200B;主控

3. **前端主控平台**，例如&#x200B;**AWS Amplify**、**Azure應用程式服務**、**Netlify**、**Heroku**、**Vercel**&#x200B;等。

## 部署設定

託管與AEM Headless互動的SPA時，主要考量事項為透過AEM的網域（或主機）存取SPA，或是在其他網域上存取。  這是因為SPA是在網頁瀏覽器中執行的網頁應用程式，所以受網頁瀏覽器安全性原則的約束。

### 共用網域

當使用者從相同網域存取兩者時，SPA和AEM會共用網域。 例如：

+ AEM存取方式： `https://wknd.site/`
+ 透過`https://wknd.site/spa`存取SPA

由於AEM和SPA都可從相同網域存取，網頁瀏覽器可讓SPA不需要CORS即可對AEM Headless端點執行XHR，並可共用HTTP Cookie (例如AEM的`login-token` Cookie)。

SPA和AEM流量在共用網域上的路由方式取決於您：具有多種來源的CDN、具有反向Proxy的HTTP伺服器、直接在AEM中託管SPA等等。

以下是SPA生產部署所需的部署設定(在與AEM相同的網域上託管)。

| SPA連線至→ | AEM 作者 | AEM Publish | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| AEM 主機 | ✘ | ✘ | ✘ |

### 不同網域

一般使用者從不同網域存取SPA和AEM時，這兩者會有不同的網域。 例如：

+ AEM存取方式： `https://wknd.site/`
+ 透過`https://wknd-app.site/`存取SPA

由於AEM和SPA可從不同的網域存取，網頁瀏覽器會強制實行安全性原則(例如[跨原始資源共用(CORS)](./configurations/cors.md))，並防止共用HTTP Cookie (例如AEM的`login-token` Cookie)。

以下是SPA生產部署所需的部署設定(託管於AEM以外的網域時)。

| SPA連線至→ | AEM 作者 | AEM Publish | AEM預覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨原始資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 不同網域上的SPA部署範例

在此範例中，SPA會部署至Netlify網域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)，而SPA會使用來自AEM Publish網域(`https://publish-p65804-e666805.adobeaemcloud.com`)的AEM GraphQL API。 以下熒幕擷取畫面會強調顯示CORS需求。

1. SPA是從Netlify網域提供，但會向其他網域上的AEM GraphQL API發出XHR呼叫。 此跨網站要求必須在AEM上設定[CORS](./configurations/cors.md)，以允許來自Netlify網域的要求存取其內容。

   從SPA和AEM主機提供的![SPA要求](assets/spa/cors-requirement.png)

2. 檢查對AEM GraphQL API的XHR要求時，`Access-Control-Allow-Origin`已存在，向網頁瀏覽器表示AEM允許來自此Netlify網域的要求存取其內容。

   如果AEM [CORS](./configurations/cors.md)遺失或不包含Netlify網域，網頁瀏覽器會導致XHR請求失敗，並報告CORS錯誤。

   ![CORS回應標題AEM GraphQL API](assets/spa/cors-response-headers.png)

## 單頁應用程式範例

Adobe提供在React中編碼的範例單頁應用程式。

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="React應用程式 — AEM Headless範例" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="React應用程式 — AEM Headless範例"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="React應用程式 — AEM Headless範例">React應用程式 — AEM Headless範例</a>
                    </p>
                    <p class="is-size-6">應用程式範例是探索 Adobe Experience Manager (AEM) 無周邊功能的好方法。此React應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


