---
title: 流量篩選器規則 (包括 WAF 規則) 的範例與結果分析
description: 了解各種流量篩選器規則 (包括 WAF 規則) 的範例。以及，如何使用 AEM as a Cloud Service (AEMCS) CDN 記錄來分析結果。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1472'
ht-degree: 100%

---

# 流量篩選器規則 (包括 WAF 規則) 的範例與結果分析

了解如何宣告各種類型的流量篩選器規則，以及使用 Adobe Experience Manager as a Cloud Service (AEMCS) CDN 記錄和儀表板工具分析結果。

在本節中，您將探索流量篩選器規則 (包括 WAF 規則) 的實際範例。您將了解如何使用 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)，根據 URI (或路徑)、IP 位址、要求數目和不同的攻擊類型來記錄、允許及封鎖要求。

此外，您還能知道如何使用收錄 AEMCS CDN 記錄的儀表板工具，透過 Adobe 提供的儀表板範例將基本量度視覺化。

若要符合自身的特定需求，您可以增強和建立自訂儀表板，藉此獲得更深入的分析，並將 AEM sites 的規則設定最佳化。

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 範例

讓我們來探索流量篩選器規則 (包括 WAF 規則) 的各種範例。請確保您已完成前面[如何設定](./how-to-setup.md)章節中所述的必要設定流程，並且已原地複製 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)。

### 記錄要求

開始記錄針對 AEM Publish 服務&#x200B;**的 WKND 登入和登出路徑的要求**。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- 提交變更並將其推送至 Cloud Manager Git 存放庫。

