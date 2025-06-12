---
title: AEM as a Cloud Service 快取
description: AEM as a Cloud Service 快取的一般概觀。
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '206'
ht-degree: 100%

---

# AEM as a Cloud Service 快取

使用 AEM as a Cloud Service 必須要了解快取。快取包含儲存和重複使用先前擷取的資料，藉以提升系統效率及縮短載入時間。這項機制能顯著加快內容傳遞速度、提升網站效能，並將使用者體驗最佳化。

AEM as a Cloud Service 具有多個快取層，而且 Author 與 Publish 服務之間採取不同的快取策略。

![AEM as a Cloud Service 快取概觀](./assets/overview/all.png){align="center"}

## AEM 快取

AEM as a Cloud Service 具有強大、可設定的多層快取策略，包括 CDN、AEM Dispatcher，也可以選用客戶管理的 CDN。您可以微調跨層快取以發揮最佳效能，確保 AEM 提供最佳體驗。對於 Author 與 Publish 服務的快取，AEM 對不同的顧慮。探索下方各項服務的快取策略。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish 服務" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish 服務快取">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish 服務快取">AEM Publish 服務快取</a></p>
            <p class="is-size-6">AEM Publish 服務使用受管理的 CDN 和 AEM Dispatcher 將一般使用者的網頁體驗最佳化。</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM Author 服務快取" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM Author 服務快取">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM Author 服務快取">AEM Author 服務快取</a></p>
                <p class="is-size-6">AEM Author 服務使用受管理的 CDN 來提供最佳化的製作體驗。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
