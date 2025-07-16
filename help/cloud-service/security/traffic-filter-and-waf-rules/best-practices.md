---
title: 流量篩選器規則 (包括 WAF 規則) 的最佳實務
description: 瞭解設定流量篩選器規則的建議最佳實務，包括AEM as a Cloud Service中的WAF規則，以增強安全性並降低風險。
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
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# 流量篩選器規則 (包括 WAF 規則) 的最佳實務

瞭解設定流量篩選器規則的建議最佳實務，包括AEM as a Cloud Service中的WAF規則，以增強安全性並降低風險。

>[!IMPORTANT]
>
>本文中說明的最佳實務並非詳盡無遺，且用意並非要取代您自己的安全性原則和程式。

## 一般最佳實務

- 從Adobe提供的[建議標準流量篩選器和WAF規則集](./overview.md#adobe-recommended-rules)開始，並根據您應用程式的特定需求和威脅狀況進行調整。
- 與您的安全性團隊共同作業，決定哪些規則符合您組織的安全性狀況和法規遵循需求。
- 在將新規則升級到階段和生產環境之前，請一律在開發環境中測試新規則或更新規則。
- 宣告及驗證規則時，從`action`型別`log`開始，在不封鎖合法流量的情況下觀察行為。
- 只有在分析足夠的流量資料並確認沒有有效的請求受到影響後，才能從`log`移至`block`。
- 逐步引入規則，涉及QA、效能和安全性測試團隊，以識別意外的副作用。
- 使用[儀表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)定期檢閱和分析規則有效性。 稽核頻率（每日、每週、每月）應與網站的流量和風險設定檔一致。
- 根據新的威脅情報、流量行為和稽核結果，持續改進規則。

## 流量篩選器規則的最佳實務

- 使用Adobe [建議的標準流量篩選規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)作為基準，其中包括邊緣、來源保護和OFAC限制規則。
- 定期檢閱警報和記錄，以識別不當使用或設定錯誤的模式。
- 根據您應用程式的流量模式和使用者行為調整速率限制的臨界值。

  關於如何選擇臨界值的指引，請參閱下方表格：

  | 變化版本 | 值 |
  | :--------- | :------- |
  | 來源 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大來源要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |
  | 邊緣 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大邊緣要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |

  如需詳細資訊，另請參閱[選擇臨界值](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)區段。

- 只有在確認`block`動作不會影響合法流量後，才移動至`log`動作。

## WAF 規則的最佳實務

- 從Adobe [建議的WAF規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)開始，其中包括封鎖已知不良IP、偵測DDoS攻擊及緩解機器人濫用的規則。
- `ATTACK` WAF標幟應警示您潛在威脅。 在移至`block`之前，請確定沒有誤判。
- 如果建議的WAF規則未涵蓋特定威脅，請考慮根據您應用程式的獨特需求建立自訂規則。 請參閱檔案中的[WAF旗標](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)完整清單。

## 實作規則

瞭解如何在AEM as a Cloud Service中實作流量篩選規則和WAF規則：

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
                    <p class="is-size-6">瞭解如何在AEM as a Cloud Service中使用Adobe建議的Web應用程式防火牆(WAF)規則來保護AEM網站免受複雜威脅，包括DoS、DDoS和機器人濫用。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他資源

- [流量篩選器規則，包括WAF規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [瞭解AEM中的DoS/DDoS預防](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [使用流量篩選規則封鎖DoS和DDoS攻擊](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

