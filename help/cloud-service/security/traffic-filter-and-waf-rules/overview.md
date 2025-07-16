---
title: 概覽 — 保護AEM網站
description: 瞭解如何使用流量篩選規則(包括其在AEM as a Cloud Service中的網頁應用程式防火牆(WAF)規則子類別)，保護您的AEM網站免受DoS、DDoS和惡意流量的傷害。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---


# 概覽 — 保護AEM網站

瞭解如何使用&#x200B;**流量篩選器規則**，包括其在AEM as a Cloud Service中的&#x200B;**Web應用程式防火牆(WAF)**&#x200B;規則的子類別，來保護您的AEM網站免受拒絕服務(DoS)、分散式拒絕服務(DDoS)、惡意流量和複雜的攻擊。

您也會瞭解標準流量篩選器與WAF流量篩選器規則之間的差異、何時使用，以及如何開始使用Adobe建議的規則。

>[!IMPORTANT]
>
> WAF流量篩選規則需要額外的&#x200B;**WAF-DDoS保護**&#x200B;或&#x200B;**增強式安全性**&#x200B;授權。 依預設，Sites和Forms客戶可使用標準流量篩選規則。

## AEM as a Cloud Service的流量安全性簡介

AEM as a Cloud Service運用整合式CDN層來保護和最佳化網站的傳送。 CDN層最關鍵的元件之一，是定義及強制執行流量規則的能力。 這些規則可做為防護盾牌，協助保護您的網站免受濫用、誤用和攻擊，而不會犧牲效能。

流量安全是維持連續運作時間、保護敏感資料，以及確保合法使用者獲得順暢體驗的必要條件。 AEM提供兩種型別的安全性規則：

- **標準流量篩選器規則**
- **Web應用程式防火牆(WAF)流量篩選規則**

規則集可協助客戶防止常見且複雜的Web威脅、減少惡意或行為不當使用者端的雜訊，並透過請求記錄、封鎖和模式偵測來改善可觀察性。

## 標準和WAF流量篩選規則之間的差異

| 功能 | 標準流量篩選規則 | WAF流量篩選規則 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 用途 | 避免濫用，例如DoS、DDoS、刮削或機器人活動 | 偵測複雜攻擊模式並做出反應(例如OWASP Top 10)，這也能防範機器人 |
| 範例 | 速率限制、地理封鎖、使用者代理程式篩選 | SQL插入、XSS、已知的攻擊IP |
| 彈性 | 可透過YAML進行高度設定 | 可透過YAML使用預先定義的WAF旗標進行高度設定 |
| 建議的模式 | 從`log`模式開始，然後移至`block`模式 | 開始為`block`個WAF旗標使用`ATTACK-FROM-BAD-IP`模式，為`log`個WAF旗標使用`ATTACK`模式，然後為兩者移至`block`模式 |
| 部署 | 在YAML中定義並透過Cloud Manager設定管道部署 | 在具有`wafFlags`的YAML中定義並透過Cloud Manager設定管道部署 |
| 授權 | 包含在Sites和Forms授權中 | **需要WAF-DDoS保護或增強式安全性授權** |

標準流量篩選規則可用來強制實施特定業務的原則，例如速率限制或封鎖特定區域，以及根據請求屬性和標頭（例如IP位址、路徑或使用者代理）封鎖流量。
另一方面，WAF流量篩選規則為已知的Web利用漏洞和攻擊向量提供全面的主動保護，並擁有進階智慧來限制誤判（亦即封鎖合法流量）。
若要定義這兩種型別的規則，請使用YAML語法，請參閱[流量篩選規則語法](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)以取得詳細資訊。

## 何時及為何使用它們

**在下列情況下使用標準流量篩選規則**：

- 您想要套用組織專屬的限制，例如IP速率節流。
- 您知道需要篩選的特定模式（例如，惡意IP位址、區域、標題）。

**使用WAF流量篩選規則**，當：

- 您想要從專家資料來源收集全面的&#x200B;**主動式保護**，以防範廣泛的已知攻擊模式（例如，插入、通訊協定濫用）以及已知的惡意IP。
- 您想要拒絕惡意要求，同時限制封鎖合法流量的機會。
- 您想要套用簡單的設定規則，以限制防禦常見複雜威脅的工作量。

