---
title: 使用流量篩選規則 (包括 WAF 規則) 保護網站
description: 瞭解流量篩選規則，包括其Web應用程式防火牆(WAF)規則的子類別。 如何建立、部署和測試規則。 此外，也分析結果，以保護您的AEM網站。
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
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 13%

---

# 使用流量篩選規則 (包括 WAF 規則) 保護網站

瞭解&#x200B;**流量篩選規則**，包括其AEM as a Cloud Service (AEMCS)中&#x200B;**Web應用程式防火牆(WAF)規則**&#x200B;的子類別。 閱讀如何建立、部署和測試規則。 此外，也分析結果，以保護您的AEM網站。

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 概觀

降低安全性違規風險是任何組織的首要任務。 AEMCS提供流量篩選規則功能(包括WAF規則)，以保護網站和應用程式。

流量篩選器規則會部署至[內建CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hant)，並在要求到達AEM基礎結構之前進行評估。 透過此功能，您可以顯著增強網站的安全性，確保只允許合法的請求存取AEM基礎結構。

本教學課程將引導您完成建立、部署、測試及分析流量篩選器規則(包括WAF規則)結果的程式。

您可以在[本文章](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hant)中閱讀更多有關流量篩選規則的資訊。

>[!IMPORTANT]
>
> 名為「WAF規則」的流量篩選規則子類別需要WAF-DdS保護或增強式安全性授權。

我們邀請您透過傳送電子郵件至：**aemcs-waf-adopter@adobe.com**，以提供意見回饋或詢問有關流量篩選規則的問題。

## 下一步

瞭解[如何設定](./how-to-setup.md)此功能，以便您可以建立、部署和測試流量篩選規則。 閱讀有關設定&#x200B;**Elasticsearch、Logstash和Kibana (ELK)**&#x200B;棧疊控制面板工具以分析AEMCS CDN記錄結果的相關資訊。


