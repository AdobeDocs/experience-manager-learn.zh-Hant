---
title: 流量篩選器規則 (包括 WAF 規則) 的最佳實務
description: 了解流量篩選器規則 (包括 WAF 規則) 的最佳實務建議。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '411'
ht-degree: 100%

---

# 流量篩選器規則 (包括 WAF 規則) 的最佳實務

了解流量篩選器規則 (包括 WAF 規則) 的最佳實務建議。需要注意的是，本文中所述的最佳實務並未包含所有資訊，且不能用於取代您自己的安全性原則和程序。

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 一般最佳實務

- 若要確定哪些規則適合您的組織，請與您的安全性團隊合作。
- 務必先在開發環境中測試各項規則，然後再部署到中繼和生產環境。
- 宣告和驗證規則時，務必從 `action` 類型 `log` 開始，以確保規則不會封鎖合法流量。
- 對於某些規則，從 `log` 到 `block` 的轉換應該純粹根據對足夠網站流量的分析進行。
- 逐步引入規則，並考慮讓您的測試團隊 (QA、效能、滲透測試) 參與這個流程。
- 定期使用[儀表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)分析規則所帶來的影響。根據您網站的流量，可以每天、每週或每月進行一次分析。
- 若要封鎖您可能在分析後發現的惡意流量，新增任何額外的規則。例如，某些一直在攻擊您網站的 IP。
- 規則的建立、部署和分析應該是一個持續且反覆進行的流程，而非一次性的活動。

## 流量篩選器規則的最佳實務

針對您的 AEM 專案啟用以下流量篩選器規則。不過，`rateLimit` 和 `clientCountry` 屬性所要使用的值，必須與您的安全性團隊共同商議決定。

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
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
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
```

>[!WARNING]
>
>針對生產環境，請與您的網頁安全性團隊合作，確認適當的 `rateLimit` 數值

## WAF 規則的最佳實務

當 WAF 已取得授權並在您的方案中啟用後，與 WAF 標幟相符的流量就會出現在圖表和要求記錄中，即使您沒有在規則中宣告亦然。因此，您總是可以發現潛在的新惡意流量，並且根據需要建立規則。查看已宣告規則中未反映的 WAF 標幟，並考慮宣告這些標幟。

您的 AEM 專案可以考慮使用以下 WAF 規則。不過，`action` 和 `wafFlags` 屬性所要使用的值，必須與您的安全性團隊共同商議決定。

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

    # Traffic Filter rules shown in above section
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
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## 摘要

總言之，本教學課程為您提供在 Adobe Experience Manager as a Cloud Service (AEMCS) 中增強網頁應用程式安全性所需的知識和工具。藉由實際的規則範例和對於結果分析的深入解析，您可以有效地保護網站和應用程式。



