---
title: 如何設定流量篩選規則，包括WAF規則
description: 瞭解如何設定以建立、部署、測試及分析流量篩選器規則(包括WAF規則)的結果。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18306
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 14%

---

# 如何設定流量篩選規則，包括WAF規則

瞭解&#x200B;**如何設定**&#x200B;流量篩選規則，包括網頁應用程式防火牆(WAF)規則。 在本教學課程中，我們會為後續的教學課程建立基礎，您將在其中設定和部署規則，然後測試和分析結果。

為了示範設定程式，教學課程使用[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)。

## 設定概述

後續教學課程的基礎工作涉及以下步驟：

- _在_&#x200B;資料夾的AEM專案中建立規則`config`
- _使用Adobe Cloud Manager設定管道部署規則_。
- _使用Curl、Vegeta和Nikto等工具測試規則_
- _使用AEMCS CDN記錄分析工具分析結果_

## 在 AEM 專案中建立規則

若要在您的AEM專案中定義&#x200B;**標準**&#x200B;和&#x200B;**WAF**&#x200B;流量篩選器規則，請遵循下列步驟：

1. 在您的AEM專案的最上層，建立名為`config`的資料夾。

2. 在`config`資料夾內，建立名為`cdn.yaml`的檔案。

3. 在`cdn.yaml`中使用下列中繼資料結構：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
```

![WKND AEM 專案規則檔案與資料夾](./assets/setup/wknd-rules-file-and-folder.png)

在[下一個教學課程](#next-steps)中，您將瞭解如何將Adobe的&#x200B;**建議標準流量篩選器和WAF規則**&#x200B;新增至上述檔案，作為您實作的堅實基礎。

## 使用Adobe Cloud Manager部署規則

在準備部署規則時，請遵循下列步驟：

1. 登入[my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)並選取您的程式。

2. 從&#x200B;**方案總覽**&#x200B;頁面，移至&#x200B;**管道**&#x200B;卡並按一下&#x200B;**+新增**&#x200B;以建立新管道。

   ![Cloud Manager 管道卡片](./assets/setup/cloud-manager-pipelines-card.png)

3. 在管道精靈中：

   - **類型**：部署管道
   - **管道名稱**：Dev-Config

   ![Cloud Manager 設定管道對話框](./assets/setup/cloud-manager-config-pipeline-step1-dialog.png)

4. Source程式碼設定：

   - **要部署的程式碼**：目標性部署
   - **包含**：設定
   - **部署環境**：例如`wknd-program-dev`
   - **存放庫**： Git存放庫（例如，`wknd-site`）
   - **Git分支**：您的工作分支
   - **代碼位置**： `/config`

   ![Cloud Manager 設定管道對話框](./assets/setup/cloud-manager-config-pipeline-step2-dialog.png)

5. 檢閱管道設定，然後按一下&#x200B;**儲存**。

在[下一個教學課程](#next-steps)中，您將瞭解如何將管道部署至您的AEM環境。

## 使用工具測試規則

若要測試標準流量篩選器和WAF規則的有效性，您可以使用各種工具來模擬請求並分析規則的回應方式。

請確認您在本機電腦上安裝了下列工具，或依照指示進行安裝：

- [Curl](https://curl.se/)：測試要求/回應流程。
- [Vegeta](https://github.com/tsenart/vegeta)：模擬高要求載入（DoS測試）。
- [Nikto](https://github.com/sullo/nikto/wiki)：掃描漏洞。

您可以使用下列指令來驗證安裝：

```shell
# Curl version check
$ curl --version

# Vegeta version check
$ vegeta -version

# Nikto version check
$ cd <PATH-OF-CLONED-REPO>/program
$ ./nikto.pl -Version
```

在[下一個教學課程](#next-steps)中，您將瞭解如何使用這些工具來模擬高要求載入數及惡意要求，以測試流量篩選器和WAF規則的有效性。

## 分析結果

若要準備分析結果，請遵循下列步驟：

1. 安裝&#x200B;**AEMCS CDN Log Analysis Tooling**，使用預先建立的儀表板以視覺化方式分析模式。

2. 從Cloud Manager UI下載記錄檔以執行&#x200B;**CDN記錄擷取**。 或者，您也可以直接將記錄轉送至支援的託管記錄目的地，例如Splunk或Elasticsearch。

### AEMCS CDN記錄分析工具

若要分析流量篩選器和WAF規則的結果，您可以使用&#x200B;**AEMCS CDN記錄分析工具**。 此工具提供預先建立的儀表板，可透過利用從AEMCS CDN收集的記錄，將CDN流量和WAF活動視覺化。

AEMCS CDN Log Analysis Tooling支援兩個可觀察性平台，**ELK** (Elasticsearch、Logstash、Kibana)和&#x200B;**Splunk**。

您可以使用「記錄轉送」功能，將記錄串流至託管的ELK或Splunk記錄服務，安裝控制面板以視覺化方式分析標準流量篩選器和WAF流量篩選器規則。 不過，在本教學課程中，您將會在電腦上安裝的本機ELK執行個體上設定儀表板。

1. 複製[AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)存放庫。

2. 按照[ELK Docker容器安裝指南](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md)在本機安裝和設定ELK棧疊。

3. 使用ELK控制面板，您可以探索IP要求、封鎖的流量、URI模式及安全性警示等量度。

   ![ELK 流量篩選器規則儀表板](./assets/setup/elk-dashboard.png)

>[!NOTE]
> 
> 如果尚未從AEMCS CDN擷取記錄，儀表板會顯示為空白。

### CDN記錄擷取

若要將CDN記錄檔擷取至ELK棧疊，請執行下列步驟：

- 從 [Cloud Manager](https://my.cloudmanager.adobe.com/) 的「**環境**」卡片中，下載 AEMCS **Publish** 服務的 CDN 記錄。

  ![Cloud Manager CDN 記錄下載](./assets/setup/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新要求可能要花 5 分鐘才會出現在 CDN 記錄中。

- 將下載的記錄檔案 (例如下面螢幕擷圖中的 `publish_cdn_2025-06-06.log`) 複製到 Elastic 儀表板工具專案的 `logs/dev` 資料夾中。

  ![ELK 工具記錄資料夾](./assets/setup/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 重新整理 Elastic 儀表板工具頁面。
   - 在上方的「**全域篩選器**」區段，編輯 `aem_env_name.keyword` 篩選器並選取 `dev` 環境值。

     ![ELK 工具全域篩選器](./assets/setup/elk-tool-global-filter.png)

   - 若要變更時間間隔，按一下右上角的行事曆圖示，然後選取要採用的時間間隔。

- 在[下一個教學課程](#next-steps)中，您將瞭解如何使用ELK棧疊中預先建立的儀表板，來分析標準流量篩選器和WAF流量篩選器規則的結果。

  ![ELK工具預先建立的儀表板](./assets/setup/elk-tool-pre-built-dashboards.png)

## 摘要

您已成功為實作流量篩選器規則(包括AEM as a Cloud Service中的WAF規則)奠定基礎。 您建立了組態檔案結構、用於部署的管道，並準備好了測試和分析結果的工具。

## 後續步驟

瞭解如何使用下列教學課程實施Adobe建議的規則：

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
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="使用WAF流量篩選規則保護AEM網站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用WAF流量篩選規則保護AEM網站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用WAF流量篩選規則保護AEM網站">使用WAF流量篩選規則保護AEM網站</a>
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

除了Adobe建議的標準流量篩選器和WAF規則，您可以實作進階情境以達到特定業務需求。 這些情況包括：

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