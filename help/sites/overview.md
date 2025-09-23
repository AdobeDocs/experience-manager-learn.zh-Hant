---
title: AEM Sites 影片和教學課程
description: 瀏覽關於 Adobe Experience Manager Sites 特性與功能的影片和教學課程。AEM Sites 是業界領先的體驗管理平台。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 2b3ff1957f9da313b71a73492777700ddbd79854
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 38%

---

# AEM Sites 影片和教學課程 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites是Adobe的體驗管理平台，可透過網站、行動應用程式或任何其他數位頻道，提供數位體驗的製作、管理和傳送功能。

## 使用AEM Sites傳遞體驗的三種方式

AEM Sites 提供三種建置、製作和傳遞體驗的方式。無論您是建置網站、最佳化邊緣效能，還是支援無周邊應用程式，AEM Sites都能根據您的專案需求提供彈性的選項：

1. **Edge Delivery Services**&#x200B;體驗使用Adobe的Edge Network以高速且低延遲的方式提供內容。 此服務會自動最佳化使用裝置、搜尋引擎和GenAI代理程式的內容。 作者可使用Adobe通用編輯器或檔案式製作來建立內容。
1. **Headless/API-first**&#x200B;體驗使用AEM Publish，透過行動應用程式、單頁應用程式(SPA)或其他Headless使用者端的HTTP API，以JSON形式傳送內容。 作者使用內容片段編輯器或通用編輯器建立內容。
1. **傳統AEM**&#x200B;體驗會使用AEM發佈以HTML網頁的形式傳送內容。 作者使用AEM作者的頁面編輯器建立內容。 此選項最適合現有專案或已移轉的專案。

所有三個選項都是有效的方法，最佳選擇取決於您的使用案例和組織需求。 每種方法都可讓團隊在任何管道或裝置上，以快速和規模提供個人化、吸引人的體驗。

>[!IMPORTANT]
>
> **Edge Delivery Services**&#x200B;是使用AEM提供網站的最新且最進階的方式。 它結合了Adobe Edge Network的速度和擴充能力，以及現代化的撰寫選項。 雖然我們建議將Edge Delivery Services用於新專案，但AEM Sites仍持續支援Headless和傳統方法，因此您可以選擇最符合您需求的路徑。

下圖說明使用AEM Sites建立體驗的不同選項：

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### 比較使用 AEM Sites 進行建置的方式

以下表格針對這三條路徑進行高層級比較。其聚焦於每條路徑在內容製作和體驗傳遞方面的細微差別。

|            | Edge Delivery Services | 無周邊/API 優先 | 傳統 AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **最適合** | 對流量、效能與擴充性有高度需求的網站 | 行動應用程式、SPA 和其他無周邊應用程式 | 現有專案或移轉的專案 |
| **編寫工具** | 檔案式撰寫、通用編輯器、頁面編輯器 | 內容片段、通用編輯器 | 頁面編輯器、通用編輯器 |
| **編寫的內容存放區** | 文件或 AEM Author (JCR) | AEM Author (JCR) | AEM Author (JCR) |
| **傳遞** | Edge Delivery Services | AEM Publish (透過 Adobe CDN + Dispatcher) | AEM Publish (透過 Adobe CDN + Dispatcher) |
| **傳遞內容存放區** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **傳遞格式** | HTML | JSON | HTML |
| **開發技術** | JavaScript、CSS | 任何 (例如 Swift、React 等) | Java™、HTL、JavaScript、CSS |
| **搜尋機器人和GenAI代理程式支援** | 針對機器人、搜尋引擎和GenAI代理程式最佳化 | 適用於機器人和代理程式，但可能需要SSR或其他設定 | 適用於機器人，但效能可能會比Edge Delivery Services慢 |

## 從AMS或內部部署移轉

如果您要從AMS或內部部署(OTP)移轉至AEM as a Cloud Service，Adobe鼓勵您評估直接移至Edge Delivery Services的方式。 移轉至AEM as a Cloud Service Publish所需的工作通常並不比這更多，但能提供更快速的效能和更大的擴充能力。 如果您認為Edge Delivery Services目前不是適合您的選擇，或是其他方法更符合您的需求，這些方法仍會完整支援您的專案，且是有效的選項。

## 教學課程

深入瞭解使用AEM Sites建立的三種方法。 以下教學課程會逐步引導您瞭解每個選項的運作方式、相關工具及使用時機。

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - 指南" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - 指南"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - 指南">Edge Delivery Services - 指南</a>
                    </p>
                    <p class="is-size-6">透過完整指南探索 Edge Delivery Services。建置、發佈和啟動指南涵蓋您開始使用Edge Delivery Services所需的一切。</p>
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
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="無周邊/API 優先 - 教學課程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="無周邊/API 優先 - 教學課程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="無周邊/API 優先 - 教學課程">無周邊/API 優先 - 教學課程</a>
                    </p>
                    <p class="is-size-6">了解如何建置由 AEM 內容支援的無周邊應用程式。教學課程涵蓋 iOS、Android 和 React 等框架，請根據您的堆疊選擇適合的框架。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="傳統 AEM - WKND 教學課程" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="傳統 AEM - WKND 教學課程"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="傳統 AEM - WKND 教學課程">傳統 AEM - WKND 教學課程</a>
                    </p>
                    <p class="is-size-6">了解如何使用 WKND 教學課程建置 AEM Sites 專案範例。本指南會為您逐步解說專案設定、核心元件、可編輯的範本、用戶端資料庫和元件開發。</p>
                </div>
                <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 其他資源

* [AEM Sites 製作文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites 開發文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites 管理文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites 部署文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service 教學課程](/help/cloud-service/overview.md)
* [AEM Assets 教學課程](/help/assets/overview.md)
* [AEM Forms 教學課程](/help/forms/overview.md)
* [AEM Foundation 教學課程](/help/foundation/overview.md)
