---
title: 搭配AEM Assets使用Adobe Stock資產
description: AEM讓使用者可直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 各組織現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保其創意和行銷專案現在可廣泛使用授權資產，並具備AEM的強大資產管理功能。
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1130
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 1%

---

# 搭配AEM Assets使用Adobe Stock{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2讓使用者可直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 各組織現在可以將其Adobe Stock企業計畫與AEM Assets整合，以確保其創意和行銷專案現在可廣泛使用授權資產，並具備AEM的強大資產管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>整合需要 [企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4 （至少部署Service Pack 2）。 如需AEM 6.4 Service Pack的詳細資訊，請參閱以下內容 [發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html).

Adobe Stock和AEM Assets整合可讓內容作者和行銷人員輕鬆授權並使用股票資產，以用於創意或行銷目的。 您可以使用Omni Search、將位置篩選器新增為Adobe Stock，或透過AEM Assets主要導覽列導覽並按一下「搜尋Adobe Stock Coral UI」圖示，來執行Stock資產搜尋。

## 功能

### 搜尋並儲存

* 在不離開AEM工作區的情況下執行Adobe Stock資產搜尋。
* 儲存Adobe Stock資產以供預覽，無需授權資產。
* 能夠授權並儲存Adobe Stock資產至AEM Assets
* 能夠在AEM Assets UI中從Adobe Stock搜尋類似的資產
* 在Adobe Stock網站的AEM Assets中，從Stock搜尋檢視選取的資產
* 授權資產檔案會以藍色授權徽章標籤，以便輕鬆識別

### 資產中繼資料

* 授權的資產會儲存在AEM Assets中。 資產屬性會在單獨的資產中繼資料標籤下包含Stock中繼資料
* 能夠將授權參考新增至資產中繼資料

### 資產庫存設定檔

* 使用者可選取下方的Adobe Stock設定檔 *使用者>我的偏好設定> Stock設定*
* 您可以在「資產授權」視窗中新增必要和選用參考。
* 能夠根據地區選擇「資產授權」視窗的語言偏好設定。

### 篩選

* 使用者可以根據資產型別、方向和檢視類似來篩選庫存資產
* 資產型別包括像片、插圖、向量、影片、範本、3D、Premium、編輯
* 「方向」包括「水準」、「垂直」和「正方形」。
* 檢視類似篩選器需要Adobe Stock檔案編號

### 存取控制

* 管理員可在設定Adobe Stock雲端服務設定時，為特定使用者/群組提供授權Stock資產的許可權。
* 如果特定使用者/群組沒有授權Stock資產的許可權， *Stock資產搜尋/資產授權* 功能將會停用。

## 使用AEM Assets設定Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2讓使用者可直接從AEM搜尋、預覽、儲存及授權Adobe Stock資產。 本影片涵蓋如何使用「Adobe主控台」透過AEM Assets設定Adobe I/O股票的快速逐步解說。

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>針對Adobe Stock雲端服務設定，您必須選取生產環境和授權資產路徑指向 `/content/dam`. 環境欄位現在已在AEM中移除。

>[!NOTE]
>
>整合需要 [企業Adobe Stock計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4及以上 [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 已部署。 如需AEM 6.4 Service Pack的詳細資訊，請參閱以下內容 [發行說明](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html). 您還需要管理員許可權，才能 [Adobe I/O主控台](https://console.adobe.io/)， [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager以設定整合。

### 安裝 {#installations}

* 針對AEM 6.4，您必須安裝 [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 然後重新安裝cq-dam-stock-integration-content-1.0.4.zip檔案。
* 確定您擁有管理員許可權： [Adobe I/O主控台](https://console.adobe.io/)， [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager以設定整合。

#### 使用Adobe I/O控制檯設定Adobe IMS設定 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 建立Adobe IMS技術帳戶設定，於 **「工具」>「安全性」**
2. 選取 *雲端解決方案* 作為 *Adobe Stock* 和建立新憑證或重複使用現有憑證進行設定。
3. 導覽至Adobe I/O控制檯並建立新的服務帳戶整合 *Adobe Stock*.
4. 從Step2上傳憑證至您的Adobe Stock服務帳戶整合。
5. 選擇所需的Adobe Stock設定檔設定，並完成服務整合。
6. 使用整合詳細資料來完成Adobe IMS技術帳戶設定
7. 請務必使用Adobe IMS技術帳戶接收存取權杖。

![Adobe IMS技術帳戶](assets/screen_shot_2018-10-22at12219pm.png)

#### 設定Adobe StockCloud Service {#set-up-adobe-stock-cloud-services}

1. 在下為Adobe Stock建立新的雲端服務設定 **「工具>Cloud Service」。**
2. 選取 *Adobe IMS設定* 在上節中為建立 *Adobe Stock Cloud* 設定

3. 請務必選取 **環境** 作為PROD。
4. **授權資產路徑** 可指向下的任何目錄 `/content/dam`.
5. 選取您的地區設定並完成設定。
6. 您也可以將使用者/群組新增至您的Adobe Stock雲端服務，以啟用特定使用者或群組的存取權。

![Adobe資產庫存設定](assets/screen_shot_2018-10-22at12425pm.png)

### 其他資源

* [企業存量計畫](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2發行說明](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html)
* [整合AEM和Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O控制檯整合API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API檔案](https://www.adobe.io/apis/creativecloud/stock/docs.html)
