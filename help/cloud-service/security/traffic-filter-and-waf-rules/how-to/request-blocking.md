---
title: 限制存取權
description: 了解如何使用 AEM as a Cloud Service 中的流量篩選器規則封鎖特定要求以限制存取。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 100%

---

# 限制存取權

了解如何使用 AEM as a Cloud Service 中的流量篩選器規則封鎖特定要求以限制存取。

本教學課程示範如何在 AEM Publish 服務中&#x200B;**封鎖來自公共 IP 的內部路徑要求**。

## 封鎖要求的理由和時機

封鎖流量有助於在特定條件下防止存取敏感性資源或網址，進而強化組織的安全性原則。相較於記錄，封鎖是一種更嚴格的行動，應在您確信特定來源的流量未經授權或不受歡迎時使用。

適合進行封鎖的常見案例包括：

- 將 `internal` 或 `confidential` 頁面的存取限制於內部 IP 範圍 (例如：透過企業 VPN)。
- 封鎖機器人流量、自動掃描器或透過 IP 或地理位置識別出的威脅行為者。
- 避免在分階段移轉期間存取已淘汰或不安全的端點。
- 限制對 Publish 層級中的製作工具或管理員路由的存取。

## 先決條件

繼續進行之前，請確保您已完成[如何設定流量篩選器和 WAF 規則](../setup.md)教學課程中所述的必要設定。此外，您已複製 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的 AEM 環境。

## 範例：封鎖公用 IP 的內部路徑

您在此範例中設定了一條規則，封鎖外部存取內部 WKND 頁面，例如來自公用 IP 位址的 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`。只有位於受信任 IP 範圍內的使用者 (例如企業 VPN) 才能存取此頁面。

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

- 使用[先前建立的](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定管道，將變更部署至 AEM 環境。

- 存取 WKND 網站的內部頁面 (例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`) 或使用下方的 CURL 命令，進行規則測試：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 從您在規則中所使用的 IP 位址，以及從另一個 IP 位址 (例如使用行動電話)，重複進行上述步驟。

## 分析

若要分析 `block-internal-paths` 規則的結果，請依照[設定教學課程](../setup.md#cdn-logs-ingestion)中所述的相同步驟進行。

您應該會看到「**已封鎖的要求**」，以及其用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 欄中的對應值。

![ELK 工具儀表板已封鎖的要求](../assets/how-to/elk-tool-dashboard-blocked.png)
