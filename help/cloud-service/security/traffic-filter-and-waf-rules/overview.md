---
title: 概觀 - 保護 AEM 網站
description: 了解如何透過流量篩選器規則保護您的 AEM 網站不受 DoS、DDoS 與惡意流量的攻擊，包含 AEM as a Cloud Service 中的 Web 應用程式防火牆 WAF 規則子類別。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 100%

---

# 概觀 - 保護 AEM 網站

了解如何使用&#x200B;**流量篩選器規則**&#x200B;保護您的 AEM 網站不受阻斷服務 (DoS)、分散式阻斷服務 (DDoS)、惡意流量和複雜攻擊的侵害，其中包括 AEM as a Cloud Service 中的 **Web 應用程式防火牆 (WAF)** 規則子類別。

您也將了解標準流量篩選器規則與 WAF 流量篩選器規則之間的差別、適用時機，以及如何開始使用 Adobe 建議的規則。

>[!IMPORTANT]
>
> WAF 流量篩選器規則需要額外的 **WAF-DDoS 防護**&#x200B;或&#x200B;**增強安全性**&#x200B;授權。在預設情況下，標準流量篩選器規則可供 Sites 和 Forms 客戶使用。


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## AEM as a Cloud Service 中的流量安全性簡介

AEM as a Cloud Service 運用整合式 CDN 層保護並最佳化網站的內容傳遞。CDN 層最重要其中一個元件是定義和執行流量規則的能力。這些規則做為保護盾，幫助保護您的網站不受濫用、誤用和攻擊，同時又不影響效能。

流量安全性對於維持系統正常運作、保護敏感性資料，以及確保合法使用者獲得順暢體驗非常重要。AEM 提供兩種安全性規則的類別：

- **標準流量篩選器規則**
- **Web 應用程式防火牆 (WAF) 流量篩選器規則**

這些規則集有助於協助客戶防範常見與複雜的網路威脅、降低來自惡意或異常行為用戶端的干擾，並透過要求記錄、封鎖與模式偵測提高可觀察性。

## 標準和 WAF 流量篩選器規則之間的區別

| 功能 | 標準流量篩選器規則 | WAF 流量篩選器規則 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 用途 | 防止濫用行為，例如 DoS、DDoS、網頁爬蟲或機器人活動 | 偵測並因應複雜的攻擊模式 (例如 OWASP 十大安全漏洞)，這也可以防範機器人攻擊 |
| 範例 | 速率限制、地理位置封鎖、使用者代理篩選 | SQL 注入、XSS、已知攻擊 IP |
| 靈活性 | 透過 YAML 的高可設定性 | 藉由預先定義的 WAF 標幟，透過 YAML 的高可設定性 |
| 建議的模式 | 從 `log` 模式開始，然後移至 `block` 模式 | 首先從 `block` 模式 (針對 `ATTACK-FROM-BAD-IP` WAF 標幟) 和 `log` 模式 (針對 `ATTACK` WAF 標誌) 開始，然後針對兩者轉到 `block` 模式 |
| 部署 | 在 YAML 中定義並透過 Cloud Manager 設定管道部署 | 藉由 `wafFlags` 在 YAML 中定義並透過 Cloud Manager 設定管道部署 |
| 授權 | 納入 Sites 和 Forms 授權 | **需要 WAF-DDoS 保護或增強安全性授權證** |

標準流量篩選器規則有助於執行企業特定的原則，例如速率限制或封鎖特定地區，以及根據要求屬性與標頭 (例如 IP 位址、路徑或使用者代理程式) 來封鎖流量。
另一方面，WAF 流量篩選器規則可針對已知的網路漏洞與攻擊向量提供全面且主動的防護，並具備進階智慧功能以降低誤判 (例如：誤封合法流量) 的情況。
若要定義這兩種類型的規則，需要使用 YAML 語法，請參閱[流量篩選器規則語法](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)以了解更多詳細資訊。

## 使用的時機和理由

在以下情況下&#x200B;**使用標準流量篩選器規則**：

- 您想要套用特定於組織的限制，例如 IP 速率限制。
- 您知道需要篩選的特定模式 (例如，惡意的 IP 位址、區域、標題)。

在以下情況下&#x200B;**使用 WAF 流量篩選器規則**：

- 您需要全面的&#x200B;**主動防護**&#x200B;以抵禦廣泛已知的攻擊模式 (例如，注入、通訊協定濫用)，以及從專家資料來源收集的已知惡意 IP。
- 您希望拒絕惡意要求，同時降低誤封合法流量的可能性。
- 您希望透過套用簡單的設定規則，來降低防禦常見與複雜威脅所需的努力。

