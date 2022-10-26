---
title: 使用前端管道進行部署
description: 了解如何建立並執行前端管道，以建置前端資源並部署至AEMas a Cloud Service的內建CDN。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# 使用前端管道進行部署

在本章中，我們會在Analytics Cloud Manager中建立並執行前端管道Adobe。 它只會從 `ui.frontend` 模組，並部署至AEM as a Cloud Service的內建CDN。 因此從  `/etc.clientlibs` 以前端資源傳送為基礎。


## 目標 {#objectives}

* 建立並運行前端管線。
* 驗證前端資源未從 `/etc.clientlibs` 但從新主機名開始 `https://static-`

## 使用前端管線

>[!VIDEO](https://video.tv.adobe.com/v/3409420/)

## 必備條件 {#prerequisites}

此為多部分教學課程，假設要執行 [更新標準AEM專案](./update-project.md) 已完成。

確保您 [在Cloud Manager中建立和部署管道的權限](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) 和 [存取AEMas a Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 更名現有管線

更名現有管道 __部署至開發人員__ to  __將FullStack WKND部署到開發__ 去 __設定__ 標籤 __非生產管道名稱__ 欄位。 這是為了透過查看管道名稱，明確管道是完整堆疊還是前端。

![更名管道](assets/fullstack-wknd-deploy-dev-pipeline.png)


也會在 __原始碼__ 索引標籤，確定「存放庫」和「Git分支」欄位值正確，且分支的前端管道合約已變更。

![原始碼配置管道](assets/fullstack-wknd-source-code-config.png)


## 建立前端管線

結束日期 __僅__ 從 `ui.frontend` 模組，請執行以下步驟：

1. 在Cloud Manager UI中，從 __管道__ ，按一下 __新增__ 按鈕，然後選取 __新增非生產管道__ (或 __新增生產管道__)，根據您要部署的AEMas a Cloud Service環境。

1. 在 __新增非生產管道__ 對話，作為 __設定__ 步驟，選擇 __部署管道__ 選項，命名為 __FrontEnd WKND部署到Dev__，然後按一下 __繼續__

![建立前端管線配置](assets/create-frontend-pipeline-configs.png)

1. 作為 __原始碼__ 步驟，選擇 __前端代碼__ ，並從 __合格的部署環境__. 在 __原始碼__ 小節確保「存放庫」和「Git分支」欄位值正確，且分支的前端管道合約已變更。
和 __最重要__ 針對 __代碼位置__ 欄位值為 `/ui.frontend` 最後，按一下 __儲存__.

![建立前端管道原始碼](assets/create-frontend-pipeline-source-code.png)


## 部署順序

* 先執行新重新命名的 __將FullStack WKND部署到開發__ 管道，從AEM存放庫移除WKND clientlib檔案。 最重要的是，要為前端管道合約做好準備，請在 __Sling設定__ 檔案(`SiteConfig`, `HtmlPageItemsConfig`)。

![未設定樣式的WKND站點](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>之後， __將FullStack WKND部署到開發__ 管道完成，您將擁有 __未設定樣式__ WKND Site，它可能看起來損壞。 請計畫中斷或在奇數時段部署，這是在初始切換期間必須計畫的一次性中斷，從使用單個全堆棧管道到前端管道。


* 最後，執行 __FrontEnd WKND部署到Dev__ 僅建置管線 `ui.frontend` 模組，並直接將前端資源部署至CDN。

>[!IMPORTANT]
>
>您會發現 __未設定樣式__ WKND站點恢復正常，這次 __前端__ 管道執行速度遠快於全堆棧管道。

## 驗證樣式變更和新的傳遞范式

* 開啟WKND網站的任何頁面，您就會看到文字顏色 __Adobe紅色__ 和前端資源(CSS、JS)檔案是從CDN傳送。 資源請求主機名的開頭為 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 和site.js或您在 `HtmlPageItemsConfig` 檔案。


![新樣式的WKND站點](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>此 `$HASH_VALUE$` 這和您在 __FrontEnd WKND部署到Dev__  管道 __內容雜湊__ 欄位。 AEM會收到前端資源的CDN URL通知，而值會儲存在 `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` 在 __prefixPath__ 屬性。


![雜湊值關聯](assets/hash-value-correlartion.png)



## 恭喜！ {#congratulations}

恭喜您已建立、執行及驗證前端管道，該管道僅會建置並部署WKND Sites專案的「ui.frontend」模組。 現在，您的前端團隊可以在完整AEM專案生命週期之外，快速反覆了解網站的設計和前端行為。

## 後續步驟 {#next-steps}

在下一章中， [考量事項](considerations.md)，您將檢閱對前端和後端開發流程的影響。
