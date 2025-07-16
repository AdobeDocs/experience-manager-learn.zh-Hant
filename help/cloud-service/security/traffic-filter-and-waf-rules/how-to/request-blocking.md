---
title: 限制存取
description: 瞭解如何使用AEM as a Cloud Service中的流量篩選規則封鎖特定請求，以限制存取權。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# 限制存取

瞭解如何使用AEM as a Cloud Service中的流量篩選規則封鎖特定請求，以限制存取權。

本教學課程示範如何在AEM發佈服務中&#x200B;**封鎖來自公用IP**&#x200B;的內部路徑請求。

## 封鎖請求的原因與時機

封鎖流量可防止在特定條件下存取敏感資源或URL，有助於強制實施組織安全政策。 與記錄相比，封鎖是更嚴格的動作，當您確信來自特定來源的流量未經授權或不需要時，應使用此動作。

適合封鎖的常見情況包括：

- 將對`internal`或`confidential`頁面的存取限製為僅限內部IP範圍（例如，在公司VPN後面）。
- 封鎖機器人流量、自動化掃描器，或由IP或地理位置識別的威脅行為體。
- 在分段移轉期間防止存取已過時或不安全的端點。
- 在發佈層級中限制對製作工具或管理路由的存取。

## 先決條件

繼續進行之前，請確定您已完成必要的設定，如[如何設定流量篩選器和WAF規則](../setup.md)教學課程中所述。 此外，您已複製[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的AEM環境。

## 範例：封鎖來自公用IP的內部路徑

在此範例中，您設定規則以封鎖來自公用IP位址對內部WKND頁面（例如`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）的外部存取。 只有受信任的IP範圍（例如公司VPN）內的使用者才能存取此頁面。

您可以建立本身的內部頁面 (例如，`demo-page.html`)，或者使用[隨附的套件](../assets/how-to/demo-internal-pages-package.zip)。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
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

- 使用先前建立的AEM設定管道[將變更部署到Cloud Manager環境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 存取 WKND 網站的內部頁面 (例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`) 或使用下方的 CURL 命令，進行規則測試：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 從您在規則中所使用的 IP 位址，以及從另一個 IP 位址 (例如使用行動電話)，重複進行上述步驟。

## 分析

若要分析`block-internal-paths`規則的結果，請依照[設定教學課程](../setup.md#cdn-logs-ingestion)中所述的相同步驟操作

您應該會在使用者端IP (cli_ip)、主機、URL、動作(waf_action)和規則名稱(waf_match)欄中看到&#x200B;**已封鎖的要求**&#x200B;和對應值。

![ELK 工具儀表板已封鎖的要求](../assets/how-to/elk-tool-dashboard-blocked.png)
