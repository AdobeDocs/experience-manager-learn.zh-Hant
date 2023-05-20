---
title: 無AEM頭部署
description: 瞭解無頭應用的各種部AEM署注意事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# 無AEM頭部署

無AEM頭客戶端部署採用多種形式；托AEM管SPA、外部SPA、網站、移動應用，甚至伺服器到伺服器進程。

根據客戶端及其部署方式，無AEM頭部署有不同的考慮。

## AEM服務架構

在探索部署考慮事項之前，必須了AEM解邏輯體系結構以及as a Cloud Service服務層AEM的分離和角色。 AEMas a Cloud Service包括兩個邏輯服務：

+ __AEM作者__ 是團隊建立、協作和發佈內容片段（和其他資產）的服務。
+ __AEM發佈__ 是已發佈的內容片段（和其他資產）被複製以用於常規消耗的服務。
+ __預AEM覽__ 是模仿AEM Publish行為，但已將內容發佈到它以供預覽或查看的服務。 預AEM覽是針對內部受眾的，而不是一般內容交付。 根據所需AEM的工作流，「預覽」(Preview)的使用是可選的。

![AEM服務架構](./assets/overview/aem-service-architecture.png)

典型AEM的as a Cloud Service無頭部署體系結構_

以生AEM產容量運行的無頭客戶端通常與AEM Publish交互，AEM Publish包含經批准的已發佈內容。 與AEM作者交互的客戶端需要特別小心，因為AEM作者在預設情況下是安全的，需要對所有請求進行授權，並且可能還包含正在進行的工作或未經批准的內容。

## 無頭客戶端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="單頁應用(SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="單頁應用(SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="單頁應用(SPA)">單頁應用(SPA)</a></p>
                   <p class="is-size-6">瞭解單頁應用的部署注意事項(SPA)。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Web元件/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web元件/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web元件/JS">Web元件/JS</a></p>
               <p class="is-size-6">瞭解Web元件和基於瀏覽器的JavaScript無頭使用者的部署注意事項。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="移動應用" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="移動應用">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="移動應用">移動應用</a></p>
               <p class="is-size-6">瞭解移動應用的部署注意事項。</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="伺服器到伺服器應用" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="伺服器到伺服器應用">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="伺服器到伺服器應用">伺服器到伺服器應用</a></p>
               <p class="is-size-6">瞭解伺服器到伺服器應用的部署注意事項</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
