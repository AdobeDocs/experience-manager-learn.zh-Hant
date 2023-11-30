---
title: 為標準AEM專案原型啟用前端管道
description: 瞭解如何為標準AEM專案啟用前端管道，以更快部署靜態資源，例如CSS、JavaScript、字型、圖示。 此外也會將前端開發與AEM上的完整棧疊後端開發分開。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 4%

---

# 為標準AEM專案原型啟用前端管道{#enable-front-end-pipeline-standard-aem-project}

瞭解如何啟用 [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd) (亦稱為標準AEM專案)建立方式 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署CSS、JavaScript、字型和圖示等前端資源，以加快開發到部署週期。 在AEM上將前端開發與完整棧疊後端開發分開。 您也會瞭解這些前端資源是如何 __非__ 從AEM存放庫提供，但從CDN提供，傳遞正規化的變更。


在Adobe Cloud Manager中建立了新的前端管道，該管道僅能建置和部署 `ui.frontend` 內建CDN的成品，並通知AEM其位置。 在網頁HTML產生期間的AEM上， `<link>` 和 `<script>` 標籤中，請參考成品在 `href` 屬性值。

不過，在WKND Sites AEM專案轉換後，前端開發人員可以與AEM上任何完整棧疊後端開發分開及並行運作，後者有自己的部署管道。

>[!IMPORTANT]
>
>一般而言，前端管道通常與 [AEM快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)，有相關教學課程 [AEM Sites快速入門 — 快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 以進一步瞭解。 因此，在本教學課程和相關影片中，您會看到參考資料，這是為了確保能找出細微的差異，並透過一些直接或間接的比較來說明重要概念。


相關 [多步驟教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 逐步說明如何使用「快速網站建立」功能，為虛擬生活風格品牌WKND實作AEM網站。 檢閱 [主題設定工作流程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 瞭解前端Pipeline運作也很有幫助。

## 前端管道的概述、優點和考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>這僅適用於AEMas a Cloud Service，不適用於AMS型AdobeCloud Manager部署。

## 先決條件

本教學課程中的部署步驟會在AdobeCloud Manager中進行，請確定您已 __部署管理員__ 角色，請參閱雲端管理 [角色定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

請務必使用 [沙箱計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 完成本教學課程時。

## 後續步驟 {#next-steps}

逐步教學課程會逐步解說 [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd) 轉換，以便為前端管道啟用它。

您還在等什麼？ 導覽至「 」以開始本教學課程 [檢閱完整棧疊專案](review-uifrontend-module.md) 在標準AEM Sites專案的情境下，章節並重述前端開發生命週期。