這些規則共同提供了深入的防禦策略，可讓AEM as a Cloud Service客戶在保護其數位財產時，同時採取主動和被動的措施。

## Adobe建議的規則

Adobe為標準流量篩選器和WAF流量篩選器規則提供建議規則，以協助您快速保護AEM網站的安全。

- **標準流量篩選規則** （預設可用）：處理常見的不當使用案例，例如&#x200B;**CDN edge**、**origin**&#x200B;遭受DoS、DDoS和機器人攻擊，或來自受制裁國家的流量。\
  例如：
   - 在CDN邊緣&#x200B;_每秒發出超過500個要求_&#x200B;的速率限制IP
   - 在來源&#x200B;_發出超過100個要求/秒_&#x200B;的速率限制IP
   - 封鎖來自外國Assets控制辦公室(OFAC)所列國家的流量

- **WAF流量篩選規則** （需要附加授權）：針對複雜威脅(包括[OWASP十大威脅](https://owasp.org/www-project-top-ten/)，例如SQL插入、跨網站指令碼(XSS)和其他網頁應用程式攻擊)提供額外保護。
例如：
   - 封鎖來自已知錯誤IP位址的請求
   - 記錄或封鎖標籤為攻擊的可疑要求

>[!TIP]
>
> 從套用&#x200B;**Adobe建議的規則**&#x200B;開始，以受益於Adobe的安全性專業知識及持續更新。 若您的企業有特定風險或邊緣案例，或發現任何誤判（封鎖合法流量），您可以定義&#x200B;**自訂規則**&#x200B;或擴充預設集以符合您的需求。

## 快速入門

透過以下設定指南和使用案例，瞭解如何在AEM as a Cloud Service中定義、部署、測試及分析流量篩選規則，包括WAF規則。 這樣可為您提供背景知識，您之後可以放心地套用Adobe建議的規則。

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
                    <a href="./setup.md" title="如何設定流量篩選規則，包括WAF規則" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="如何設定流量篩選規則，包括WAF規則"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="如何設定流量篩選規則，包括WAF規則">如何設定流量篩選器規則，包括WAF規則</a>
                    </p>
                    <p class="is-size-6">瞭解如何設定以建立、部署、測試及分析流量篩選器規則(包括WAF規則)的結果。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">立即開始</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobe建議的規則設定指南

本指南逐步說明如何在您的AEM as a Cloud Service環境中設定和部署Adobe建議的標準流量篩選器和WAF流量篩選器規則。

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="使用標準流量篩選規則保護AEM網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="使用標準流量篩選規則保護AEM網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用標準流量篩選規則保護AEM網站">使用標準流量篩選規則保護AEM網站</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用Adobe在AEM as a Cloud Service中建議的標準流量篩選規則來保護AEM網站免受DoS、DDoS和機器人濫用。</p>
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
                    <a href="./use-cases/using-waf-rules.md" title="使用WAF規則保護AEM網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用WAF規則保護AEM網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用WAF規則保護AEM網站">使用WAF規則保護AEM網站</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AEM as a Cloud Service中推薦的Web應用程式防火牆(WAF)流量篩選規則，保護AEMAdobe網站免受複雜威脅，包括DoS、DDoS和機器人濫用。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 進階使用案例

如需更進階的案例，您可以探索下列使用案例，示範如何根據特定業務需求實作自訂流量篩選規則：

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
                    <a href="./how-to/request-logging.md" title="監控敏感請求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="監控敏感請求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="監控敏感請求">正在監視敏感要求</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AEM as a Cloud Service中的流量篩選規則來記錄敏感請求，以監控這些請求。</p>
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
                    <a href="./how-to/request-blocking.md" title="限制存取" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="限制存取"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="限制存取">限制存取</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AEM as a Cloud Service中的流量篩選規則封鎖特定請求，以限制存取權。</p>
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
                    <a href="./how-to/request-transformation.md" title="標準化請求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="標準化請求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="標準化請求">標準化請求</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AEM as a Cloud Service中的流量篩選規則轉換請求，以標準化請求。</p>
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

- [流量篩選器規則，包括WAF規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
