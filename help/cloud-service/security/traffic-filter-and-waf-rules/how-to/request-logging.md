---
title: 監控敏感請求
description: 瞭解如何使用AEM as a Cloud Service中的流量篩選規則來記錄敏感請求，以監控這些請求。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 34%

---

# 監控敏感請求

瞭解如何使用AEM as a Cloud Service中的流量篩選規則來記錄敏感請求，以監控這些請求。

記錄可讓您在不影響一般使用者或服務的情況下觀察流量模式，是實作封鎖規則之前重要的第一步。

本教學課程示範如何針對AEM Publish服務&#x200B;**記錄WKND登入和登出路徑的要求**。

## 記錄請求的原因與時間

記錄特定請求是一種低風險、高價值的做法，可讓您瞭解使用者（潛在的惡意行為者）如何與您的AEM應用程式互動。 在強制實施封鎖規則之前，此功能特別有用，可讓您在不中斷合法流量的情況下調整安全性狀態。

紀錄的常見案例包括：

- 在將規則提升為`block`模式之前，驗證規則的影響和範圍。
- 監控登入/登出路徑和驗證端點，以瞭解異常模式或暴力嘗試。
- 追蹤對API端點的高頻存取是否可能濫用或DoS活動。
- 在套用更嚴格的控制項之前，建立機器人行為的基準線。
- 萬一發生安全性事件，請提供鑑證資料，以瞭解攻擊的性質和受影響的資源。

## 先決條件

繼續之前，請確定您已完成必要的設定，如[如何設定流量篩選器和WAF規則](../setup.md)教學課程中所述。 此外，您已複製[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的AEM環境。

## 範例：記錄WKND登入和登出請求

在此範例中，您會建立流量篩選規則，以記錄對AEM Publish服務上WKND登入和登出路徑提出的請求。 它可以協助您監控驗證嘗試，並識別潛在的安全問題。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
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

- 使用先前建立的AEM設定管道[將變更部署到Cloud Manager環境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 登入並登出程式的WKND網站（例如，`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`）以測試規則。 您可以使用 `asmith/asmith` 作為使用者名稱和密碼。

  ![WKND 登入](../assets/how-to/wknd-login.png)

## 分析

讓我們從Cloud Manager下載AEMCS CDN記錄檔並使用`publish-auth-requests`AEMCS CDN記錄檔分析工具[，分析](../setup.md#setup-the-elastic-dashboard-tool)規則的結果。

- 從 [Cloud Manager](https://my.cloudmanager.adobe.com/) 的「**環境**」卡片中，下載 AEMCS **Publish** 服務的 CDN 記錄。

  ![Cloud Manager CDN 記錄下載](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新要求可能要花 5 分鐘才會出現在 CDN 記錄中。

- 將下載的記錄檔案 (例如下面螢幕擷圖中的 `publish_cdn_2023-10-24.log`) 複製到 Elastic 儀表板工具專案的 `logs/dev` 資料夾中。

  ![ELK 工具記錄資料夾](../assets/how-to/elk-tool-logs-folder.png)

- 重新整理 Elastic 儀表板工具頁面。
   - 在上方的「**全域篩選器**」區段，編輯 `aem_env_name.keyword` 篩選器並選取 `dev` 環境值。

     ![ELK 工具全域篩選器](../assets/how-to/elk-tool-global-filter.png)

   - 若要變更時間間隔，按一下右上角的行事曆圖示，然後選取要採用的時間間隔。

     ![ELK 工具時間間隔](../assets/how-to/elk-tool-time-interval.png)

- 檢閱更新後儀表板的「**已分析的要求**」、「**已標記的要求**」和「**已標記的要求詳細資訊**」面板。若為符合的 CDN 記錄項目，其應會顯示各項目的用戶端 IP (cli_ip)、主機、URL、動作 (waf_action) 和規則名稱 (waf_match) 的值。

  ![ELK 工具儀表板](../assets/how-to/elk-tool-dashboard.png)

