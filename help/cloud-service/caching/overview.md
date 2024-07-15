---
title: AEM as a Cloud Service快取
description: AEM as a Cloud Service快取的一般概觀。
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEM as a Cloud Service快取

在AEM as a Cloud Service中，瞭解快取至關重要。 快取涉及儲存及重複使用先前擷取的資料，以提高系統效率並減少載入時間。 此機制可大幅加快內容傳送、提升網站效能及最佳化使用者體驗。

AEM as a Cloud Service有多個快取層，且Author服務與Publish服務之間的策略也不同。

![AEM as a Cloud Service快取概觀](./assets/overview/all.png){align="center"}

## AEM快取

AEM as a Cloud Service具有強大、可設定的多層快取策略，包括CDN、AEM Dispatcher以及客戶管理的CDN （選用）。 可以微調跨圖層的快取以最佳化效能，確保AEM僅提供最佳體驗。 AEM對Author和Publish服務有不同的快取考量。 探索以下各服務的快取策略。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish服務" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish服務快取">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish服務快取">AEM Publish服務快取</a></p>
            <p class="is-size-6">AEM Publish服務使用受管理的CDN和AEM Dispatcher來最佳化一般使用者網頁體驗。</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM作者服務快取" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM作者服務快取">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM作者服務快取">AEM作者服務快取</a></p>
                <p class="is-size-6">AEM Author服務使用受管理的CDN來提供最佳化的撰寫體驗。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">學習</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
