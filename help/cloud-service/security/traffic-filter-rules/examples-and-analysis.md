---
title: 流量篩選器規則(包括WAF規則)的範例和結果分析
description: 瞭解各種流量篩選器規則，包括WAF規則範例。 此外，如何使用AEM as a Cloud Service (AEMCS) CDN記錄來分析結果。
version: Cloud Service
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
source-git-commit: 67091c068634e6c309afaf78942849db626128f6
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 0%

---

# 流量篩選器規則(包括WAF規則)的範例和結果分析

瞭解如何宣告各種型別的流量篩選規則，並使用Adobe Experience Manager as a Cloud Service (AEMCS) CDN記錄和儀表板工具來分析結果。

在本節中，您將探索流量篩選規則的實用範例，包括WAF規則。 您將瞭解如何使用[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)，根據URI （或路徑）、IP位址、要求數及不同的攻擊型別來記錄、允許及封鎖要求。

此外，您將瞭解如何使用儀表板工具來擷取AEMCS CDN記錄，並透過Adobe提供的範例儀表板將基本量度視覺化。

為符合您的特定需求，您可以增強和建立自訂儀表板，進而獲得更深入的見解，並最佳化AEM網站的規則設定。

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 範例

讓我們探索各種流量篩選規則的範例，包括WAF規則。 請確定您已依照先前[如何設定](./how-to-setup.md)章節所述完成必要的設定程式，且您已複製[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)。

### 記錄請求

首先針對AEM Publish服務&#x200B;**記錄WKND登入和登出路徑**&#x200B;的請求。

- 將下列規則新增至WKND專案的`/config/cdn.yaml`檔案。

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

- 提交變更並將其推送到Cloud Manager Git存放庫。

