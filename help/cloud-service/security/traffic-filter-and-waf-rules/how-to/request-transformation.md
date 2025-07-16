---
title: 標準化請求
description: 瞭解如何使用AEM as a Cloud Service中的流量篩選規則轉換請求，以標準化請求。
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
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# 標準化請求

瞭解如何使用AEM as a Cloud Service中的流量篩選規則轉換請求，以標準化請求。

## 轉換請求的原因與時機

當您想要將傳入的流量標準化，並減少不需要的查詢引數或標題所導致的不必要差異時，請求轉換會很有用。 此技巧通常用於：

- 移除與AEM應用程式無關的防快取引數，以提高快取效率。
- 將請求排列減到最少，並減少不必要的處理，以保護來源不受濫用。
- 在轉送請求至AEM之前，先清理或簡化請求。

這些轉換通常會套用在CDN層，尤其是針對為公共流量服務的AEM Publish層。

## 先決條件

繼續進行之前，請確定您已完成必要的設定，如[如何設定流量篩選器和WAF規則](../setup.md)教學課程中所述。 此外，您已複製[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)並將其部署至您的AEM環境。

## 範例：取消設定應用程式不需要的查詢引數

在此範例中，您設定的規則是&#x200B;**移除除** `search`和`campaignId`以外的所有查詢引數，以減少快取片段。

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

- 使用先前建立的AEM設定管道[將變更部署到Cloud Manager環境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 存取WKND網站的頁面以測試規則，例如`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`。

- 在AEM記錄檔(`aemrequest.log`)中，您應該會看到要求已轉換為`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`，且`otherParam`已移除。

  ![WKND要求轉換](../assets/how-to/aemrequest-log-transformation.png)

