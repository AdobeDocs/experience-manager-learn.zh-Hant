---
title: AEM Sites影片和教學課程
description: Adobe Experience Manager (AEM) Sites是領先的體驗管理平台。 本使用手冊包含了有關AEM Sites許多功能的影片和教學課程。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 5bd752ec257763e6d5da99c3da4b44fa9a43fd25
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 5%

---

# AEM Sites影片和教學課程 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites是領先的體驗管理平台。 本使用手冊包含了有關AEM Sites許多功能的影片和教學課程。

## 使用AEM Sites建立的三種方法

AEM Sites提供三種方法來建置、編寫及傳遞體驗。 無論您是要建立完整頁面、最佳化邊緣效能，還是要支援無周邊應用程式，AEM Sites都能根據您的專案需求提供彈性的選項：

1. **傳統網站**&#x200B;使用AEM Author的頁面編輯器來製作內容，該內容隨後會啟動，並由AEM Publish as HTML網頁傳送給使用者。
1. **Edge Delivery Services**&#x200B;網站利用檔案式製作或Adobe Universal Editor來製作內容，然後啟動內容，再由Edge Delivery Services以HTML網頁的形式傳送給使用者。
1. **Headless/API-first**&#x200B;網頁體驗使用內容片段編輯器或通用編輯器來製作內容，然後啟動該內容，並由AEM Publish以JSON格式傳送。

這些選項是專為滿足行銷組織的各種需求而設計，可跨任何管道或裝置以高速和規模提供個人化、吸引人的體驗。

下圖說明了不同的路徑：

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### 比較使用AEM Sites建置的方式

下表提供這三個路徑的高階比較。 它著重於內容編寫和體驗每個路徑的傳送細微差別。

|            | 傳統網站 | Edge Delivery Services | Headless / API優先 |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **編寫工具** | 頁面編輯器 | 檔案式撰寫，通用編輯器 | 內容片段，通用編輯器 |
| **編寫的內容存放區** | AEM作者(JCR) | 檔案或AEM作者(JCR) | AEM作者(JCR) |
| **傳遞** | AEM發佈(含Adobe CDN + Dispatcher) | Edge Delivery Services | AEM發佈(含Adobe CDN + Dispatcher) |
| **傳遞內容存放區** | AEM發佈(JCR) | Edge Delivery Services | AEM發佈(JCR) |
| **傳遞格式** | HTML | HTML | JSON |
| **開發技術** | Java™、JavaScript、CSS | JavaScript， CSS | 任何（例如Swift、React等） |

## 教學課程

透過下列教學課程，瞭解使用AEM Sites建置的三個路徑中的每一個：

<!-- CARDS

* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional Sites - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional Sites - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="傳統Sites - WKND教學課程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="傳統Sites - WKND教學課程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="傳統Sites - WKND教學課程">傳統網站 — WKND教學課程</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用WKND教學課程建置範例AEM Sites專案。 本指南會逐步引導您瞭解專案設定、核心元件、可編輯的範本、使用者端程式庫和元件開發。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services — 指南" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services — 指南"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services — 指南">Edge Delivery Services — 指南</a>
                    </p>
                    <p class="is-size-6">使用完整的指南探索Edge Delivery Services。 建置、發佈和Launch指南涵蓋您開始使用EDS所需的一切。</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API — 優先 — 教學課程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API — 優先 — 教學課程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API — 優先 — 教學課程">Headless/API — 優先 — 教學課程</a>
                    </p>
                    <p class="is-size-6">瞭解如何建立由AEM內容提供支援的無頭式應用程式。 教學課程涵蓋iOS、Android™和React等架構，選擇適合您棧疊的專案。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 其他資源

* [AEM Sites編寫檔案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites開發檔案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites管理檔案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites部署檔案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service教學課程](/help/cloud-service/overview.md)
* [AEM Assets教學課程](/help/assets/overview.md)
* [AEM Forms教學課程](/help/forms/overview.md)
* [AEM Foundation教學課程](/help/foundation/overview.md)
