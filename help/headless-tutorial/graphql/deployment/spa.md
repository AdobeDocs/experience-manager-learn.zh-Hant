---
title: 為SPAGraphQL部AEM署
description: 瞭解單頁應用(SPA)無頭部署的部AEM署注意事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 1%

---


# 無AEM頭部SPA署


無AEM頭單頁應用(SPA)部署涉及基於JavaScript的應用程式，這些應用程式使用React、Vue或Angular等框架構建，這些框架以無頭方式使用和與AEM內容交互。

以無SPA頭方式AEM交互的部署涉及托管SPA和通過Web瀏覽器訪問。

## 主持SPA

ASPA由本機Web資源集合組成： **HTML、CSS和JavaScript**。 這些資源是在 _構建_ 進程(例如， `npm run build`)並部署到主機供最終用戶使用。

有各種各樣的 **托管** 選項取決於您組織的要求：

1. **雲提供程式** 例如 **Azure** 或 **AWS**。

2. **內部** 在公司里 **資料中心**

3. **前端主機平台** 例如 **AWS放大**。 **Azure應用服務**。 **網路**。 **赫羅庫**。 **韋爾塞爾**&#x200B;的子菜單。

## 部署配置

承載與無頭SPA交互的AEM主要考慮是SPA通過域（或主機）AEM或不同域訪問。  原因是WebSPA瀏覽器中運行的Web應用程式，因此受Web瀏覽器安全策略的影響。

### 共用域

A和SPA共AEM享域（如果兩個域都由同一域的最終用戶訪問）。 例如：

+ 通AEM過： `https://wknd.site/`
+ SPA通過 `https://wknd.site/spa`

由AEM於兩SPA者都從同一域訪問，因此Web瀏覽器SPA允許將XHR設定為無頭端點AEM，而不需要CORS，並允許共用HTTP Cookie(如 `login-token` cookie)。

在共SPA享域AEM上路由流量的方式取決於您：CDN具有多源、HTTP伺服器具有反向代理、SPA直接AEM在中托管等。

以下是生產部署所需SPA的部署配置，當它托管在與相同的域AEM上。

| SPA連接 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源資源共用(CORS) | ✘ | ✘ | ✘ |
| AEM主機 | ✘ | ✘ | ✘ |

### 不同域

ASPA和AEM在最終用戶從不同域訪問它們時具有不同的域。 例如：

+ 通AEM過： `https://wknd.site/`
+ SPA通過 `https://wknd-app.site/`

由AEM於Web瀏覽SPA器可從不同域訪問，因此會實施安全策略，如 [跨源資源共用](./configurations/cors.md)，並防止共用HTTP Cookie(如 `login-token` cookie)。

以下是生產部署所需的部SPA署配置，當托管在與之不同的域AEM上時。

| SPA連接 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 在不同SPA域上部署示例

在本示例中，SPA將部署到Netlify域(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)，並SPA從AEMAEM發佈域使用GraphQL API(`https://publish-p65804-e666805.adobeaemcloud.com`)。 以下螢幕截圖突出了CORS要求。

1. 從SPANetlify域提供，但對不同域上的AEMGraphQL API進行XHR調用。 此跨站點請求需要 [CORS](./configurations/cors.md) 設定，以允AEM許來自Netlify域的請求訪問其內容。

   ![SPA從主機SPA提供AEM ](assets/spa/cors-requirement.png)

2. 檢查GraphQL API的XHRAEM請求， `Access-Control-Allow-Origin` 存在，表示Web瀏覽器允許AEM來自此Netlify域的請求訪問其內容。

   如果 [CORS](./configurations/cors.md) 丟失或未包括Netlify域，Web瀏覽器將失敗XHR請求，並報告CORS錯誤。

   ![CORS響應頭AEMGraphQL API](assets/spa/cors-response-headers.png)

## 單頁應用示例

Adobe提供了在React中編碼的單頁應用程式示例。

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="反應應用" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="反應應用">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="反應應用">反應應用</a></p>
               <p class="is-size-6">用React編寫的單頁應用程式示例，它使用來自無頭GraphQL APIAEM的內容。</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
