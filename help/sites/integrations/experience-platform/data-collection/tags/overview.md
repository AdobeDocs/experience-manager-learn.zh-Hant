---
title: 整合 Adobe Experience Platform 和 AEM 中的標記
description: Experience Platform Data Collection 中的標記是 Adobe 新一代標記管理解決方案，也是部署 Adobe Analytics、Target、Audience Manager 和許多其他解決方案的最佳方式。了解 Adobe Experience Platform 的標記，以及建議與 Adobe Experience Manager 整合的概要介紹。
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
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# 整合 Experience Platform Data Collection 標記和 AEM {#overview}

了解如何整合 Adobe Experience Platform 標記和 Adobe Experience Manager。

標記是 Adobe Experience Platform 的新一代標記管理技術。標記是部署 Adobe Analytics、Target、Audience Manager 和許多其他解決方案的最簡單方式。了解標記以及建議與 Adobe Experience Manager 整合的概要介紹。

>[!VIDEO](https://video.tv.adobe.com/v/3445211?quality=12&learn=on&captions=chi_hant)

## 先決條件

整合 Experience Platform Data Collection 標記時，以下為必要條件。

+ AEM as a Cloud Service 環境的 AEM 管理員存取權
+ 已經部署如 [WKND](https://github.com/adobe/aem-guides-wknd) 的參照網站。
+ Adobe Experience Platform Data Collection 解決方案的存取權
+ [Adobe Developer Console](https://developer.adobe.com/developer-console/) 的系統管理員存取權


## 概括性步驟

+ 在 Adobe Experience Platform Data Collection 中，建立一個標記屬性並進行編輯以「_新增規則_」。接著「_新增資料庫_」，選取新加入的規則，核准該規則然後發佈。
+ 使用現有 (或新的) IMS 設定連結 AEM 和標記
+ 在 AEM 中，建立標記雲端服務設定，然後將其套用至現有網站，最後檢查並確認標記屬性及其資料庫是否已載入到已發佈或製作的網站上。

## 後續步驟

[建立標記屬性](create-tag-property.md)

## 其他資源 {#additional-resources}

+ [Experience Platform 與 Experience Cloud 應用程式的整合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=zh-Hant)
+ [標記概觀](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=zh-Hant)
+ [使用標記在網站中實施 Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=zh-Hant)
