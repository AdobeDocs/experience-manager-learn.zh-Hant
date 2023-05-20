---
title: 利用社交媒體共用AEM Sites
description: 瀏覽設定和使用社交媒體共用元件。
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

# 使用社交媒體共用 {#using-social-media-sharing-in-aem-sites}

瀏覽設定和使用社交媒體共用元件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

此視頻探討了社交媒體共用元件的以下功能( [核AEM心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html))使用 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 示例網站。

* 0:00 — 添加和配置社交媒體共用元件
* 1:00 — 共用到Facebook
* 3:10 — 與Pinterest共用
* 6:25 — 在產品頁面上使用社交媒體共用元件

## 外部化程式設定 {#externalizer-setup}

![第CQ天連結外部化程式](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[外AEM部化](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 應在AEM Author和AEM Publish上設定，以將發佈運行模式映射到用於訪問AEM Publish的可公開訪問的域。

在此視頻中，我們使用 `/etc/hosts` 到 *www.example.com* 要解析為localhost，請使用 [基本AEMDispatcher配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 允許www.example.com在AEM發佈前顯示。

## 支撐材料 {#supporting-materials}

* [下載核AEM心元件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下載We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安裝 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
