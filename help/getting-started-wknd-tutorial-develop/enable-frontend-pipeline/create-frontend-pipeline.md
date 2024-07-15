---
title: 使用前端管道進行部署
description: 瞭解如何建立和執行可建置前端資源並部署到AEM as a Cloud Service中內建CDN的前端管道。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
duration: 225
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# 使用前端管道進行部署

在本章中，我們會在AdobeCloud Manager中建立和執行前端管道。 它只會從`ui.frontend`模組建置檔案，並將其部署至AEM as a Cloud Service中的內建CDN。 因此將遠離以`/etc.clientlibs`為基礎的前端資源傳遞。


## 目標 {#objectives}

* 建立和執行前端管道。
* 確認前端資源不是從`/etc.clientlibs`傳遞，而是從以`https://static-`開頭的新主機名稱傳遞

## 使用前端管道

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設[更新標準AEM專案](./update-project.md)中概述的步驟已完成。

確保您有[許可權可在Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions)中建立和部署管道，以及[對AEM as a Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html)的存取權。

## 重新命名現有管道

將現有管道從&#x200B;__部署到Dev__&#x200B;重新命名為&#x200B;__FullStack WKND部署到Dev__，方法是前往&#x200B;__設定__&#x200B;索引標籤的&#x200B;__非生產管道名稱__&#x200B;欄位。 這是為了明確表示管道是完整棧疊還是前端，只需檢視其名稱。

![重新命名管道](assets/fullstack-wknd-deploy-dev-pipeline.png)


同樣在&#x200B;__Source程式碼__&#x200B;索引標籤中，確定存放庫和Git分支欄位值正確，且分支有您前端管道合約變更。

![Source程式碼設定管道](assets/fullstack-wknd-source-code-config.png)


## 建立前端管道

若要&#x200B;__僅__&#x200B;從`ui.frontend`模組建置及部署前端資源，請執行下列步驟：

1. 在Cloud Manager UI中，從&#x200B;__管道__&#x200B;區段中，按一下&#x200B;__新增__&#x200B;按鈕，然後根據您想要部署到的AEM as a Cloud Service環境選取&#x200B;__新增非生產管道__ （或&#x200B;__新增生產管道__）。

1. 在&#x200B;__新增非生產管道__&#x200B;對話方塊中，作為&#x200B;__組態__&#x200B;步驟的一部分，選取&#x200B;__部署管道__&#x200B;選項，將其命名為&#x200B;__前端WKND部署至開發__，然後按一下&#x200B;__繼續__

![建立前端管道設定](assets/create-frontend-pipeline-configs.png)

1. 在&#x200B;__Source程式碼__&#x200B;步驟中，請選取&#x200B;__前端程式碼__&#x200B;選項，然後從&#x200B;__合格的部署環境__&#x200B;中挑選環境。 在&#x200B;__Source程式碼__區段中，確定存放庫和Git分支欄位值正確，且分支有您前端管道合約變更。
以及__最重要的__ __代碼位置__&#x200B;欄位的值為`/ui.frontend`，最後按一下&#x200B;__儲存__。

![建立前端管道Source程式碼](assets/create-frontend-pipeline-source-code.png)


## 部署順序

* 首先執行新重新命名的&#x200B;__FullStack WKND Deploy to Dev__&#x200B;管道，以從AEM存放庫中移除WKND clientlib檔案。 而且最重要的是透過新增&#x200B;__Sling設定__&#x200B;檔案(`SiteConfig`， `HtmlPageItemsConfig`)為前端管道合約準備AEM。

![未設定樣式的WKND網站](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>之後，__FullStack WKND Deploy to Dev__&#x200B;管道完成時，您將會有&#x200B;__未設定樣式的__ WKND網站，該網站可能會損毀。 請針對意外停機或部署進行規劃，這是您必須在從使用單一全棧疊管道切換到前端管道的初始切換期間規劃的一次性中斷。


* 最後，執行&#x200B;__FrontEnd WKND Deploy to Dev__&#x200B;管道以僅建置`ui.frontend`模組，並將前端資源直接部署至CDN。

>[!IMPORTANT]
>
>您注意到&#x200B;__未設定樣式的__ WKND網站已恢復正常，而此時&#x200B;__FrontEnd__&#x200B;管道執行速度比完整棧疊管道快很多。

## 驗證樣式變更和新傳遞正規化

* 開啟WKND網站的任何頁面，您會看到文字顏色為&#x200B;__Adobe紅色__，而前端資源(CSS、JS)檔案是從CDN傳遞。 資源要求主機名稱以`https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css`開頭，同樣以`HtmlPageItemsConfig`檔案中參考的site.js或任何其他靜態資源開頭。


![新樣式的WKND網站](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>這裡的`$HASH_VALUE$`與您在&#x200B;__FrontEnd WKND Deploy to Dev__&#x200B;管道的&#x200B;__內容雜湊__&#x200B;欄位中看到的相同。 AEM會收到前端資源的CDN URL通知，此值儲存在&#x200B;__prefixPath__&#x200B;屬性下的`/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content`。


![雜湊值相互關聯](assets/hash-value-correlartion.png)



## 恭喜！ {#congratulations}

恭喜，您已建立、執行及驗證只建置和部署WKND Sites專案的「ui.frontend」模組的前端管道。 現在，您的前端團隊可以在完整AEM專案生命週期之外，快速重複網站的設計和前端行為。

## 後續步驟 {#next-steps}

在下一章[考量事項](considerations.md)中，您將檢閱對前端和後端開發程式的影響。
