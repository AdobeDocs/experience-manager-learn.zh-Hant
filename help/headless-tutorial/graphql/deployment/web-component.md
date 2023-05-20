---
title: 無AEM頭Web元件部署
description: 瞭解Web元件/純基於JS的無頭部署的部AEM署注意事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# 無AEM頭Web元件部署

無AEM頭 [Web元件](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS部署是純JavaScript應用程式，它在Web瀏覽器中運行，以無頭方式消耗和AEM與內容交互。 Web元件/JS部署與 [SPA部署](./spa.md) 它們不使用強健的SPA框架，並預期被嵌入任何網站的上下文中，從網站傳遞到表面內AEM容。


## 部署配置

Web元件/JS部署必須就地配置以下部署配置。

| Web元件/JS應用程式連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [跨源資源共用(CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 示例Web元件

Adobe提供了Web元件示例。

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Web元件" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Web元件">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Web元件">Web元件</a></p>
                   <p class="is-size-6">以純JavaScript編寫的Web元件示例，它使用來自無頭GraphQLAPIAEM的內容。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
