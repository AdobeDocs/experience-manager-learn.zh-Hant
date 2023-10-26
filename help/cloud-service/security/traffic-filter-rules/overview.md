---
title: 使用流量篩選規則（包括WAF規則）保護網站
description: 瞭解流量篩選規則，包括其Web應用程式防火牆(WAF)規則的子類別。 如何建立、部署和測試規則。 此外，也分析結果，以保護您的AEM網站。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---


# 使用流量篩選規則（包括WAF規則）保護網站

瞭解 **流量篩選器規則**，包括其子類別 **網頁應用程式防火牆(WAF)規則** 在AEMas a Cloud Service(AEMCS)中。 閱讀如何建立、部署和測試規則。 此外，也分析結果，以保護您的AEM網站。

## 概觀

降低安全性違規風險是任何組織的首要任務。 AEMCS提供流量篩選規則功能，包括WAF規則，以保護網站和應用程式。

流量篩選器規則會部署至 [內建CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 在請求到達AEM基礎結構之前會評估和。 透過此功能，您可以顯著增強網站的安全性，確保只允許合法的請求存取AEM基礎結構。

本教學課程將引導您完成建立、部署、測試及分析流量篩選器規則（包括WAF規則）結果的程式。

您可以在中閱讀更多有關流量篩選規則的資訊 [本文](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> 名為「WAF規則」的流量篩選規則子類別需要WAF-DDoS保護授權


## 下一步

瞭解 [如何設定](./how-to-setup.md) 功能，方便您建立、部署和測試流量篩選規則。 閱讀有關設定 **Elasticsearch、Logstash和Kibana (ELK)** 棧疊控制面板工具以分析AEMCS CDN記錄的結果。



