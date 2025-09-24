---
title: 啟用標準 AEM 專案原型的前端管道
description: 了解如何啟用標準 AEM 專案的前端管道，以便更快部署 CSS、JavaScript、字體、圖示等靜態資源。此外，也將 AEM 上的前端開發與全堆疊後端開發分開
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
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# 啟用標準 AEM 專案原型的前端管道{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

了解如何啟用使用 [AEM 專案原型](https://github.com/adobe/aem-project-archetype)建立的 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd) (即標準 AEM 專案)，使其能透過前端管道來部署前端資源 (例如 CSS、JavaScript、字體和圖示)，以利加速開發至部署的週期。將 AEM 上的前端開發與全堆疊後端開發分開。您也會了解這些前端資源並&#x200B;__不是__&#x200B;從 AEM 存放庫而是從內容傳遞網路提供使用，這是傳遞典範的改變。


在 Adobe Cloud Manager 中建立一個新的前端管道，該管道只會建置和部署 `ui.frontend` 成品到內建內容傳遞網路，並將其位置告知 AEM。在 AEM 產生網頁 HTML 期間，`<link>` 和 `<script>` 標記會在其 `href` 屬性值中參照到這個成品的位置。

然而，在 WKND Sites AEM 專案轉換之後，前端開發人員可以獨立於 AEM 上任何全堆疊後端開發而進行平行作業。而全堆疊後端開發擁有自己的部署管道。

>[!IMPORTANT]
>
>一般來說，前端管道通常與 [AEM Quick Site Creation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=zh-hant) 搭配使用，要了解更多相關資訊，可參閱相關教學課程 [AEM Sites 快速入門 - Quick Site Creation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hant)。因此，在本教學課程和相關影片中，您會看到其相關參照，這是為了確保突顯出細微的差別，並透過一些直接或間接的比較來解釋關鍵概念。


有個相關的[多步驟教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hant)，會逐步引導如何使用 Quick Site Creation 功能為虛構的生活風格品牌 WKND 實施 AEM 網站。檢閱[主題工作流程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=zh-Hant)來了解前端管道的運作方式也很有幫助。

## 前端管道的概觀、優點和考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>這僅適用於 AEM as a Cloud Service，而不適用於 AMS 型 Adobe Cloud Manager 部署。

## 先決條件

本教學課程中的部署步驟在 Adobe Cloud Manager 中進行，須確認您擁有&#x200B;__部署管理員__&#x200B;角色，請參閱 Cloud Manager 的[角色定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=zh-hant#role-definitions)。

在完成本教學課程時，請務必使用[沙箱程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=zh-Hant)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=zh-Hant)。

## 後續步驟 {#next-steps}

循序漸進的教學課程將逐步引導您完成 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)轉換，使其能使用前端管道。

您還在等什麼？要開始教學課程請導覽至「[檢閱全堆疊專案](review-uifrontend-module.md)」章節，然後在標準 AEM Sites 專案的背景下，回顧前端開發生命週期。
