---
title: 整合Adobe Experience Manager與Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: 說明如何針對不同情況使用Adobe Target設定Adobe Experience Manager的文章。
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 4%

---

# 整合Adobe Experience Manager與Adobe Target

在本節中，我們將討論如何針對不同的情況，與Adobe Target設定Adobe Experience Manager。 根據您的藍本和組織需求。

* **新增Adobe Target JavaScript程式庫（所有案例均為必要）**
對於AEM上托管的網站，您可以使用 [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch提供部署及管理所有必要標籤的簡單方式，以便支援相關客戶體驗。
* **新增Adobe TargetCloud Services（體驗片段案例的必要項目）**
若AEM客戶想要使用體驗片段選件在Adobe Target中建立活動，您需要使用舊版Cloud Services將Adobe Target與AEM整合。 若要將體驗片段從AEM推送至Target做為HTML/JSON選件，並且讓選件與AEM保持同步，此整合是必要的。 
*實施案例1需要此整合。*

## 必備條件

* **Adobe Experience Manager(AEM){#aem}**
   * AEM 6.5(*建議使用最新的Service Pack*)
   * 下載AEM WKND參考網站套件
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心元件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數位資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud已布建以下解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O主控台](https://console.adobe.io)

* **環境**
   * Java 1.8或Java 11(僅限AEM 6.5+)
   * Apache Maven (3.3.9 或更新版本)
   * 鉻黃

>[!NOTE]
>
> 客戶需要布建Experience Platform Launch和Adobe I/O [Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html) 或聯繫您的系統管理員

### 設定AEM{#set-up-aem}

完成本教學課程需要AEM製作和發佈例項。 我們正在執行Author例項 `http://localhost:4502` 和發佈執行個體 `http://localhost:4503`. 如需詳細資訊，請參閱： [設定本機AEM開發環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### 設定AEM製作和發佈例項

1. 取得 [AEM Quickstart Jar和許可證。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在電腦上建立資料夾結構，如下所示：
   ![資料夾結構](assets/implementation/aem-setup-1.png)
3. 將Quickstart Jar更名為 `aem-author-p4502.jar` 把它放在下面 `/author` 目錄。 新增 `license.properties` 檔案 `/author` 目錄。
   ![AEM Author例項](assets/implementation/aem-setup-author.png)
4. 製作快速啟動Jar的副本，將其更名為 `aem-publish-p4503.jar` 把它放在下面 `/publish` 目錄。 新增 `license.properties` 檔案 `/publish` 目錄。
   ![AEM發佈例項](assets/implementation/aem-setup-publish.png)
5. 按兩下 `aem-author-p4502.jar` 檔案來安裝Author例項。 這會啟動製作執行個體，在本機電腦的連接埠4502上執行。
6. 使用下列憑證登入，而成功登入時，系統會將您導向至AEM首頁畫面。
使用者名稱： **管理員**
密碼： **管理員**
   ![AEM發佈例項](assets/implementation/aem-author-home-page.png)
7. 按兩下 `aem-publish-p4503.jar` 檔案來安裝發佈執行個體。 您可以注意到，瀏覽器中針對您的發佈執行個體開啟了一個新索引標籤，執行於連接埠4503，並顯示WeRetail首頁。 我們使用本教學課程的WKND參考網站，現在將套件安裝在製作執行個體上。
8. 在網頁瀏覽器中導覽至「AEM作者」，網址為 `http://localhost:4502`. 在AEM開始畫面上，導覽至 *[工具>部署>套件](http://localhost:4502/crx/packmgr/index.jsp)*.
9. 下載和上傳AEM的套件(如上所列： *[必要條件> AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安裝套件後，在AEM Package Manager中選取每個已上傳的套件，然後選取 **更多>複製** 以確保套件已部署至AEM Publish。
11. 此時，您已成功安裝了WKND參考網站和本教學課程所需的所有其他套件。

[下一章](./using-launch-adobe-io.md):在下一章中，您將整合Launch與AEM。
