---
title: 搭配Adobe Stock資產使用AEM Assets
description: 'AEM可讓使用者直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 組織現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保授權資產現在可廣泛供其創意和行銷專案使用，同時具備AEM強大的資產管理功能。 '
feature: Adobe Stock
version: 6.4, 6.5
topic: 內容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 3%

---


# 將Adobe Stock與AEM Assets搭配使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2可讓使用者直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 組織現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保授權資產現在可廣泛供其創意和行銷專案使用，同時具備AEM強大的資產管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>此整合需要[企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和AEM 6.4，且至少部署了Service Pack 2。 如需AEM 6.4 Service Pack詳細資訊，請參閱以下[發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)。

Adobe Stock和AEM Assets整合可讓內容作者和行銷人員輕鬆授權及使用stock資產，以用於創意或行銷用途。 您可以使用Omni Search、將位置篩選條件新增為Adobe Stock，或瀏覽AEM Assets主導覽列，然後按一下「搜尋Adobe Stock Coral UI圖示」 ，以執行Stock資產搜尋。

## 功能

### 搜尋並儲存

* 執行Adobe Stock資產搜尋，而不需離開AEM工作區。
* 儲存Adobe Stock資產以供預覽，不需授權資產。
* 能將Adobe Stock資產授權並儲存至AEM Assets
* 可在AEM Assets UI中從Adobe Stock搜尋類似資產
* 在Adobe Stock網站的AEM Assets內，從Stock Search檢視選取的資產
* 授權資產檔案會標示藍色授權徽章，以方便識別

### 資產中繼資料

* 授權資產會儲存在AEM Assets中。 資產屬性會在個別的資產中繼資料標籤下包含庫存中繼資料
* 能將授權參考新增至資產中繼資料

### 資產庫存設定檔

* 使用者可以在&#x200B;*使用者>我的偏好設定>庫存設定*&#x200B;下選取Adobe Stock設定檔
* 可以將強制和選用參考新增至「資產授權」視窗。
* 根據地區為「資產授權」窗口選擇語言首選項的功能。

### 篩選

* 使用者可以根據資產類型、方向和檢視類似項目來篩選庫存資產
* 資產類型包括照片、插圖、向量、視頻、模板、3D、高級、編輯
* 方向包括水準、垂直和方形。
* 檢視類似篩選器需要Adobe Stock檔案號

### 存取控制

* 設定Adobe Stock雲端服務設定時，管理員可向特定使用者/群組提供授權Stock資產的權限。
* 如果特定使用者/群組沒有授權Stock資產的權限，*Stock Asset Search / Asset授權*&#x200B;功能將會停用。

## 使用AEM Assets設定Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2可讓使用者直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 本影片說明如何使用Adobe主控台，透過AEM Assets設定Adobe I/O庫存。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>對於Adobe Stock雲端服務設定，您必須選取指向/content/dam的PROD環境和授權資產路徑。 環境欄位將在下一個AEM版本中移除，且授權資產路徑是即將推出的功能的一部分，並將在下一個AEM版本中推出對此欄位的支援。

>[!NOTE]
>
>整合需要[企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和至少部署[Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0)的AEM 6.4。 如需AEM 6.4 Service Pack詳細資訊，請參閱以下[發行說明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)。 您也需要[Adobe I/O主控台](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager的管理員權限，才能設定整合。

### 安裝 {#installations}

* 若是AEM 6.4，您需要安裝[AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0)，然後重新安裝cq-dam-stock-integration-content-1.0.4.zip檔案。
* 請確定您擁有[Adobe I/O主控台](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager的管理員權限，以設定整合。

#### 使用Adobe控制台{#set-up-adobe-ims-configuration-using-adobe-i-o-console}設定Adobe I/OIMS設定

1. 在&#x200B;**工具>安全性**&#x200B;下建立AdobeIMS技術帳戶設定
2. 選取&#x200B;*雲端解決方案*&#x200B;作為&#x200B;*Adobe Stock*，然後建立新憑證或重新使用現有憑證進行設定。
3. 導覽至「Adobe I/O主控台」，並為&#x200B;*Adobe Stock*&#x200B;建立新的服務帳戶整合。
4. 從步驟2上傳憑證至您的Adobe Stock服務帳戶整合。
5. 選擇所需的Adobe Stock設定檔設定並完成服務整合。
6. 使用整合詳細資訊來完成AdobeIMS技術帳戶設定
7. 請確定您可以使用AdobeIMS技術帳戶接收存取權杖。

![Adobe IMS 技術帳戶](assets/screen_shot_2018-10-22at12219pm.png)

#### 設定Adobe StockCloud Services{#set-up-adobe-stock-cloud-services}

1. 在「**工具>Cloud Services」下，為Adobe Stock建立新的雲端服務設定。**
2. 選取在上節中為您的&#x200B;*Adobe Stock雲端*&#x200B;設定建立的&#x200B;*AdobeIMS設定*

3. 請務必選擇&#x200B;**ENVIRONMENT**&#x200B;作為PROD。 測試環境不受支援，將於下一版AEM中移除。
4. **授權的** 資產路徑可指向/content/dam下的任何目錄。下一版AEM將新增此欄位的功能支援
5. 選擇您的地區並完成設定。
6. 您也可以將使用者/群組新增至您的Adobe Stock雲端服務，以啟用特定使用者或群組的存取權。

![Adobe資產庫存設定](assets/screen_shot_2018-10-22at12425pm.png)

### 其他資源

* [企業股票計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2發行說明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [整合AEM和Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [Adobe I/O主控台整合API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API檔案](https://www.adobe.io/apis/creativecloud/stock/docs.html)