---
title: 流量篩選器規則(包括WAF規則)的最佳作法
description: 瞭解流量篩選器規則(包括WAF規則)的建議最佳實務。
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
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# 流量篩選器規則(包括WAF規則)的最佳作法

瞭解流量篩選規則(包括WAF規則)的建議最佳實務。 請務必注意，本文中說明的最佳實務並非詳盡無遺，且用意並非要取代您自己的安全性原則和程式。

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 一般最佳實務

- 若要確定哪些規則適合您的組織，請與您的安全性團隊共同作業。
- 在將規則部署到中繼和生產環境之前，請一律在開發環境中測試規則。
- 宣告及驗證規則時，一律以`action`型別`log`開頭，以確保規則不會封鎖合法流量。
- 對於某些規則，從`log`到`block`的轉換應該完全以足夠網站流量的分析為基礎。
- 以漸進方式推出規則，並考慮讓您的測試團隊（QA、效能、滲透測試）參與流程。
- 使用[儀表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)定期分析規則的影響。 根據您網站的流量而定，可每日、每週或每月執行分析。
- 若要封鎖分析後可能察覺到的惡意流量，請新增任何其他規則。 例如，某些攻擊您網站的IP。
- 規則的建立、部署和分析應該是一個持續的、反複的過程。 這不是一次性活動。

## 流量篩選規則的最佳實務

為您的AEM專案啟用下列流量篩選規則。 但是，`rateLimit`和`clientCountry`屬性的所需值必須透過與您的安全性團隊共同作業來決定。

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
>針對您的生產環境，請與您的Web安全性團隊共同作業，以決定`rateLimit`的適當值

## WAF規則的最佳作法

在您的方案授權並啟用WAF後，即使使用者未在規則中宣告流量，符合WAF旗標的流量也會顯示在圖表和請求記錄中。 因此，您一律會察覺到潛在的新惡意流量，並可視需要建立規則。 檢視未反映在宣告規則中的WAF旗標，並考慮宣告它們。

請考量下列WAF規則中適用於您的AEM專案。 但是，`action`和`wafFlags`屬性的所需值必須透過與您的安全性團隊共同作業來決定。

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

總而言之，本教學課程已為您提供在Adobe Experience Manager as a Cloud Service (AEMCS)中加強網頁應用程式安全性所需的知識和工具。 透過實用的規則範例和對結果分析的深入解析，您可以有效地保護您的網站和應用程式。



