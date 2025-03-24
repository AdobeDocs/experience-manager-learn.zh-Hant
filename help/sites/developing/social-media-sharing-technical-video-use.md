---
title: 在AEM Sites中使用社群媒體共用
description: 探索設定及使用社群媒體分享元件。
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# 使用社群媒體分享 {#using-social-media-sharing-in-aem-sites}

探索設定及使用社群媒體分享元件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

此影片使用[We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail)範例網站，探索下列社群媒體分享元件([AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hant)的一部分)的功能。

* 0:00 — 新增和設定社群媒體分享元件
* 1:00 — 分享到Facebook
* 3:10 — 共用至Pinterest
* 6:25 — 在產品頁面上使用社群媒體分享元件

## 外部化器設定 {#externalizer-setup}

![天CQ連結外部器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM的外部化程式](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html)應同時在AEM Author和AEM Publish上設定，以將發佈執行模式對應到用來存取AEM Publish的公開存取網域。

在此影片中，我們使用`/etc/hosts`來偽造&#x200B;*www.example.com*，以解析為localhost，並使用[基本AEM Dispatcher設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)，以允許www.example.com裝載AEM Publish。

## 支援材料 {#supporting-materials}

* [下載AEM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下載We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安裝 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
