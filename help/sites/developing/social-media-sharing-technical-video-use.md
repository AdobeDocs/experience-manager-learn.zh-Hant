---
title: 在AEM Sites中使用社群媒體共用
description: 探索設定及使用社群媒體分享元件。
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 3%

---

# 使用社群媒體分享 {#using-social-media-sharing-in-aem-sites}

探索設定及使用社群媒體分享元件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

本影片探討以下社群媒體分享元件的功能（的一部分） [AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html))使用 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 範例網站。

* 0:00 — 新增和設定社群媒體分享元件
* 1:00 — 共用至Facebook
* 3:10 — 共用至Pinterest
* 6:25 — 在產品頁面上使用社群媒體分享元件

## 外部化器設定 {#externalizer-setup}

![Day CQ連結外部化器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM外部化器](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 應同時在AEM Author和AEM Publish上設定，以將發佈執行模式對應到用來存取AEM Publish的公開存取網域。

在這段影片中，我們使用 `/etc/hosts` 到欺騙 *www.example.com* 解析為localhost，並使用 [基本AEM Dispatcher設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 允許www.example.com做為AEM Publish的前端。

## 支援材料 {#supporting-materials}

* [下載AEM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下載We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安裝 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
