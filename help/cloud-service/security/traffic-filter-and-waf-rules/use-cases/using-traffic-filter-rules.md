---
title: 使用標準流量篩選器規則保護 AEM 網站
description: 了解如何使用 Adobe 建議的標準流量篩選器規則，在 AEM as a Cloud Service 中保護 AEM 網站不受阻斷服務 (DoS) 攻擊與機器人濫用的侵害。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
exl-id: 5e235220-82f6-46e4-b64d-315f027a7024
source-git-commit: b7f567da159865ff04cb7e9bd4dae0b140048e7d
workflow-type: ht
source-wordcount: '1780'
ht-degree: 100%

---

# 使用標準流量篩選器規則保護 AEM 網站

了解如何使用 _Adobe 建議的_**標準流量篩選器則**，在 AEM as a Cloud Service 中保護 AEM 網站不受阻斷服務 (DoS)、分散式阻斷服務 (DDoS) 攻擊與機器人濫用的侵害。


>[!VIDEO](https://video.tv.adobe.com/v/3469396/?quality=12&learn=on)

## 學習目標

- 審閱 Adobe 建議的標準流量篩選器規則。
- 定義、部署、測試並分析規則的結果。
- 了解根據流量模式調整規則的時機和方式。
- 了解如何使用 AEM 行動中心審閱規則所產生的警示。

### 實作概觀

實作步驟包括：

- 將標準流量篩選器規則新增至 AEM WKND 專案的 `/config/cdn.yaml` 檔案。
- 提交變更並將其推送至 Cloud Manager Git 存放庫。
- 使用 Cloud Manager 的設定管線將變更部署至 AEM 環境。
- 使用 [Vegeta](https://github.com/tsenart/vegeta) 模擬 DoS 攻擊來測試規則
- 使用 AEMCS CDN 記錄和 ELK 儀表板工具分析結果。

## 先決條件

繼續進行之前，請確保您已完成[如何設定流量篩選器和 WAF 規則](../setup.md)教學課程中所述的必要準備工作。此外，您已複製 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的 AEM 環境。

## 規則的關鍵動作

在深入了解標準流量篩選器規則的詳細資料之前，我們先認識這些規則所執行的關鍵動作。每條規則中的 `action` 屬性定義流量篩選器在條件滿足時的回應方式。相關動作包括：

- **記錄**：這些規則會記錄事件以供監視與分析，讓您可以審閱流量模式並視情況所需調整臨界值。這會由 `type: log` 屬性指定。

- **警報**：這些規則在條件滿足時觸發警報，協助您找出潛在的問題。這會由 `alert: true` 屬性指定。

- **封鎖**：這些規則在條件滿足時封鎖流量，以防止存取您的 AEM 網站。這會由 `action: block` 屬性指定。

## 審閱和定義規則

以 Adobe 建議的標準流量篩選器規則做為識別潛在惡意流量模式的基礎層，透過記錄事件 (例如超出 IP 速率限制) 進行監視，並封鎖來自特定國家/地區的流量。這些記錄有助於團隊驗證臨界值和做出明智的決策，以便最終在不干擾合法流量的情況下，**轉換為封鎖模式**&#x200B;規則。

讓我們審閱您應該新增到 AEM WKND 專案 `/config/cdn.yaml` 檔案的三個標準流量篩選器規則：

- **在邊緣防止 DoS 攻擊**：此規則會監視來自用戶端 IP 的每秒要求數 (RPS)，以偵測在 CDN 邊緣可能發生的阻斷服務 (DoS) 攻擊。
- **在來源防止 DoS 攻擊**：此規則會監視來自用戶端 IP 的擷取要求，以偵測來源可能發生的阻斷服務 (DoS) 攻擊。
- **封鎖 OFAC 國家/地區**：此規則會封鎖來自受美國海外資產辦公室 (OFAC) 所限制之特定國家/地區的存取。

### &#x200B;1. 在邊緣防止 DoS 攻擊

此規則會在偵測到可能於 CDN 發生的阻斷服務 (DoS) 攻擊時&#x200B;**發送警報**。當某個使用者在邊緣的每個 CDN POP (網路服務提供點) 超出&#x200B;**每秒 500 個要求** (以 10 秒為平均基準) 時，就會滿足推薦準則條件而觸發此規則。

其計算&#x200B;**所有的**&#x200B;要求，並按用戶端 IP 進行分類。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

`action` 屬性指定該規則應在條件滿足時，記錄事件並觸發警報。因此，那有助於監視潛在的阻斷服務 (DoS) 攻擊，卻不會封鎖合法流量。然而，您的目標是在驗證流量模式並調整臨界值之後，最終將此規則轉換為封鎖模式。

### &#x200B;2. 在來源防止 DoS 攻擊

此規則會在偵測到可能於來源發生的阻斷服務 (DoS) 攻擊時&#x200B;**發送警報**。該推薦準則是某個使用者在來源的每個用戶端 IP 超過&#x200B;**每秒 100 個要求** (以 10 秒為平均基準) 時，即會觸發此規則。

其計算&#x200B;**擷取** (繞過快取的要求) 次數，並按用戶端 IP 進行分組。

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

`action` 屬性指定該規則應在條件滿足時，記錄事件並觸發警報。因此，那有助於監視潛在的阻斷服務 (DoS) 攻擊，卻不會封鎖合法流量。然而，您的目標是在驗證流量模式並調整臨界值之後，最終將此規則轉換為封鎖模式。

### &#x200B;3. 封鎖 OFAC 國家/地區

此規則封鎖受到 [OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information) 限制之特定國家/地區的存取。
您可以視情況所需審閱和修改國家/地區清單。

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

`action` 屬性指定該規則應封鎖來自指定國家/地區的存取。這有助於您防止可能有安全性風險的地區存取您的 AEM 網站。

具有上述規則的完整 `cdn.yaml` 檔案顯示如下：

![WKND CDN YAML 規則](../assets/use-cases/wknd-cdn-yaml-rules.png)

## 部署規則

若要部署上述規則，請依照以下步驟進行：

- 提交變更並將其推送至 Cloud Manager Git 存放庫。

- 使用[先前建立的](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定管線，將變更部署至 AEM 環境。

  ![Cloud Manager 設定管線](../assets/use-cases/cloud-manager-config-pipeline.png)

## 測試規則

為了驗證標準流量篩選器規則在 **CDN Edge** 和&#x200B;**來源**&#x200B;的有效性，可使用多功能的 HTTP 負載測試工具 [Vegeta](https://github.com/tsenart/vegeta) 模擬高要求流量。

- 在邊緣測試 DoS 規則 (500 rps 限制)。以下命令會模擬每秒 200 次要求，持續 15 秒，超出邊緣臨界值 (500 rps)。

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Vegeta DoS 攻擊 Edge](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  注意上述報告中 *100%* 成功和 _200_ 的狀態代碼。由於規則設定為 `log` 和 `alert`，要求&#x200B;_未遭封鎖_&#x200B;但予以記錄以用於監視、分析和發送警報目的。

- 在來源測試 DoS 規則 (100 rps 限制)。以下命令會模擬每秒 110 次擷取要求，持續 1 秒，超出來源臨界值 (100 rps)。為了模擬繞過快取的要求，會建立一個包含唯一查詢參數的 `targets.txt` 檔案，以確保將每個要求都視為擷取要求。

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Vegeta DoS 攻擊來源](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  注意上述報告中 *100%* 成功和 _200_ 的狀態代碼。由於規則設定為 `log` 和 `alert`，要求&#x200B;_未遭封鎖_&#x200B;但予以記錄以用於監視、分析和發送警報目的。

- 為求簡化，此處並未測試 OFAC 規則。

## 審閱警報

在觸發流量篩選器規則時會產生警報。您可以在 [AEM 行動中心](https://experience.adobe.com/aem/actions-center)審閱這些警報。

![WKND AEM 行動中心](../assets/use-cases/wknd-aem-action-center.png)

## 分析結果

您可以使用 AEMCS CDN 記錄和 ELK 儀表板工具分析流量篩選器規則的結果。依照 [CDN 記錄攝取](../setup.md#ingest-cdn-logs)設定部分的指示，將 CDN 記錄攝取至 ELK 堆疊。

您可以在下方的螢幕截圖中，看到 AEM Dev 環境的 CDN 記錄已攝取至 ELK 堆疊。

![WKND CDN 記錄 ELK](../assets/use-cases/wknd-cdn-logs-elk.png)

在 ELK 應用程式中，**CDN 流量儀表板**&#x200B;應該顯示在模擬 DoS 攻擊期間，**Edge** 和&#x200B;**來源**&#x200B;出現的流量峰值。

兩個面板&#x200B;_邊緣每個用戶端 IP 和 POP 的 RPS_&#x200B;和&#x200B;_來源每個用 IP 和 POP 的 RPS_ 分別顯示邊緣和來源的每秒要求數 (RPS)，並依用戶端 IP 與網路服務提供點 (POP) 進行分組。

![WKND CDN 邊緣流量儀表板](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

您也可以使用 CDN 流量儀表板中的其他面板來分析流量模式，例如&#x200B;_熱門用戶端 IP_、_熱門國家/地區_&#x200B;和&#x200B;_熱門使用者代理程式_。這些面板有助於識別潛在威脅，並依此調整流量篩選器規則。

### Splunk 整合

已將 [Splunk 記錄轉送啟用](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)的客戶可以建立新的儀表板來分析流量模式。

若要在 Splunk 中建立儀表板，請依照[用於 AEMCS CDN 記錄分析的 Splunk 儀表板](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis)步驟。

下方的螢幕截圖展示一個 Splunk 儀表板範例，其中顯示每個 IP 的來源與邊緣最大要求數，有助於識別潛在的 DoS 攻擊。

![Splunk 儀表板 - 每個 IP 的最大來源和邊緣要求數](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## 調整改善規則的時機和方式

您的目標是避免封鎖合法流量，同時保護您的 AEM 網站不受潛在的威脅。標準流量篩選器規則是針對警告和記錄 (以及最終在模式切換時封鎖) 威脅所設計，並不會封鎖合法流量。

請考量以下步驟以調整改善規則：

- **監視流量模式**：使用 CDN 記錄和 ELK 儀表板監視流量模式並識別流量的任何異常或尖峰。
- **調整臨界值**：根據流量模式調整規則中的臨界值 (提高或降低速率限制)，以更好的方式符合您的特定要求。例如，如果您注意到合法流量觸發警報，則可以提高速率限制或調整分組方式。
下方表格提供關於如何選擇臨界值的指引：

  | 變化版本 | 值 |
  | :--------- | :------- |
  | 來源 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大來源要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |
  | 邊緣 | 取&#x200B;**正常**&#x200B;流量條件下每個 IP/POP 最大邊緣要求數的最高值 (即不是 DDoS 攻擊時的速率)，然後將其增加數倍 |

  另請參閱[選擇臨界值](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values)部分以了解更多詳細資訊。

- **移至封鎖規則**：在驗證流量模式並調整臨界值之後，就應將規則轉變為封鎖模式。

## 摘要

在本教學課程可以了解如何使用 Adobe 建議的標準流量篩選器規則，在 AEM as a Cloud Service 中保護 AEM 網站不受阻斷服務 (DoS)、分散式阻斷服務 (DDoS) 攻擊與機器人濫用的侵害。

## 建議的 WAF 規則

了解如何實作 Adobe 建議的 WAF 規則，以保護您的 AEM 網站免於使用進階技術來繞過傳統安全措施的複雜威脅。

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="使用 WAF 流量篩選器規則保護 AEM 網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="使用 WAF 流量篩選器規則保護 AEM 網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="使用 WAF 流量篩選器規則保護 AEM 網站">使用 WAF 流量篩選器規則保護 AEM 網站</a>
                    </p>
                    <p class="is-size-6">了解如何使用 Adobe 建議的 Web 應用程式防火牆 (WAF) 流量篩選器規則，在 AEM as a Cloud Service 中保護網站不受包括 DoS、DDoS 及機器人濫用攻擊等在內的複雜威脅。</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">啟動 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 使用案例－超越標準規則

針對較進階的案例，您可以參考以下使用案例，了解如何根據特定業務需求實作自訂的流量篩選器規則。

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="監視敏感性要求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="監視敏感性要求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="監視敏感性要求">監視敏感性要求</a>
                    </p>
                    <p class="is-size-6">了解如何透過使用 AEM as a Cloud Service 中的流量篩選器規則記錄敏感性要求以進行監視。</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="限制存取權" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="限制存取權"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="限制存取權">限制存取權</a>
                    </p>
                    <p class="is-size-6">了解如何使用 AEM as a Cloud Service 中的流量篩選器規則封鎖特定要求以限制存取。</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="標準化要求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="標準化要求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="標準化要求">標準化要求</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中，透過流量篩選器規則轉換要求以進行標準化處理。</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解更多</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 其他資源

- [推薦的入門規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)
