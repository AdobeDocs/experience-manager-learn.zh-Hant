---
title: 搭配使用Adobe Stock資產與AEM資產
description: 'AEM讓使用者能夠直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 企業組織現在可將其Adobe Stock Enterprise計畫與AEM Assets整合，以確保授權資產現在可廣泛用於其創意和行銷專案，以及AEM的強大資產管理功能。 '
feature: creative-cloud-integration
topics: authoring, collaboration, operations, sharing, metadata, images, stock
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 3%

---


# 搭配使用Adobe Stock和AEM資產{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2讓使用者能夠直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 企業組織現在可將其Adobe Stock Enterprise計畫與AEM Assets整合，以確保授權資產現在可廣泛用於其創意和行銷專案，以及AEM的強大資產管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>整合需要企 [業版Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4，且至少必須部署Service Pack 2。 如需AEM 6.4 Service Pack詳細資訊，請參閱這些版 [本注意事項](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)。

Adobe Stock和AEM Assets整合可讓內容作者和行銷人員輕鬆授權及使用Stock資產，以進行創意或行銷。 您可以使用Omni Search、將位置篩選新增為Adobe Stock或在AEM Assets主導覽中導覽，然後按一下「搜尋Adobe Stock Coral UI Icon」，來執行Stock資產搜尋。

## 功能

### 搜尋與儲存

* 執行Adobe Stock資產搜尋，而不需離開AEM工作區。
* 儲存Adobe Stock資產以進行預覽，毋需取得資產授權。
* 可授權Adobe Stock資產並儲存至AEM Assets
* 能夠在AEM Assets UI中從Adobe Stock搜尋類似資產
* 在Adobe Stock網站的AEM Assets中，從Stock Search檢視選取的資產
* 授權資產檔案上標有藍色授權徽章，以方便識別

### 資產中繼資料

* 授權資產會儲存在AEM Assets中。 資產屬性在個別的資產中繼資料標籤下包含Stock中繼資料
* 能夠將授權參考新增至資產中繼資料

### 資產庫存設定檔

* 使用者可以在「使用者>我的偏好設定> Stock設 *定」下選取「Adobe Stock設定檔」*
* 可將強制和選擇性參考新增至「資產授權」視窗。
* 可根據地區選擇資產授權視窗的語言偏好設定。

### 篩選

* 使用者可以根據「資產類型」、「方向」和「檢視類似」來篩選庫存資產
* 資產類型包括像片、插圖、向量、視訊、範本、3D、優質、編輯
* 方向包括水準、垂直和正方形。
* 檢視類似篩選需要Adobe Stock檔案編號

### 存取控制

* 管理員可在設定Adobe Stock Cloud服務設定時，為特定使用者／群組提供授權Stock資產的權限。
* 如果特定使用者／群組沒有授權Stock資產的權限， *Stock Asset Search / Asset授權功能將會停用* 。

## 使用AEM資產設定Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2讓使用者能夠直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 本影片介紹如何使用Adobe I/O Console使用AEM Assets設定Adobe Stock的快速解說。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>對於Adobe Stock Cloud服務配置，必須選擇指向/content/dam的「PROD環境」(PROD Environment)和「授權」(Licensed)資產路徑。 「環境」欄位將會在下一個AEM版本中移除，而授權的資產路徑是即將推出的功能的一部分，而下一個AEM版本將會推出此欄位的支援。

>[!NOTE]
>
>整合需要企 [業版Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4，且至少必須部 [署Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) 。 如需AEM 6.4 Service Pack詳細資訊，請參閱這些版 [本注意事項](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)。 您也需要 [Adobe I/O Console](https://console.adobe.io/)、 [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager的管理員權限才能設定整合。

### 安裝 {#installations}

* 對於AEM 6.4，您必須安裝 [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) ，然後重新安裝cq-dam-stock-integration-content-1.0.4.zip檔案。
* 請確定您擁有 [Adobe I/O Console](https://console.adobe.io/)、 [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager的管理員權限，以設定整合。

#### 使用Adobe I/O Console設定Adobe IMS設定 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 在「工具>安全性」下建立Adobe IMS技術 **帳戶設定**
2. 選取 *Cloud Solution* （雲端解決方案） *為Adobe Stock* ，並建立新憑證或重新使用現有憑證進行設定。
3. 導覽至Adobe I/O Console並建立Adobe Stock的新服務帳戶 *整合*。
4. 將憑證從步驟2上傳到您的Adobe Stock Service帳戶整合。
5. 選擇所需的Adobe Stock設定檔設定，並完成服務整合。
6. 使用整合詳細資訊以完成Adobe IMS技術帳戶設定
7. 請確定您可以使用Adobe IMS技術帳戶接收存取Token。

![Adobe IMS 技術帳戶](assets/screen_shot_2018-10-22at12219pm.png)

#### 設定Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. 在「工具>雲端服務」下，建立Adobe Stock的 **新雲端服務設定。**
2. 選取在 *上節中為您的* Adobe Stock Cloud設定建立的Adobe IMS設定 **

3. 確保選擇「環 **境** 」(ENVIRONMENT)作為PROD。 測試環境不受支援，將在下一版AEM中移除。
4. **授權資產路徑** ，可指向/content/dam下的任何目錄。 AEM的下一版將新增此欄位的功能支援
5. 選擇您的地區並完成設定。
6. 您也可以將使用者／群組新增至Adobe Stock Cloud服務，以啟用特定使用者或群組的存取權。

![Adobe Assets Stock設定](assets/screen_shot_2018-10-22at12425pm.png)

### 其他資源

* [Enterprise Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)
* [整合AEM和Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [Adobe I/O Console整合API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API檔案](https://www.adobe.io/apis/creativecloud/stock/docs.html)