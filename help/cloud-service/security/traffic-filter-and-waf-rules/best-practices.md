---
title: 流量篩選器規則 (包括 WAF 規則) 的最佳實務
description: 了解在 AEM as a Cloud Service 中設定流量篩選器規則 (包括 WAF 規則) 所建議的最佳實務，以增強安全性並降低風險。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '638'
ht-degree: 100%

---

# 流量篩選器規則 (包括 WAF 規則) 的最佳實務

了解在 AEM as a Cloud Service 中設定流量篩選器規則 (包括 WAF 規則) 所建議的最佳實務，以增強安全性並降低風險。

>[!IMPORTANT]
>
>本文所述的最佳實務並未包含所有資訊，且不能用於取代您自己的安全性原則和程序。

## 一般最佳實務

- 從 Adobe 提供的標準流量篩選器規則與 WAF 規則[建議集](./overview.md#adobe-recommended-rules)開始，並根據應用程式的特定需求與威脅態勢進行調整。
- 與您的安全性團隊協同合作，確定符合組織安全態勢和合規性要求的規則。
- 在將新規則或更新後的規則推送至階段和生產環境之前，請務必先在開發環境中進行測試。
- 在宣告與驗證規則時，請先使用 `action` 類型`log` 觀察行為而不封鎖合法流量。
- 僅在分析足夠的流量資料並確認未影響任何合法要求之後，才從 `log` 移到 `block`。
- 逐步導入規則，並邀請 QA、效能與安全性測試團隊參與，以識別可能產生的非預期副作用。
- 使用[儀表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)定期審閱和分析規則的有效性。審閱頻率 (每日、每週、每月) 應符合網站的流量和風險概況。
- 根據最新的威脅情報、流量行為與稽核結果，持續調整規則。

## 流量篩選器規則的最佳實務

- 使用 Adobe [建議的標準流量篩選器規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)做為基準，其中包括邊緣、來源保護和基於 OFAC 限制等規則。
- 定期審閱警報和記錄以識別濫用或設定錯誤的模式。
- 根據應用程式的流量模式和使用者行為調整速率限制的臨界值。

  關於如何選擇臨界值的指引，請參閱下方表格：

  | 變化版本 | 值 |
  | :--------- | :------- |
  | 來源 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大來源要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |
  | 邊緣 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大邊緣要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |

  另請參閱[選擇臨界值](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)部分以了解更多詳細資訊。

- 移至 `block` 動作 (只有在確認 `log` 動作不會影響合法流量之後)。

## WAF 規則的最佳實務

- 從 Adobe [建議的 WAF 規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)開始，其中包括封鎖已知惡意 IP、偵測 DDoS 攻擊和降低機器人濫用等規則。
- 此 `ATTACK` WAF 標幟應該提醒您注意潛在的威脅。在轉換至 `block` 之前，請確保沒有誤判。
- 如果建議的 WAF 規則未涵蓋特定威脅，請考慮根據應用程式的獨特要求建立自訂規則。請參閱文件中 [WAF 標幟](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)的完整清單。

## 實作規則

了解如何在 AEM as a Cloud Service 中實作流量篩選器規則和 WAF 規則：

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
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
                    <p class="is-size-6">了解如何使用 Adobe 建議的 Web 應用程式防火牆 (WAF) 規則，在 AEM as a Cloud Service 中保護網站不受包括 DoS、DDoS 及機器人濫用攻擊等在內的複雜威脅。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他資源

- [流量篩選規則包括 WAF 規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [了解 AEM 中的 DoS/DDoS 防護](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [使用流量篩選器規則封鎖 DoS 和 DDoS 攻擊](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

