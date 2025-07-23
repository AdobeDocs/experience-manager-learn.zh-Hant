---
title: 標準化要求
description: 了解如何在 AEM as a Cloud Service 中，透過流量篩選器規則轉換要求以進行標準化處理。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '259'
ht-degree: 100%

---

# 標準化要求

了解如何在 AEM as a Cloud Service 中，透過流量篩選器規則轉換要求以進行標準化處理。

## 轉換要求的理由和時機

在希望將進入的流量標準化，並減少由不需要的查詢參數或標頭造成非必要的變異時，要求轉換會非常有用。此技術通常用於：

- 透過移除與 AEM 應用程式無關的快取破壞參數，以提升快取效率。
- 透過將要求排列組合最小化並降低不必要的處理，以保護來源不受濫用。
- 先清理或簡化要求之後，再轉送至 AEM。

這些轉換通常套用於 CDN 層，尤其是用於處理公共流量的 AEM Publish 層級。

## 先決條件

繼續進行之前，請確保您已完成[如何設定流量篩選器和 WAF 規則](../setup.md)教學課程中所述的必要設定。此外，您已複製 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的 AEM 環境。

## 範例：取消設定應用程式不需要的查詢參數

在此範例中，您設定了一條規則，**將移除所有查詢參數，僅保留** `search` 和 `campaignId` 減少快存片段化。

- 在 WKND 專案的 `/config/cdn.yaml` 檔案中新增以下規則。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- 提交變更並將其推送至 Cloud Manager Git 存放庫。

- 使用[先前建立的](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定管線，將變更部署至 AEM 環境。

- 透過存取 WKND 網站的頁面 (例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`) 以測試該規則。

- 在 AEM 記錄中 (`aemrequest.log`)，應該可以看到將要求轉換為 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`，並移除 `otherParam`。

  ![WKND 要求轉換](../assets/how-to/aemrequest-log-transformation.png)

