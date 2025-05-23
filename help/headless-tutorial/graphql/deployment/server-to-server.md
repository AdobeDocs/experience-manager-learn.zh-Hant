---
title: AEM Headless伺服器對伺服器部署
description: 瞭解伺服器對伺服器AEM Headless部署的部署考量事項。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# AEM Headless伺服器對伺服器部署

AEM Headless伺服器對伺服器部署涉及伺服器端應用程式或程式，這些應用程式或程式會以Headless方式使用並與AEM中的內容互動。

伺服器對伺服器部署需要最少的設定，因為與AEM Headless API的HTTP連線不會在瀏覽器的內容中起始。

## 部署設定

伺服器對伺服器應用程式部署必須具備下列部署設定。

| 伺服器對伺服器應用程式連線到→ | AEM 作者 | AEM 發佈 | AEM預覽 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨原始資源共用(CORS) | ✘ | ✘ | ✘ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 授權需求

對AEM GraphQL API的授權要求通常會發生在伺服器對伺服器應用程式的內容中，因為其他應用程式型別（例如[單頁應用程式](./spa.md)、[行動裝置](./mobile.md)或[網頁元件](./web-component.md)）通常會使用授權，因為很難保護認證。

向AEM as a Cloud Service授權請求時，請使用[服務認證型權杖驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=zh-Hant)。 若要進一步瞭解如何向AEM as a Cloud Service驗證請求，請檢閱[權杖型驗證教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=zh-Hant)。 本教學課程會探索使用[AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=zh-Hant)的權杖式驗證，但相同的概念與方法適用於與AEM Headless GraphQL API互動的應用程式。

## 範例伺服器對伺服器應用程式

Adobe提供Node.js中編碼的伺服器對伺服器應用程式範例。

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="伺服器對伺服器應用程式" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="伺服器對伺服器應用程式">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="伺服器對伺服器應用程式">伺服器對伺服器應用程式</a></p>
                   <p class="is-size-6">以Node.js撰寫的伺服器對伺服器應用程式範例，會使用AEM Headless GraphQL API的內容。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">檢視範例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
