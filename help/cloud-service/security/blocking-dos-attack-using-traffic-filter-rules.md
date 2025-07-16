---
title: 使用流量篩選規則封鎖DoS、DDoS和複雜的攻擊
description: 瞭解如何使用AEM as a Cloud Service中的流量篩選規則來封鎖DoS、DDoS和複雜的攻擊。
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 32%

---

# 使用流量篩選規則封鎖DoS、DDoS和複雜的攻擊

瞭解如何在AEM as a Cloud Service (AEMCS)管理的CDN中使用&#x200B;**流量篩選規則**&#x200B;來封鎖阻斷服務(DoS)、分散式阻斷服務(DDoS)和複雜的攻擊。

這些攻擊會導致 CDN，可能還有 AEM Publish 服務 (又稱來源) 出現流量尖峰，並可能影響網站的回應能力和可用性。

本文概述您的AEM網站的預設保護，以及如何透過客戶設定擴充這些保護。 同時也說明如何分析流量模式及設定標準流量篩選規則以封鎖這些攻擊。

## AEM as a Cloud Service中的預設保護

我們來了解 AEM 網站的預設 DDoS 防護：

- **快取：**&#x200B;透過良好的快取原則，DDoS 攻擊的影響會受到更大的限制，因為 CDN 可以防止大多數要求傳送至來源而導致效能下降。
- **自動縮放：** AEM 製作與發佈服務會自動縮放來處理流量尖峰，但仍然可能因為流量突然大幅增加而受到影響。
- **封鎖：**&#x200B;如果來自特定 IP 位址的流量超過 Adobe 根據 CDN PoP (網路服務提供點) 所定義的速率，Adobe CDN 會封鎖流向來源的流量。
- **警報：**&#x200B;當流量超過一定速率時，行動中心會發送來源出現流量尖峰的警報通知。當任何特定 CDN PoP 的流量超過每個 IP 位址上 _Adobe 定義_&#x200B;的要求速率時，便會觸發此警報。如需詳細資訊，請參閱[流量篩選器規則警報](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)。

這些內建的防護措施應被視為組織將 DDoS 攻擊對效能的影響降至最低之能力基準線。由於每個網站都有不同的效能特性，且在達到Adobe定義的速率限制之前可能會看到效能降低，因此建議透過&#x200B;_客戶組態_&#x200B;延伸預設保護。

## 使用流量篩選規則擴充保護

我們來了解一些客戶可以採取的額外建議措施，以便保護他們的網站抵禦 DDoS 攻擊：

- 實作Adobe建議的[標準流量篩選規則](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)，透過記錄可疑行為並發出警報，識別潛在的惡意流量模式。
- 使用&#x200B;**WAF-DDoS保護**&#x200B;或&#x200B;**增強式安全性**&#x200B;附加元件，並實作Adobe建議的[WAF流量篩選規則](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)，以抵禦複雜攻擊，包括使用進階通訊協定或裝載型技術的攻擊。
- 設定[要求轉換](./traffic-filter-and-waf-rules/how-to/request-transformation.md)以忽略不必要的查詢引數，以增加快取涵蓋範圍。

## 開始使用

探索下列教學課程，以設定Adobe建議的規則以封鎖攻擊。

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="如何設定流量篩選規則，包括WAF規則" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="如何設定流量篩選規則，包括WAF規則"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="如何設定流量篩選規則，包括WAF規則">如何設定流量篩選器規則，包括WAF規則</a>
                    </p>
                    <p class="is-size-6">瞭解如何設定以建立、部署、測試及分析流量篩選器規則(包括WAF規則)的結果。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即開始</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="使用標準流量篩選規則保護AEM網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="使用標準流量篩選規則保護AEM網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用標準流量篩選規則保護AEM網站">使用標準流量篩選規則保護AEM網站</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用Adobe在AEM as a Cloud Service中建議的標準流量篩選規則來保護AEM網站免受DoS、DDoS和機器人濫用。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">套用規則</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="使用WAF流量篩選規則保護AEM網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="使用WAF流量篩選規則保護AEM網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用WAF流量篩選規則保護AEM網站">使用WAF流量篩選規則保護AEM網站</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AEM as a Cloud Service中推薦的Web應用程式防火牆(WAF)流量篩選規則，保護AEMAdobe網站免受複雜威脅，包括DoS、DDoS和機器人濫用。</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
