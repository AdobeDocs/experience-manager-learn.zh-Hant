---
title: 整合Adobe Experience Platform和AEM中的標籤
description: Experience Platform Data Collection中的標籤是Adobe的新一代標籤管理解決方案，也是部署Adobe Analytics、Target、Audience Manager和其他解決方案的最佳方式。 取得Adobe Experience Platform中的標籤概觀，以及與Adobe Experience Manager整合的建議。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# 整合Experience Platform資料收集標籤和AEM {#overview}

瞭解如何將Adobe Experience Platform中的標籤與Adobe Experience Manager整合。

標籤是Adobe Experience Platform的下一代標籤管理技術。 標籤提供部署Adobe Analytics、Target、Audience Manager和更多解決方案的最簡單方式。 取得標籤的總覽，以及建議的Adobe Experience Manager整合。

>[!VIDEO](https://video.tv.adobe.com/v/3445211?quality=12&learn=on&captions=chi_hant)

## 先決條件

整合Experience Platform資料收集標籤時，需要下列專案。

+ AEM管理員對AEM as a Cloud Service環境的存取權
+ [WKND](https://github.com/adobe/aem-guides-wknd)之類的參考網站已部署至該網站。
+ 存取Adobe Experience Platform資料收集解決方案
+ 系統管理員對[Adobe Developer Console](https://developer.adobe.com/developer-console/)的存取權


## 高階步驟

+ 在Adobe Experience Platform Data Collection中，建立Tag屬性並編輯為&#x200B;_新增規則_。 然後&#x200B;_新增資料庫_，選取新新增的規則，核准並發佈。
+ 使用現有（或新的） IMS設定連線AEM和標籤
+ 在AEM中，建立Tags雲端服務設定，然後將其套用至現有網站，最後確認Tags屬性及其程式庫已載入發佈或作者網站。

## 後續步驟

[建立標籤屬性](create-tag-property.md)

## 其他資源 {#additional-resources}

+ [Experience Platform與Experience Cloud應用程式的整合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=zh-Hant)
+ [標籤總覽](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=zh-Hant)
+ [在具有標籤的網站中實作Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=zh-Hant)
