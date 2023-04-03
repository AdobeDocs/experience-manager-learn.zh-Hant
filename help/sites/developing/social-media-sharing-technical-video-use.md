---
title: 在AEM Sites中使用社交媒體分享
description: 探索如何設定及使用社交媒體分享元件。
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 3%

---

# 使用社交媒體分享 {#using-social-media-sharing-in-aem-sites}

探索如何設定及使用社交媒體分享元件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

此影片會探索社交媒體分享元件( [AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html))使用 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 網站範例。

* 0:00 — 新增和設定社交媒體分享元件
* 1:00 — 共用至Facebook
* 3:10 — 共用至Pinterest
* 6:25 — 在產品頁面上使用社交媒體分享元件

## 外置程式設定 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizer](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 應在「AEM作者」和「AEM發佈」上設定，將發佈執行模式對應至用來存取AEM Publish的公開存取網域。

在此影片中，我們使用 `/etc/hosts` 到Spo *www.example.com* 要解析為localhost，請使用 [基本AEM Dispatcher設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 允許www.example.com搶先發佈AEM。

## 支援材料 {#supporting-materials}

* [下載AEM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下載We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安裝 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
