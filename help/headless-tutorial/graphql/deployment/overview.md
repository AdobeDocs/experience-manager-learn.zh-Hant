---
title: AEM無頭部署
description: 了解AEM Headless應用程式的各種部署考量事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM無頭部署

AEM無頭式用戶端部署需採用多種形式；AEM托管的SPA、外部SPA、網站、行動應用程式，甚至伺服器對伺服器程式。

AEM無頭部署會根據用戶端及其部署方式而有不同的考量。

## AEM服務架構

在探索部署考量事項之前，請務必了解AEM邏輯架構，以及AEM as a Cloud Service服務層的分離和角色。 AEMas a Cloud Service包含兩個邏輯服務：

+ __AEM作者__ 是供團隊建立、共同作業及發佈內容片段（和其他資產）的服務。
+ __AEM發佈__ 是已發佈的內容片段（和其他資產）所複製的服務，以供一般使用。
+ __AEM預覽__ 即以行為模擬AEM發佈，但已發佈內容供其預覽或檢閱的服務。 AEM預覽的目的是內部對象，而非一般傳送內容。 視需要的工作流程選擇使用AEM預覽。

![AEM服務架構](./assets/overview/aem-service-architecture.png)

典型AEMas a Cloud Service無頭部署體系結構_

以生產容量運作的AEM無頭式用戶端通常會與AEM Publish互動，後者包含已核准的已發佈內容。 與AEM作者互動的用戶端必須特別小心，因為AEM作者依預設是安全的，需要所有要求的授權，而且可能還包含進行中的工作或未核准的內容。

## 無頭式用戶端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="單頁應用程式(SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="單頁應用程式(SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="單頁應用程式(SPA)">單頁應用程式(SPA)</a></p>
                   <p class="is-size-6">了解單頁應用程式(SPA)的部署考量事項。</p>
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
               <p class="is-size-6">了解Web元件和瀏覽器型JavaScript無頭消費者的部署考量事項。</p>
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
               <a href="./mobile.md" title="行動應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="行動應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="行動應用程式">行動應用程式</a></p>
               <p class="is-size-6">了解行動應用程式的部署考量事項。</p>
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
               <a href="./server-to-server.md" title="伺服器對伺服器應用程式" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="伺服器對伺服器應用程式">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="伺服器對伺服器應用程式">伺服器對伺服器應用程式</a></p>
               <p class="is-size-6">了解伺服器對伺服器應用程式的部署考量事項</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>