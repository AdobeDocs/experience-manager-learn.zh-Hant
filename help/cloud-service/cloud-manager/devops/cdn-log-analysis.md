---
title: CDN記錄分析工具
description: 瞭解Adobe提供的AEM Cloud Service CDN記錄分析工具，以及它如何協助您深入瞭解您的CDN效能和AEM實作。
version: Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 8051f262f978cdf5aff48cb27e5408a7ee3c0b9d
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# CDN記錄分析工具

瞭解 _AEM Cloud Service CDN記錄分析工具_ 該Adobe提供的資訊，以及可如何協助您深入瞭解您的CDN效能和AEM實作。
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## 概觀

此 [AEMas a Cloud ServiceCDN記錄分析工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) 提供預先建立的控制面板，您可與整合 [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) 或 [麋鹿棧疊](https://www.elastic.co/elastic-stack) 以即時監控及分析您的CDN記錄。

使用此工具，您可以實現即時監控和主動問題偵測。 因此，可確保最佳化的內容傳遞，以及針對拒絕服務(DoS)和分散式拒絕服務(DDoS)攻擊的適當安全性措施。

## 主要功能

- 簡化的記錄分析
- 即時監視
- 緊密整合
- 儀表板
   - 識別潛在的安全性威脅
   - 更快速的使用者體驗

## 控制面板概觀

為了快速啟動記錄分析，Adobe為Splunk和ELK棧疊提供預先建立的儀表板。

- **CDN快取命中率**：提供按HIT、PASS和MISS狀態區分的快取命中率總計和請求總計數的深入分析。 它也會提供熱門點選、通過和錯過URL。

  ![CDN快取命中率](assets/CHR-dashboard.png)

- **CDN流量儀表板**：透過CDN和來源請求率、4xx和5xx錯誤率以及非快取請求提供流量的深入分析。 此外，每個使用者端IP位址每秒的最大CND和來源要求數，以及最佳化CDN設定的更多深入分析。

  ![CDN流量儀表板](assets/Traffic-dashboard.png)

- **WAF控制面板**：透過已分析、已標幟和已封鎖的請求提供深入分析。 此外，它還提供WAF旗標ID的熱門攻擊、使用者端IP、國家/地區和使用者代理的前100名攻擊者，以及最佳化WAF設定的更多深入分析。

  ![WAF控制面板](assets/WAF-Dashboard.png)

## Splunk整合

針對組織運用 [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) 已啟用AEMCS記錄轉送至其Splunk執行個體的使用者，可以快速匯入預先建立的儀表板。 此設定有助於加速記錄分析，提供可操作的深入分析，以最佳化AEM實作並減少DOS攻擊等安全性威脅。

您可以開始使用 [適用於AEMCS CDN記錄分析的Splunk控制面板](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/READEME.md#splunk-dashboards-for-aemcs-cdn-log-analysis) 指南。


## ELK整合

此 [麋鹿棧疊](https://www.elastic.co/elastic-stack)包含Elasticsearch、Logstash和Kibana的變數是另一個強大的記錄分析選項。 對於無法存取Splunk設定或記錄轉送功能的組織，此功能非常有用。 在本機設定ELK棧疊很簡單，工具提供Docker Compose檔案以快速開始。 接著，您可以匯入預先建立的控制面板，並擷取使用AdobeCloud Manager下載的CDN記錄。

您可以開始使用 [AEMCS CDN記錄分析的ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis) 指南。
