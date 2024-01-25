---
title: 如何設定包含WAF規則的流量篩選規則
description: 瞭解如何設定以建立、部署、測試及分析流量篩選器規則（包括WAF規則）的結果。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 321
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 2%

---

# 如何設定包含WAF規則的流量篩選規則

瞭解 **如何設定** 流量篩選規則，包括WAF規則。 閱讀建立、部署、測試和分析結果的相關資訊。

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 設定

設定程式涉及下列專案：

- _建立規則_ 具有適當的AEM專案結構和組態檔。
- _部署規則_ 使用Adobe Cloud Manager的設定管道。
- _測試規則_ 使用各種工具來產生流量。
- _分析結果_ 使用AEMCS CDN記錄檔和控制面板工具。

### 在您的AEM專案中建立規則

若要建立規則，請遵循下列步驟：

1. 在AEM專案的最上層，建立一個資料夾 `config`.

1. 在 `config` 資料夾，建立名為的新檔案 `cdn.yaml`.

1. 將下列中繼資料新增至 `cdn.yaml` 檔案：

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

檢視範例： `cdn.yaml` AEM Guides WKND Sites專案中的檔案：

![WKND AEM專案規則檔案和資料夾](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### 透過Cloud Manager部署規則 {#deploy-rules-through-cloud-manager}

若要部署規則，請遵循下列步驟：

1. 在 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 登入 Cloud Manager 並選取適當的組織和方案。

1. 導覽至 _管道_ 卡來自 _計畫總覽_ 頁面，然後按一下 **+新增** 按鈕並選取所需的配管型別。

   ![Cloud Manager管道卡](./assets/cloud-manager-pipelines-card.png)

   在上述範例中，為了示範之用 _新增非生產管道_ 已選取，因為已使用開發環境。

1. 在 _新增非生產管道_ 對話方塊，選擇並輸入以下詳細資訊：

   1. 設定步驟：

      - **型別**：部署管道
      - **管道名稱**： Dev-Config

      ![Cloud Manager設定管道對話方塊](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. 原始程式碼步驟：

      - **要部署的程式碼**：目標部署
      - **包含**：設定
      - **部署環境**：您的環境名稱，例如wknd-program-dev。
      - **存放庫**：管道應從中擷取程式碼的Git存放庫；例如， `wknd-site`
      - **Git分支**：Git存放庫分支的名稱。
      - **程式碼位置**： `/config`，對應至上一步驟中建立的頂層設定資料夾。

      ![Cloud Manager設定管道對話方塊](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 透過產生流量來測試規則

若要測試規則，有多種可用的協力廠商工具，而且您的組織可能有偏好的工具。 為了示範，讓我們使用下列工具：

- [Curl](https://curl.se/) 用於基本測試，例如叫用URL和檢查回應代碼。

- [韋蓋塔](https://github.com/tsenart/vegeta) 執行拒絕服務(DOS)。 請遵循的安裝指示，從 [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) 以找出潛在的問題和安全漏洞，例如XSS、SQL隱碼攻擊等。 請遵循的安裝指示，從 [Nikto GitHub](https://github.com/sullo/nikto).

- 執行下列命令，確認終端機已安裝並可以使用這些工具：

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

建立、部署和測試規則後，您可以使用以下工具來分析結果 **Elasticsearch、Logstash和Kibana (ELK)** 儀表板工具。 它可以剖析AEMCS CDN記錄，讓您以各種圖表和圖形的形式呈現結果。

控制面板工具可直接從 [AEMCS-CDN-Log-Analysis-ELK-Tool GitHub存放庫](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) 並依照步驟安裝和載入 **流量篩選規則（包括WAF）** 儀表板。

- 載入範例儀表板後，您的Elastic儀表板工具頁面應該如下所示：

  ![ELK流量篩選規則控制面板](./assets/elk-dashboard.png)

>[!NOTE]
>
>    由於尚未擷取AEMCS CDN記錄，因此儀表板是空的。


## 下一步

瞭解如何在中宣告流量篩選規則，包括WAF規則 [範例和結果分析](./examples-and-analysis.md) 章節，使用AEM WKND Sites專案。
