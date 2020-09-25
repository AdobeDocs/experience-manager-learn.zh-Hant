---
title: 整合Adobe Experience Manager與Adobe Target
seo-title: 文章涵蓋將Adobe Experience Manager(AEM)與Adobe Target整合以提供個人化內容的不同方式。
description: 文章說明如何針對不同藍本設定Adobe Experience Manager與Adobe Target。
seo-description: 文章說明如何針對不同藍本設定Adobe Experience Manager與Adobe Target。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 2%

---


# 整合Adobe Experience Manager與Adobe Target

在本節中，我們將討論如何針對不同的藍本，使用Adobe Target來設定Adobe Experience Manager。 根據您的藍本和組織需求。

* **新增Adobe Target JavaScript程式庫（所有藍本皆為必要）**&#x200B;對於AEM上裝載的網站，您可以使用 [Launch將Target程式庫新增至您的網站](https://docs.adobe.com/content/help/en/launch/using/overview.html)。 Launch提供簡單的方式，來部署及管理所有必要標籤，以強化相關的客戶體驗。
* **新增Adobe Target雲端服務（「體驗片段」藍本的必要項目）** AEM客戶若想使用「體驗片段」選件在Adobe Target中建立活動，您將需要使用舊版雲端服務將Adobe Target與AEM整合。 必須進行此整合，才能將「體驗片段」從AEM推送至Target作為HTML/JSON選件，並讓選件與AEM保持同步。 
*實施方案1需要此整合。*

## 必備條件

* **Adobe Experience Manager(AEM){#aem}**
   * AEM 6.5(建議使&#x200B;*用最新的Service Pack*)
   * 下載AEM WKND參考網站套件
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心元件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數位資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建下列解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

* **環境**
   * Java 1.8或Java 11（僅限AEM 6.5+）
   * Apache Maven（3.3.9或更新版本）
   * Chrome

>[!NOTE]
>
> 客戶需要透過 [Adobe支援布建Experience Platform Launch和Adobe I/O](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html) ，或聯絡系統管理員

### 設定AEM{#set-up-aem}

AEM作者和發佈例項是完成本教學課程的必要條件。 我們已在上執行作者例項， `http://localhost:4502` 並在上執行發佈例項 `http://localhost:4503`。 如需詳細資訊，請參閱： [設定本機AEM開發環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 設定AEM作者和發佈例項

1. 取得 [AEM Quickstart Jar和授權。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在您的電腦上建立資料夾結構，如下所示：
   ![資料夾結構](assets/implementation/aem-setup-1.png)
3. 將快速啟動jar更名 `aem-author-p4502.jar` 為並將其放在目錄 `/author` 下。 在目錄 `license.properties` 下添加文 `/author` 件。
   ![AEM Author Instance](assets/implementation/aem-setup-author.png)
4. 製作快速啟動jar的副本，將其更名 `aem-publish-p4503.jar` 並將其放在目 `/publish` 錄下。 在目錄下添加 `license.properties` 檔案副 `/publish` 本。
   ![AEM Publish Instance](assets/implementation/aem-setup-publish.png)
5. 連按兩下檔 `aem-author-p4502.jar` 案以安裝Author執行個體。 這將啟動作者實例，在本地電腦的埠4502上運行。
6. 使用下列認證登入，成功登入後，您會被導向至「AEM首頁畫面」。
用戶名： **管理**&#x200B;員密碼： **管理員**
   ![AEM Publish Instance](assets/implementation/aem-author-home-page.png)
7. 連按兩下 `aem-publish-p4503.jar` 檔案以安裝發佈例項。 您可以注意到，瀏覽器中針對您的發佈例項開啟了一個新標籤，執行於連接埠4503並顯示WeRetail首頁。 我們將在本教學課程中使用WKND參考網站，讓我們在作者實例上安裝這些套件。
8. 在您的網頁瀏覽器中導覽至「AEM作者」，網址為 `http://localhost:4502`:。 在「AEM開始」畫面上，導覽至「工 *[具>部署>套件」](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下載並上傳AEM的套件(列在「必要條件> AEM *[」下方](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安裝套件後，在AEM Package Manager中選取每個已上傳的套件，然後選取「 **More > Replicate** 」（更多>複製），以確保套件已部署至AEM Publish。
11. 目前，您已成功安裝WKND參考網站和本教學課程所需的所有其他套件。

[下一章](./using-launch-adobe-io.md):在下一章中，您將整合Launch與AEM。