- 使用[先前建立的](how-to-setup.md#deploy-rules-through-cloud-manager) Cloud Manager `Dev-Config` 設定管道將變更部署至 AEM Dev 環境。

  ![Cloud Manager 設定管道](./assets/cloud-manager-config-pipeline.png)

- 在 Publish 服務上登入及登出您的方案之 WKND 網站 (例如，`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`) 來測試規則。您可以使用 `asmith/asmith` 作為使用者名稱和密碼。

  ![WKND 登入](./assets/wknd-login.png)

#### 分析{#analyzing}

請從 Cloud Manager 下載 AEMCS CDN 記錄，並使用您在先前章節中所設定的[儀表板工具](how-to-setup.md#analyze-results-using-elk-dashboard-tool)，進行 `publish-auth-requests` 規則的結果分析。

- 從 [Cloud Manager](https://my.cloudmanager.adobe.com/) 的「**環境**」卡片中，下載 AEMCS **Publish** 服務的 CDN 記錄。

  ![Cloud Manager CDN 記錄下載](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    新要求可能要花 5 分鐘才會出現在 CDN 記錄中。

- 將下載的記錄檔案 (例如下面螢幕擷圖中的 `publish_cdn_2023-10-24.log`) 複製到 Elastic 儀表板工具專案的 `logs/dev` 資料夾中。

  ![ELK 工具記錄資料夾](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 重新整理 Elastic 儀表板工具頁面。
   - 在上方的「**全域篩選器**」區段，編輯 `aem_env_name.keyword` 篩選器並選取 `dev` 環境值。

     ![ELK 工具全域篩選器](./assets/elk-tool-global-filter.png)

   - 若要變更時間間隔，按一下右上角的行事曆圖示，然後選取要採用的時間間隔。

     ![ELK 工具時間間隔](./assets/elk-tool-time-interval.png)

- 檢閱更新後儀表板的「**已分析的要求**」、「**已標記的要求**」和「**已標記的要求詳細資訊**」面板。若為符合的 CDN 記錄項目，其應會顯示各項目的用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 的值。

  ![ELK 工具儀表板](./assets/elk-tool-dashboard.png)


### 封鎖要求

在此範例中，請於已部署之 WKND 專案內，在路徑 `/content/wknd/internal` 的&#x200B;_內部_&#x200B;資料夾中新增一個頁面。然後宣告一個流量篩選器規則，將任何地方傳到子頁面的&#x200B;**流量封鎖**，但來自與組織相符的指定 IP 位址 (例如企業 VPN) 者除外。

您可以建立本身的內部頁面 (例如，`demo-page.html`)，或者使用[隨附的套件](./assets/demo-internal-pages-package.zip)。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則：

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- 提交變更並將其推送至 Cloud Manager Git 存放庫。

- 使用[先前建立的](how-to-setup.md#deploy-rules-through-cloud-manager) Cloud Manager `Dev-Config` 設定管道，將變更部署至 AEM Dev 環境。

- 存取 WKND 網站的內部頁面 (例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`) 或使用下方的 CURL 命令，進行規則測試：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 從您在規則中所使用的 IP 位址，以及從另一個 IP 位址 (例如使用行動電話)，重複進行上述步驟。

#### 分析

若要分析 `block-internal-paths` 規則的結果，請依照[先前範例](#analyzing)中所述的相同步驟進行。

不過，這次您應該會看到「**已封鎖的要求**」，及其用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 欄中的對應值。

![ELK 工具儀表板已封鎖的要求](./assets/elk-tool-dashboard-blocked.png)


### 避免 DoS 攻擊

請封鎖來自每秒發出 100 次要求之 IP 位址的要求，以&#x200B;**防止 DoS 攻擊**，該位址會被封鎖 5 分鐘。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下[速率限制流量篩選器規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant#ratelimit-structure)。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>針對生產環境，請與您的網頁安全性團隊合作，確認適當的 `rateLimit` 數值。

- 提交、推送和部署變更，如同[先前範例](#logging-requests)所述。

- 若要模擬 DoS 攻擊，請使用以下的 [Vegeta](https://github.com/tsenart/vegeta) 命令。

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  這項命令會在 5 秒間發出 120 個要求，並輸出報告。如您所見，成功率為 32.5%；其餘皆收到 406 HTTP 回應代碼，表示流量已被封鎖。

  ![Vegeta DoS 攻擊](./assets/vegeta-dos-attack.png)

#### 分析

若要分析 `prevent-dos-attacks` 規則的結果，請依照[先前範例](#analyzing)中所述的相同步驟進行。

這次您應該會看到許多&#x200B;**已封鎖的要求**，及其用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 欄中的對應值。

![ELK 工具儀表板 DoS 要求](./assets/elk-tool-dashboard-dos.png)

此外，**依照用戶端 IP、國家/地區和使用者代理程式劃分的攻擊前 100 名**&#x200B;面板會顯示更多詳細資訊，可用於進一步將規則設定最佳化。

![ELK 工具儀表板前 100 名 DoS 要求](./assets/elk-tool-dashboard-dos-top-100.png)

如需關於如何避免 DoS 與 DDoS 攻擊的更多資訊，請查看[使用流量篩選器規則封鎖 DoS 與 DDoS 攻擊](../blocking-dos-attack-using-traffic-filter-rules.md)教學課程。

### WAF 規則

目前為止的流量篩選器規則範例，所有 Sites 和 Forms 客戶都可以進行設定。

接下來，我們要探討採購增強式安全性或 WAF-DDoS 保護授權之客戶的體驗，客戶可利用這些授權來設定進階規則，保護網站並抵禦更複雜的攻擊。

在繼續進行之前，請為您的方案啟用 WAF-DDoS 保護，如流量篩選器規則文件[設定步驟](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant#setup)中所述。

#### 未使用 WAFFlags

首先，來看看宣告 WAF 規則之前的體驗。當您的方案啟用 WAF-DDoS 時，CDN 記錄預設會記錄下任何符合惡意流量的情況，因此您可以取得正確的資訊來建立合適的規則。

首先，在不新增 WAF 規則 (或使用 `wafFlags` 屬性) 的情況下攻擊 WKND 網站，然後分析結果。

- 若要模擬攻擊，請使用下方的 [Nikto](https://github.com/sullo/nikto) 命令，可在 6 分鐘內發出約 700 個惡意要求。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto 攻擊模擬](./assets/nikto-attack.png)

  若要進一步了解關於攻擊模擬的資訊，請查看 [Nikto：掃描調整](https://github.com/sullo/nikto/wiki/Scan-Tuning)文件，了解如何指定要納入或排除的測試攻擊類型。

##### 分析

若要分析攻擊模擬的結果，請依照[先前範例](#analyzing)中所述的相同步驟進行。

不過，這次您應該會看到「**已標記的要求**」，及其用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 欄中的對應值。您可以運用這些資訊來分析結果，以及將規則設定最佳化。

![ELK 工具儀表板 WAF 已標記的要求](./assets/elk-tool-dashboard-waf-flagged.png)

請注意，「**WAF 標幟分佈**」和「**主要攻擊**」面板會顯示更多詳細資訊，可用於進一步將規則設定最佳化。

![ELK 工具儀表板 WAF 標幟攻擊要求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK 工具儀表板 WAF 主要攻擊要求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### 使用 WAFFlags

現在，新增一個 WAF 規則，在 `action` 屬性中包含 `wafFlags` 屬性，然後&#x200B;**封鎖所模擬的攻擊要求**。

從語法角度來看，WAF 規則與先前所出現的規則類似，但 `action` 屬性會參照一個或多個 `wafFlags` 值。若要了解更多關於 `wafFlags` 的資訊，請參閱 [WAF 標幟清單](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant#waf-flags-list)區段。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則。請注意，`block-waf-flags` 規則包含一些受到模擬的惡意流量攻擊時曾出現在儀表板工具中的 wafFlags。長期持續分析記錄並藉此判斷要宣告哪些新規則確實是很好的做法，因為威脅情勢會不斷演變。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- 提交、推送和部署變更，如同[先前範例](#logging-requests)所述。

- 若要模擬攻擊，請使用與先前相同的 [Nikto](https://github.com/sullo/nikto) 命令。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 分析

重複[先前範例](#analyzing)中所述的相同步驟。

這次您應該會看到「**已封鎖的要求**」之下的項目，及其用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 欄中的對應值。

![ELK 工具儀表板 WAF 已封鎖的要求](./assets/elk-tool-dashboard-waf-blocked.png)

此外，「**WAF 標幟分佈**」和「**主要攻擊**」面板會顯示更多詳細資訊。

![ELK 工具儀表板 WAF 標幟攻擊要求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK 工具儀表板 WAF 主要攻擊要求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 綜合分析

在以上的&#x200B;_分析_&#x200B;區段中，您已了解如何使用儀表板工具分析特定規則的結果。您可以使用其他儀表板面板進一步探索分析結果，包括：


- 已分析、已標記和已封鎖的要求
- 隨時間變化的 WAF 標幟分佈
- 隨時間變化的已觸發流量篩選器規則
- 依照 WAF 標幟 ID 劃分的主要攻擊
- 最常被觸發的流量篩選器
- 依照用戶端 IP、國家/地區和使用者代理程式劃分的前 100 名攻擊者

![ELK 工具儀表板綜合分析](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK 工具儀表板綜合分析](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 下一步

熟悉建議的[最佳實務](./best-practices.md)以降低安全性漏洞的風險。

## 其他資源

[流量篩選器規則語法](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant#rules-syntax)

[CDN 記錄格式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant#cdn-log-format)

