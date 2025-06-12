---
title: 如何設定流量篩選器規則 (包括 WAF 規則)
description: 了解如何設定、建立、部署、測試和分析流量篩選器規則 (包括 WAF 規則) 的結果。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '575'
ht-degree: 100%

---

# 如何設定流量篩選器規則 (包括 WAF 規則)

了解&#x200B;**如何設定**&#x200B;流量篩選器規則 (包括 WAF 規則)。閱讀關於建立、部署、測試和分析結果的資訊。

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 設定

設定流程包括以下步驟：

- _建立規則_，透過適當的 AEM 專案結構和設定檔案。
- _部署規則_，使用 Adobe Cloud Manager 的設定管道。
- _測試規則_，使用各種工具來產生流量。
- _分析結果_，使用 AEMCS CDN 記錄和儀表板工具。

### 在 AEM 專案中建立規則

若要建立規則，請依照下列步驟進行：

1. 在 AEM 專案的頂層建立一個 `config` 資料夾。

1. 在 `config` 資料夾內，建立一個名為 `cdn.yaml` 的新檔案。

1. 在 `cdn.yaml` 檔案中新增下列中繼資料：

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
```

查看 AEM Guides WKND 網站專案內的 `cdn.yaml` 檔案範例：

![WKND AEM 專案規則檔案與資料夾](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### 透過 Cloud Manager 部署規則 {#deploy-rules-through-cloud-manager}

若要部署規則，請依照下列步驟進行：

1. 在 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 登入 Cloud Manager，並選取適當的組織和方案。

1. 從「_方案概觀_」頁面導覽至「_管道_」卡片，然後按一下「**+新增**」按鈕，接著選取想要的管道類型。

   ![Cloud Manager 管道卡片](./assets/cloud-manager-pipelines-card.png)

   在上面的範例中，因為使用開發環境，所以基於示範目的選取「_新增非生產管道_」。

1. 在「_新增非生產管道_」對話框中，選擇及輸入以下詳細資訊：

   1. 設定步驟：

      - **類型**：部署管道
      - **管道名稱**：Dev-Config

      ![Cloud Manager 設定管道對話框](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. 原始程式碼步驟：

      - **要部署的程式碼**：目標性部署
      - **包含**：設定
      - **部署環境**：您環境的名稱，例如 wknd-program-dev。
      - **存放庫**：管道應擷取程式碼的 Git 存放庫來源；例如，`wknd-site`
      - **Git 分支**：Git 存放庫分支的名稱。
      - **程式碼位置**：`/config`，對應上一步驟所建立的頂層設定資料夾。

      ![Cloud Manager 設定管道對話框](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 產生流量來測試規則

若要測試規則，有各種第三方工具可供使用，且您的組織可能有偏好的工具。出於示範目的，我們使用以下工具：

- [Curl](https://curl.se/) 用於基本測試，例如叫用 URL 和檢查回應代碼。

- [Vegeta](https://github.com/tsenart/vegeta) 執行阻斷服務 (DOS)。請依照 [Vegeta GitHub](https://github.com/tsenart/vegeta#install) 的安裝指示進行。

- [Nikto](https://github.com/sullo/nikto/wiki) 尋找潛在問題和安全性漏洞，例如 XSS、SQL 注入等。請依照 [Nikto GitHub](https://github.com/sullo/nikto) 的安裝指示進行。

- 執行以下命令來驗證工具是否已安裝並可在終端機中使用：

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### 使用儀表板工具分析結果

建立、部署和測試規則後，您可以使用 **CDN** 記錄和 **AEMCS-CDN-Log-Analysis-Tooling** 分析結果。此工具提供一組儀表板，可以將 Splunk 和 ELK (Elasticsearch、Logstash 和 Kibana) 堆疊的結果以視覺化圖形呈現。

此工具可以從 [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub 存放庫原地複製。然後，請依照指示，針對您偏好的可觀察性工具，安裝並載入 **CDN 流量儀表板**&#x200B;和 **WAF 儀表板**。

在本教學課程中，我們使用 ELK 堆疊。依照[用於 AEMCS CDN 記錄分析之 ELK Docker 容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md)的指示，設定 ELK 堆疊。

- 載入儀表板範例後，您的 Elastic 儀表板工具頁面應如下所示：

  ![ELK 流量篩選器規則儀表板](./assets/elk-dashboard.png)

>[!NOTE]
>
>    由於尚未收錄任何 AEMCS CDN 記錄，因此儀表板內容是空白的。


## 下一步

在[範例與結果分析](./examples-and-analysis.md)章節中，了解如何使用 AEM WKND 網站專案宣告流量篩選器規則 (包括 WAF 規則)。
