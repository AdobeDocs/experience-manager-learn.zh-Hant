---
title: AEM無頭伺服器到伺服器部署
description: 瞭解伺服器到伺服器無頭部署的部AEM署注意事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---

# AEM無頭伺服器到伺服器部署

無AEM頭伺服器到伺服器部署涉及伺服器端應用程式或進程，這些應用程式或進程以無頭方式消耗和AEM與內容交互。

伺服器到伺服器的部署需要最少的配置，AEM因為到無頭API的HTTP連接不是在瀏覽器上下文中啟動的。

## 部署配置

以下部署配置必須就地用於伺服器到伺服器應用程式部署。

| 伺服器到伺服器應用程式連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源資源共用(CORS) | ✘ | ✘ | ✘ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 授權要求

對GraphQLAEM API的授權請求通常在伺服器到伺服器應用的上下文中發生，因為其他應用類型(如 [單頁應用](./spa.md)。 [移動](./mobile.md)或 [Web元件](./web-component.md)，通常使用授權，因為很難保護憑據。

授權請求到AEMas a Cloud Service時，使用 [基於服務憑據的令牌身份驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html)。 要瞭解有關向as a Cloud Service驗證請求的詳細信AEM息，請查看 [基於令牌的身份驗證教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)。 本教程探討使用 [AEM AssetsHTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) 但與HeadlessGraphQLAPI交互的應用同樣適用AEM於這些概念和方法。

## 伺服器到伺服器應用示例

Adobe提供了Node.js中編碼的伺服器到伺服器應用示例。

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="伺服器到伺服器應用" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="伺服器到伺服器應用">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="伺服器到伺服器應用">伺服器到伺服器應用</a></p>
                   <p class="is-size-6">以Node.js編寫的伺服器到伺服器應用示例，它使用來自無頭GraphQLAPIAEM的內容。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
