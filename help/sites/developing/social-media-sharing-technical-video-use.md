---
title: 在AEM Sites中使用社交媒體分享
description: 探索如何設定及使用社交媒體分享元件。
feature: 核心元件
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: 內容管理
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 4%

---


# 使用社交媒體分享 {#using-social-media-sharing-in-aem-sites}

探索如何設定及使用社交媒體分享元件。

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

此影片會使用[We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail)範例網站，探索社交媒體共用元件([AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)的一部分)的下列功能。

* 0:00 — 新增和設定社交媒體共用元件
* 1:00 — 共用至Facebook
* 3:10 — 共用至Pinterest
* 6:25 — 在產品頁面上使用社交媒體分享元件

## 外置程式設定 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 外部化應設定在AEM作者和AEM發佈上，將發佈執行模式對應至用於存取AEM Publish的公開存取網域。

在此影片中，我們使用`/etc/hosts`來假設&#x200B;*www.example.com*&#x200B;來解析為localhost，並使用[基本AEM Dispatcher設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)來允許www.example.com將AEM Publish置於最前。

## 支援材料 {#supporting-materials}

* [下載AEM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下載We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安裝 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
