---
title: 使用流量篩選器規則 (包括 WAF 規則) 保護網站
description: 了解流量篩選器規則，包括其子類別，即網頁應用程式防火牆 (WAF) 規則。如何建立、部署和測試規則。此外，分析結果以保護您的 AEM 網站。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# 使用流量篩選器規則 (包括 WAF 規則) 保護網站

了解&#x200B;**流量篩選器規則**，包括其在 AEM as a Cloud Service (AEMCS) 中的子類別，即&#x200B;**網頁應用程式防火牆 (WAF) 規則**。閱讀如何建立、部署和測試規則的內容。此外，分析結果以保護您的 AEM 網站。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概觀

降低安全性漏洞的風險是所有組織的首要任務。AEMCS 提供流量篩選器規則功能 (包括 WAF 規則)，用於保護網站和應用程式。

流量篩選器規則會部署至[內建 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)，並於要求到達 AEM 基礎結構之前進行評估。透過此功能，您可以顯著增強網站的安全性，確保只有合法要求才能存取 AEM 基礎結構。

本教學課程會引導您逐步完成建立、部署、測試流量篩選器規則 (包括 WAF 規則) 及分析其結果的流程。

您可以在[此文章](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant)中讀到關於流量篩選器規則的更多資訊。

>[!IMPORTANT]
>
> 流量篩選器規則的子類別 (稱為「WAF 規則」)，需要 WAF-DDoS 防護授權或增強型安全性授權。

我們邀請您寄送電子郵件至：**aemcs-waf-adopter@adobe.com**，提供意見回饋或詢問有關流量篩選器規則的問題。

## 下一步

了解[如何設定](./how-to-setup.md)此功能，讓您可以建立、部署和測試流量篩選器規則。閱讀關於如何設定 **Elasticsearch、Logstash 和 Kibana (ELK)** 堆疊儀表板工具以分析 AEMCS CDN 記錄結果的資訊。


