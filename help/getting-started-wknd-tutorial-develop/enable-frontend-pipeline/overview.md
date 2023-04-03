---
title: 為標準AEM專案原型啟用前端管道
description: 了解如何為標準AEM專案啟用前端管道，以更快部署靜態資源，例如CSS、JavaScript、字型、圖示。 同時將前端開發與AEM上的全堆疊後端開發分開。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 1%

---


# 為標準AEM專案原型啟用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何啟用 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd) (亦稱為標準AEM專案)使用 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 使用前端管道來部署前端資源，例如CSS、JavaScript、字型和圖示，以縮短開發至部署的週期。 在AEM上將前端開發與全堆疊後端開發分離。 您也可以了解這些前端資源 __not__ 由AEM存放庫提供，但由CDN提供，這改變了傳遞范式。


新的前端管道會在Analytics Cloud Manager中建立，僅供建置和部署Adobe `ui.frontend` 對應至內建的CDN，並通知AEM其位置。 在AEM上，在網頁的HTML產生期間， `<link>` 和 `<script>` 標籤，請參閱 `href` 屬性值。

不過，在WKND Sites AEM專案轉換後，前端開發人員可與AEM上任何具有自己部署管道的完整堆疊後端開發分開及平行運作。

>[!IMPORTANT]
>
>一般而言，前端管道通常與 [AEM快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)，則提供相關教學課程 [AEM Sites快速入門 — 快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 以進一步了解。 因此，在本教學課程和相關影片中，您會看到這些參照，這是為了確保能找出細微的差異，並有一些直接或間接的比較來解釋重要的概念。


相關 [多步驟教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 逐步使用「快速建立網站」功能，針對虛構的生活風格品牌實作AEM網站，以了解WKND。 檢閱 [主題工作流程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 了解前端管道運作也很有幫助。

## 前端管道的概述、優點和考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>這僅適用於AEMas a Cloud Service，而不適用於AMS型AdobeCloud Manager部署。

## 必備條件

本教學課程中的部署步驟在Analytics Cloud Manager中進行，請確定您有 __部署管理員__ 角色，請參閱雲端管理 [角色定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

請務必使用 [沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 完成本教學課程時。

## 後續步驟 {#next-steps}

逐步教學課程將逐步說明 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd) 轉換，以便用於前端管道。

你在等什麼？ 導覽至 [檢閱完整堆疊專案](review-uifrontend-module.md) 章節，並回顧標準AEM Sites項目的前端開發生命週期。