- 使用先前建立的AEM `Dev-Config`設定管道[，將變更部署至Cloud Manager開發環境](how-to-setup.md#deploy-rules-through-cloud-manager)。

  ![Cloud Manager設定管道](./assets/cloud-manager-config-pipeline.png)

- 在Publish服務上登入並登出您程式的WKND網站以測試規則（例如，`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`）。 您可以使用`asmith/asmith`作為使用者名稱和密碼。

  ![WKND登入](./assets/wknd-login.png)

#### 分析{#analyzing}

讓我們從Cloud Manager下載AEMCS CDN記錄檔，並使用您在上一章中設定的[儀表板工具](how-to-setup.md#analyze-results-using-elk-dashboard-tool)，來分析`publish-auth-requests`規則的結果。

- 從[Cloud Manager](https://my.cloudmanager.adobe.com/)的&#x200B;**環境**&#x200B;卡下載AEMCS **發佈**&#x200B;服務的CDN記錄。

  ![Cloud Manager CDN記錄下載](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    新請求可能需要5分鐘的時間才會出現在CDN記錄檔中。

- 將下載的記錄檔（例如底下熒幕擷圖中的`publish_cdn_2023-10-24.log`）複製到Elastic Dashboard工具專案的`logs/dev`資料夾中。

  ![ELK工具記錄檔資料夾](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 重新整理「彈性儀表板」工具頁面。
   - 在前&#x200B;**個全域篩選器**&#x200B;區段中，編輯`aem_env_name.keyword`篩選器並選取`dev`環境值。

     ![ELK工具全域篩選器](./assets/elk-tool-global-filter.png)

   - 若要變更時間間隔，請按一下右上角的日曆圖示，然後選取想要的時間間隔。

     ![ELK工具時間間隔](./assets/elk-tool-time-interval.png)

- 檢閱更新儀表板的&#x200B;**分析請求**、**已標幟的請求**&#x200B;和&#x200B;**已標幟請求詳細資料**&#x200B;面板。 為了比對CDN記錄專案，它應該顯示每個專案的使用者端IP (cli_ip)、主機、url、動作(waf_action)和規則名稱(waf_match)的值。

  ![ELK工具儀表板](./assets/elk-tool-dashboard.png)


### 封鎖要求

在此範例中，讓我們在已部署的WKND專案中位於路徑`/content/wknd/internal`的&#x200B;_內部_&#x200B;資料夾中新增頁面。 然後，宣告流量篩選規則，該規則將&#x200B;**封鎖來自符合您組織（例如公司VPN）的指定IP位址以外任何位置的子頁面的流量**。

您可以建立自己的內部頁面（例如，`demo-page.html`）或使用[附加的封裝](./assets/demo-internal-pages-package.zip)。

- 在WKND專案的`/config/cdn.yaml`檔案中新增下列規則：

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

- 提交變更並將其推送到Cloud Manager Git存放庫。

- 使用Cloud Manager中先前建立的[個](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config`設定管道，將變更部署至AEM開發環境。

- 存取WKND網站的內部頁面（例如`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）或使用下列CURL命令測試規則：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 從規則中使用的IP位址重複上述步驟，然後使用不同的IP位址（例如使用行動電話）。

#### 分析

若要分析`block-internal-paths`規則的結果，請遵循與[先前範例](#analyzing)中說明的相同步驟。

不過，這次您應該會在使用者端IP (cli_ip)、主機、URL、動作(waf_action)和規則名稱(waf_match)欄中看到&#x200B;**封鎖的要求**&#x200B;和對應值。

![ELK工具儀表板封鎖的要求](./assets/elk-tool-dashboard-blocked.png)


### 防止DoS攻擊

讓&#x200B;**透過封鎖IP位址的請求來避免DoS攻擊**，每秒發出100個請求，導致其被封鎖5分鐘。

- 在WKND專案的`/config/cdn.yaml`檔案中新增下列[速率限制流量篩選器規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure)。

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
>針對您的生產環境，請與您的Web安全性團隊共同作業，以決定`rateLimit`的適當值，

- 認可、推播和部署變更，如[先前範例](#logging-requests)中所述。

- 若要模擬DoS攻擊，請使用下列[Vegeta](https://github.com/tsenart/vegeta)命令。

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  此命令會在5秒內發出120個請求並輸出報表。 如您所見，成功率為32.5%；其餘部分會收到406 HTTP回應代碼，表示流量遭封鎖。

  ![Vegeta DoS攻擊](./assets/vegeta-dos-attack.png)

#### 分析

若要分析`prevent-dos-attacks`規則的結果，請遵循與[先前範例](#analyzing)中說明的相同步驟。

這次您應該會在使用者端IP (cli_ip)、主機、URL、動作(waf_action)和規則名稱(waf_match)欄中看到許多&#x200B;**已封鎖的要求**&#x200B;和對應的值。

![ELK工具儀表板DoS要求](./assets/elk-tool-dashboard-dos.png)

此外，使用者端IP、國家/地區和使用者代理程式的&#x200B;**前100名攻擊**&#x200B;面板會顯示其他詳細資料，可用來進一步最佳化規則設定。

![ELK工具儀表板DoS前100個請求](./assets/elk-tool-dashboard-dos-top-100.png)

如需如何防止DoS和DDoS攻擊的詳細資訊，請檢閱[使用流量篩選規則封鎖DoS和DDoS攻擊](../blocking-dos-attack-using-traffic-filter-rules.md)教學課程。

### WAF規則

目前為止的流量篩選器規則範例可供所有Sites和Forms客戶設定。

接下來，讓我們針對已購買增強式安全性或WAF-DDoS保護授權的客戶，探索其體驗，該授權可讓他們設定進階規則，以保護網站免受更複雜的攻擊。

在繼續之前，請按照流量篩選規則檔案[設定步驟](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup)中的說明，為您的程式啟用WAF-DDoS保護。

#### 不含WAFFlags

讓我們在宣告WAF規則之前先看看體驗。 在您的程式中啟用WAF-DDoS後，您的CDN會依預設記錄任何相符的惡意流量，因此您擁有正確的資訊以提出適當的規則。

我們從攻擊WKND網站開始，但不新增WAF規則（或使用`wafFlags`屬性）並分析結果。

- 若要模擬攻擊，請使用下面的[Nikto](https://github.com/sullo/nikto)命令，它會在6分鐘內傳送約700個惡意要求。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto攻擊模擬](./assets/nikto-attack.png)

  若要瞭解攻擊模擬，請檢閱[Nikto - Scan Tuning](https://github.com/sullo/nikto/wiki/Scan-Tuning)檔案，其中會說明如何指定要包含或排除的測試攻擊型別。

##### 分析

若要分析攻擊模擬的結果，請遵循與[先前範例](#analyzing)中所述的相同步驟。

不過，這次您應該會在使用者端IP (cli_ip)、主機、URL、動作(waf_action)和規則名稱(waf_match)欄中看到&#x200B;**標幟的要求**&#x200B;和對應值。 此資訊可讓您分析結果並最佳化規則設定。

![ELK工具儀表板WAF已標幟的請求](./assets/elk-tool-dashboard-waf-flagged.png)

請注意&#x200B;**WAF旗標分佈**&#x200B;和&#x200B;**熱門攻擊**&#x200B;面板如何顯示其他詳細資料，這些詳細資料可用來進一步最佳化規則設定。

![ELK工具儀表板WAF會標籤攻擊要求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK工具儀表板WAF熱門攻擊要求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### 具有WAFFlags

現在，新增包含`wafFlags`屬性的WAF規則做為`action`屬性的一部分，並&#x200B;**封鎖模擬的攻擊要求**。

從語法的角度來看，WAF規則與先前看到的規則類似，不過，`action`屬性會參考一或多個`wafFlags`值。 若要深入瞭解`wafFlags`，請檢閱[WAF旗標清單](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list)區段。

- 在WKND專案的`/config/cdn.yaml`檔案中新增下列規則。 請注意，`block-waf-flags`規則如何包含某些受到模擬惡意流量攻擊時出現在儀表板工具中的wafFlags。 事實上，隨著威脅的演變，長期分析記錄以判斷要宣告哪些新規則是很好的做法。

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

- 認可、推播和部署變更，如[先前範例](#logging-requests)中所述。

- 若要模擬攻擊，請使用與之前相同的[Nikto](https://github.com/sullo/nikto)命令。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 分析

重複與[先前範例](#analyzing)中說明的相同步驟。

這次您應該會在&#x200B;**已封鎖的要求**&#x200B;下看到專案，以及使用者端IP (cli_ip)、主機、URL、動作(waf_action)和規則名稱(waf_match)欄中的對應值。

![ELK工具儀表板WAF已封鎖請求](./assets/elk-tool-dashboard-waf-blocked.png)

此外，**WAF標幟分佈**&#x200B;和&#x200B;**熱門攻擊**&#x200B;面板會顯示其他詳細資料。

![ELK工具儀表板WAF會標籤攻擊要求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK工具儀表板WAF熱門攻擊要求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 全面分析

在上述&#x200B;_分析_&#x200B;區段中，您已瞭解如何使用儀表板工具來分析特定規則的結果。 您可以進一步探索使用其他控制面板面板的分析結果，包括：


- 已分析、已標幟和已封鎖的要求
- WAF會標示一段時間內的分佈
- 一段時間內觸發的流量篩選規則
- 依WAF旗標ID排名最前的攻擊
- 排名在前的觸發流量篩選器
- 依使用者端IP、國家/地區和使用者代理程式區分的前100名攻擊者

![ELK工具儀表板綜合分析](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK工具儀表板綜合分析](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 下一步

熟悉建議的[最佳實務](./best-practices.md)，以降低安全性遭到破壞的風險。

## 其他資源

[流量篩選器規則語法](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[CDN記錄格式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)

