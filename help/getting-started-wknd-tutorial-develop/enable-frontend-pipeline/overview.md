---
title: 為標準項目原型啟用前AEM端管線
description: 瞭解如何為標準項目啟用前端管線AEM，以加快靜態資源（如CSS、JavaScript、字型、表徵圖）的部署。 同時將前端開發與全堆棧後端開發分離AEM。
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
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 1%

---

# 為標準項目原型啟用前AEM端管線{#enable-front-end-pipeline-standard-aem-project}

瞭解如何啟用 [WKNDAEM站點項目](https://github.com/adobe/aem-guides-wknd) (又稱標AEM準項目) [項AEM目原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署前端資源（如CSS、JavaScript、字型和表徵圖），以加快開發到部署的週期。 將前端開發與全堆棧後端開發分離AEM。 您還可以瞭解這些前端資源 __不__ 從儲存庫提AEM供，但從CDN提供，這改變了交付模式。


在Adobe雲管理器中建立新的前端管道，該管道僅構建和部署 `ui.frontend` 內置CDN的對象，並通知其AEM位置。 在網AEM頁的HTML生成過程中， `<link>` 和 `<script>` 標籤，請參閱中的此對象位置 `href` 屬性值。

但是，在WKND站點項AEM目轉換後，前端開發人員可以與具有自己部署管道的任何全堆棧後端開發分開工作，並與之並行AEM。

>[!IMPORTANT]
>
>通常，前端管線與 [快AEM速建立站點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)，有相關教程 [AEM Sites入門 — 快速站點建立](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 瞭解更多。 因此，在本教程和相關視頻中，您遇到了對它的引用，這是為了確保能夠消除細微的差別，並且有一些直接或間接的比較來解釋關鍵概念。


相關 [多步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 使用「快速站AEM點建立」功能，在WKND中為虛擬生活方式品牌實施一個站點。 審閱 [主題工作流](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 瞭解前端管線工作方式也很有幫助。

## 前端管道的概述、優點和注意事項

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>這僅適用於AEMas a Cloud Service，而不適用於基於AMS的Adobe雲管理器部署。

## 必備條件

本教程中的部署步驟在Adobe雲管理器中進行，確保 __部署管理器__ 角色，請參閱雲管理 [角色定義](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions)。

確保使用 [沙盒程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 完成本教程時。

## 後續步驟 {#next-steps}

逐步教程將介紹 [WKNDAEM站點項目](https://github.com/adobe/aem-guides-wknd) 轉換，以啟用前端管線。

你在等什麼？ 導航至 [查看完整堆棧項目](review-uifrontend-module.md) 並結合標準AEM Sites項目回顧了前端開發生命週期。
