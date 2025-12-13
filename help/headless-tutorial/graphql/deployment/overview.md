---
title: AEM Headless 部署
description: 了解 AEM Headless 應用程式的各種部署考量。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 100%

---

# AEM Headless 部署

AEM Headless 用戶端部署有多種形式；AEM 託管的 SPA、外部 SPA、網站、行動應用程式，甚至伺服器對伺服器的流程。

根據用戶端及其部署方式，AEM Headless 部署有不同的考量。

## AEM 服務架構

在探討部署考量之前，必須先了解 AEM 的邏輯架構，以及 AEM as a Cloud Service 服務層級的功能區隔和角色。AEM as a Cloud Service 由兩項邏輯服務組成：

+ __AEM Author__ 是團隊建立、協作和發佈內容片段 (及其他資產) 的服務。
+ __AEM Publish__ 是複製已發佈的內容片段 (及其他資產) 以供一般使用的服務。
+ __AEM Preview__ 是模仿 AEM Publish 之行為的服務，但發佈內容至 Preview 是為了預覽或檢閱。AEM Preview 以內部對象為目標，不適用於一般內容傳遞。可以根據所需的工作流程決定是否選用 AEM Preview。

![AEM 服務架構](./assets/overview/aem-service-architecture.png)

典型的 AEM as a Cloud Service 無周邊部署架構_

以生產環境中運作的 AEM Headless 用戶端通常會與 AEM Publish 互動，而 AEM Publish 中包含已核准、已發佈的內容。與 AEM Author 互動的用戶端需要特別小心，因為 AEM Author 預設具有安全性，所有要求都需要授權，且可能包含進行中的工作或尚未核准的內容。

## 無周邊用戶端部署

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="單頁應用程式 (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="單頁應用程式 (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="單頁應用程式 (SPA)">單頁應用程式 (SPA)</a></p>
                   <p class="is-size-6">了解單頁應用程式 (SPA) 的部署考量。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
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
               <a href="./web-component.md" title="網頁元件/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="網頁元件/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="網頁元件/JS">網頁元件/JS</a></p>
               <p class="is-size-6">了解網頁元件和瀏覽器型 JavaScript 無周邊使用者的部署考量。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
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
               <p class="is-size-6">了解行動應用程式的部署考量。</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
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
               <p class="is-size-6">了解伺服器對伺服器應用程式的部署考量</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