這些規則共同構成一套縱深防禦策略，使 AEM as a Cloud Service 的客戶能夠採取主動與被動的安全措施，以保護其數位資產。

## Adobe 建議的規則

Adobe 提供了標準流量篩選器和 WAF 流量篩選器規則的建議規則，以協助您迅速保護 AEM 網站的安全。

- **標準流量篩選器規則** (預設可用)：處理常見的濫用案例，例如針對 **CDN 邊緣**、**來源**&#x200B;或來自受制裁國家/地區流量的 DoS、DDoS 和機器人攻擊。\
  例如：
   - 對在 _CDN 邊緣_&#x200B;每秒發出超過 500 次要求的 IP 進行速率限制
   - 對在&#x200B;_來源_&#x200B;每秒發出超過 100 次要求的 IP 進行速率限制
   - 封鎖來自美國海外資產辦公室 (OFAC) 所列的國家/地區流量

- **WAF 流量篩選器規則** (需附加元件授權)：提供對複雜威脅的額外保護，包括像 SQL 注入、跨網站指令碼 (XSS) 和其他 Web 應用程式攻擊等 [OWASP 十大安全漏洞](https://owasp.org/www-project-top-ten/)的威脅。例如：
   - 封鎖來自已知惡意 IP 位址的要求
   - 記錄或封鎖標幟為攻擊的可疑要求

>[!TIP]
>
> 首先套用 **Adobe 建議的規則**，以善用 Adobe 的安全性專業知識與持續更新的防護機制。如果您的企業面臨特定風險或邊緣案例，或發現有誤判 (例如封鎖合法流量)，則可以定義&#x200B;**自訂規則**&#x200B;或擴充預設規則集以符合您的需求。

## 快速入門

透過以下設定指南與使用案例，了解如何在 AEM as a Cloud Service 中定義、部署、測試與分析流量篩選器規則 (包含 WAF 規則)。此提供您背景知識，以便日後可以自信地套用 Adobe 建議的規則。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="如何設定流量篩選器規則 (包括 WAF 規則)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="如何設定流量篩選器規則 (包括 WAF 規則)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="如何設定流量篩選器規則 (包括 WAF 規則)">如何設定流量篩選器規則 (包括 WAF 規則)</a>
                    </p>
                    <p class="is-size-6">了解如何設定、建立、部署、測試和分析流量篩選器規則 (包括 WAF 規則) 的結果。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即開始</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobe 建議的規則設定指南

本指南提供逐步操作說明，協助您 AEM as a Cloud Service 環境中設定，並部署 Adobe 建議的標準流量篩選器規則與 WAF 流量篩選器規則。

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="使用標準流量篩選器規則保護 AEM 網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="使用標準流量篩選器規則保護 AEM 網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用標準流量篩選器規則保護 AEM 網站">使用標準流量篩選器規則保護 AEM 網站</a>
                    </p>
                    <p class="is-size-6">了解如何使用 Adobe 建議的標準流量篩選器規則，在 AEM as a Cloud Service 中保護 AEM 網站不受 DoS、DDoS 攻擊與機器人濫用的侵害。</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">套用規則</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="使用 WAF 規則保護 AEM 網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用 WAF 規則保護 AEM 網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用 WAF 規則保護 AEM 網站">使用 WAF 規則保護 AEM 網站</a>
                    </p>
                    <p class="is-size-6">了解如何使用 Adobe 建議的 Web 應用程式防火牆 (WAF) 流量篩選器規則，在 AEM as a Cloud Service 中保護網站不受包括 DoS、DDoS 及機器人濫用攻擊等在內的複雜威脅。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 進階使用案例

針對較進階的案例，您可以參考以下使用案例，了解如何根據特定業務需求實作自訂的流量篩選器規則。

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="監視敏感性要求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="監視敏感性要求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="監視敏感性要求">監視敏感性要求</a>
                    </p>
                    <p class="is-size-6">了解如何透過使用 AEM as a Cloud Service 中的流量篩選器規則記錄敏感性要求以進行監視。</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="限制存取權" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="限制存取權"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="限制存取權">限制存取權</a>
                    </p>
                    <p class="is-size-6">了解如何使用 AEM as a Cloud Service 中的流量篩選器規則封鎖特定要求以限制存取。</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="標準化要求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="標準化要求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="標準化要求">標準化要求</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中，透過流量篩選器規則轉換要求以進行標準化處理。</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他資源

- [流量篩選規則包括 WAF 規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
