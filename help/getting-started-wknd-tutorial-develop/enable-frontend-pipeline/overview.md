---
title: 為標準AEM專案原型啟用前端管道
description: 瞭解如何為標準AEM專案啟用前端管道，以更快部署靜態資源，例如CSS、JavaScript、字型、圖示。 此外也會將前端開發與AEM上的完整棧疊後端開發分開。
version: Experience Manager as a Cloud Service
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
duration: 206
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# 為標準AEM專案原型啟用前端管道{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

瞭解如何啟用使用[AEM專案原型](https://github.com/adobe/aem-project-archetype)建立的[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd) (亦稱為標準AEM專案)，以使用前端管道部署前端資源(例如CSS、JavaScript、字型和圖示)來加快開發到部署的週期。 在AEM上將前端開發與完整棧疊後端開發分開。 您也會瞭解這些前端資源是如何&#x200B;__不從AEM存放庫提供__，而是從CDN （傳遞正規化的變更）提供。


在Adobe Cloud Manager中建立新的前端管道，只將`ui.frontend`成品建置和部署至內建CDN並通知AEM其位置。 在網頁產生HTML期間，在AEM上，`<link>`和`<script>`標籤在`href`屬性值中參照此成品位置。

不過，在WKND Sites AEM專案轉換後，前端開發人員可以與AEM上的任何全棧疊後端開發分開及並行運作，後者有自己的部署管道。

>[!IMPORTANT]
>
>一般而言，前端管道通常會與[AEM快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)搭配使用，這裡有相關的教學課程[AEM Sites快速入門 — 快速網站建立](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html)以瞭解更多相關資訊。 因此，在本教學課程和相關影片中，您會看到參考資料，這是為了確保能找出細微的差異，並透過一些直接或間接的比較來說明重要概念。


相關的[多步驟教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html)會使用「快速網站建立」功能，逐步解說如何為虛擬生活風格品牌WKND實作AEM網站。 檢閱[主題工作流程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)以瞭解前端管道運作也很有幫助。

## 前端管道的概述、優點和考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>這僅適用於AEM as a Cloud Service ，不適用於以AMS為基礎的Adobe Cloud Manager部署。

## 先決條件

本教學課程中的部署步驟會在Adobe Cloud Manager中進行，請確認您有&#x200B;__部署管理員__&#x200B;角色，請參閱雲端管理[角色定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions)。

完成本教學課程時，請務必使用[沙箱程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html)。

## 後續步驟 {#next-steps}

逐步教學課程會逐步解說[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)的轉換，以為前端管道啟用。

您還在等什麼？ 導覽至[檢閱完整棧疊專案](review-uifrontend-module.md)章節，並回顧標準AEM Sites專案內容中的前端開發生命週期，以開始本教學課程。
