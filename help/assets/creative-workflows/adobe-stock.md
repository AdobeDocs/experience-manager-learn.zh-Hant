---
title: 將Adobe Stock資產與AEM Assets
description: 使用AEM戶能夠直接從中搜索、預覽、保存和許可Adobe Stock資AEM產。 企業現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保獲得許可的資產現在能夠廣泛用於其創意和營銷項目，並具備強大的資產管理AEM能力。
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 3%

---

# 將Adobe Stock與AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2使用戶能夠直接從中搜索、預覽、保存和許可Adobe Stock資AEM產。 企業現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保獲得許可的資產現在能夠廣泛用於其創意和營銷項目，並具備強大的資產管理AEM能力。

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>整合需要 [企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM6.4 ，並且至少部署了Service Pack 2。 有AEM關6.4 Service Pack的詳細資訊，請參閱 [發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)。

Adobe Stock和AEM Assets的整合使內容作者和營銷人員能夠輕鬆地許可和使用股票資產，用於創意或營銷目的。 您可以使用Omni Search執行股票資產搜索，方法是將位置篩選器添加為Adobe Stock，或者通過瀏覽AEM Assets主導航並按一下搜索Adobe Stock珊瑚UI表徵圖。

## 功能

### 搜索和保存

* 執行Adobe Stock資產搜索，而不離開AEM工作區。
* 保存Adobe Stock資產以供預覽，而不授予資產許可。
* 將Adobe Stock資產許可和保存到AEM Assets
* 能夠在AEM Assets用戶介面內從Adobe Stock搜索類似資產
* 在Adobe Stock網站上查看AEM Assets內股票搜索中的選定資產
* 授權資產檔案用藍色授權標籤標籤，以便輕鬆識別

### 資產中繼資料

* 授權資產儲存在AEM Assets。 資產屬性包含單獨的資產元資料頁籤下的庫存元資料
* 能夠將許可證引用添加到資產元資料

### 資產庫存配置檔案

* 用戶可以在下面選擇Adobe Stock配置檔案 *「用戶」>「我的首選項」>「庫存配置」*
* 可以將必備和可選引用添加到「資產許可」窗口。
* 能夠根據區域為「資產許可」窗口選擇語言首選項。

### 篩選

* 用戶可以根據「資產類型」、「方向」和「查看相似」篩選庫存資產
* 資產類型包括照片、插圖、向量、視頻、模板、3D、高級、編輯
* 方向包括水準、垂直和方形。
* 查看類似篩選器需要Adobe Stock檔案號

### 存取控制

* 管理員可向某些用戶/組提供權限，以在設定Adobe Stock雲服務配置時許可股票資產。
* 如果特定用戶/組沒有許可股票資產的權限， *庫存資產搜索/資產許可* 功能將被禁用。

## 建立Adobe Stock和AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2使用戶能夠直接從中搜索、預覽、保存和許可Adobe Stock資AEM產。 此視頻介紹如何使用Adobe控制台與AEM Assets建立Adobe I/O庫。

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>對於Adobe Stock雲服務配置，您必須選擇生產環境和許可資產路徑點到 `/content/dam`。 現在，中刪除了環境字AEM段。

>[!NOTE]
>
>整合需要 [企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 及AEM6.4，且 [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=。%2Fjcr%3內容%2Fmetadata%2Fdc%3Rasion&amp;2_group.propertyvalues.operation=等於&amp;2_group.propertyvalues.0_values=目標版本%3Aem%2F6-4&amp;3_group.propertyvalues.property=。%2fjcr%3內容%2fmetadata%2fdc%3軟體類型&amp;3_group.propertyvalues.operation=等於&amp;3_group.propertyvalues.0_values=軟體類型%3服務和累積修復&amp;orderby=%40jcr%3內容%2ftle元資料%2fdc%3Atiorder.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 已部署。 有AEM關6.4 Service Pack的詳細資訊，請參閱 [發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)。 您還需要管理員權限才能 [Adobe I/O控制台](https://console.adobe.io/)。 [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager組建一體化。

### 安裝 {#installations}

* 對AEM於6.4，需要安裝 [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=。%2Fjcr%3內容%2Fmetadata%2Fdc%3Rasion&amp;2_group.propertyvalues.operation=等於&amp;2_group.propertyvalues.0_values=目標版本%3Aem%2F6-4&amp;3_group.propertyvalues.property=。%2fjcr%3內容%2fmetadata%2fdc%3軟體類型&amp;3_group.propertyvalues.operation=等於&amp;3_group.propertyvalues.0_values=軟體類型%3服務和累積修復&amp;orderby=%40jcr%3內容%2ftle元資料%2fdc%3Atiorder.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 然後重新安裝cq-dam-stock-integration-content-1.0.4.zip檔案。
* 確保您對 [Adobe I/O控制台](https://console.adobe.io/)。 [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager組建一體化。

#### 使用Adobe I/O控制台設定Adobe IMS配置 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 在下建立Adobe IMS技術帳戶配置 **「工具」>「安全性」**
2. 選擇 *雲解決方案* 如 *Adobe Stock* 建立新證書或重新使用現有證書進行配置。
3. 導航到Adobe I/O控制台，然後為建立新的服務帳戶整合 *Adobe Stock*。
4. 將證書從步驟2上載到您的Adobe Stock服務帳戶整合。
5. 選擇所需的Adobe Stock配置檔案配置並完成服務整合。
6. 使用整合詳細資訊完成Adobe IMS技術帳戶配置
7. 確保可以使用Adobe IMS技術帳戶接收訪問令牌。

![Adobe IMS 技術帳戶](assets/screen_shot_2018-10-22at12219pm.png)

#### 設定Adobe StockCloud Services {#set-up-adobe-stock-cloud-services}

1. 為Adobe Stock建立新的雲服務配置 **工具>Cloud Services。**
2. 選擇 *Adobe IMS配置* 建立的 *Adobe Stock雲* 配置

3. 確保您選擇 **環境** 作為PROD。
4. **授權資產路徑** 可以指向位於 `/content/dam`。
5. 選擇區域設定並完成設定。
6. 您還可以將用戶/組添加到Adobe Stock雲服務，以啟用特定用戶或組的訪問。

![Adobe資產庫存配置](assets/screen_shot_2018-10-22at12425pm.png)

### 其他資源

* [企業庫存計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [《 AEM 6.4 Service Pack 2發行說明》](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html)
* [整合AEM與Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O控制台整合API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe StockAPI文檔](https://www.adobe.io/apis/creativecloud/stock/docs.html)
