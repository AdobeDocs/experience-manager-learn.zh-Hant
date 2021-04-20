---
title: 整合Adobe Experience Manager與Adobe Target
seo-title: 文章涵蓋將Adobe Experience Manager(AEM)與Adobe Target整合以提供個人化內容的不同方式。
description: 一篇文章，介紹如何為不同情景設立Adobe Experience Manager與Adobe Target。
seo-description: 一篇文章，介紹如何為不同情景設立Adobe Experience Manager與Adobe Target。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 4%

---


# 整合Adobe Experience Manager與Adobe Target

在本節中，我們將討論如何針對不同情況與Adobe Target建立Adobe Experience Manager。 根據您的藍本和組織需求。

* **新增Adobe TargetJavaScript程式庫(所有藍本都**
是必要的) [對於AEM裝載在的網站，您可以使用Launch將Target程式庫新增至您的網](https://docs.adobe.com/content/help/en/launch/using/overview.html)站。Launch提供簡單的方式，來部署及管理所有必要標籤，以強化相關的客戶體驗。
* **新增Adobe TargetCloud Services（體驗片段藍本的必要項）**
AEM對於想要使用體驗片段選件在Adobe Target建立活動的客戶，您需要將Adobe Target與舊版Cloud ServicesAEM整合。必須進行此整合，才能將體驗片段AEM推送至Target做為HTML/JSON選件，並讓選件與之保持同AEM步。 
*實施方案1需要此整合。*

## 必備條件

* **Adobe Experience ManagerAEM(){#aem}**
   * AEM 6.5（建議使用最新的Service Pack *）*
   * 下載AEMWKND參考網站套件
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心元件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數位資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

* **環境**
   * Java 1.8或Java 11(AEM僅限6.5+)
   * Apache Maven (3.3.9 或更新版本)
   * Chrome

>[!NOTE]
>
> 客戶需要從[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)向系統管理員提供Experience Platform Launch和Adobe I/O

### 設定AEM{#set-up-aem}

製作AEM和發佈例項是完成本教學課程的必要條件。 我們在`http://localhost:4502`上執行作者例項，並在`http://localhost:4503`上執行publish例項。 如需詳細資訊，請參閱：[設定本AEM地開發環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 設定AEM作者和發佈例項

1. 獲取[快速AEM入門Jar和許可證的副本。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在您的電腦上建立資料夾結構，如下所示：
   ![資料夾結構](assets/implementation/aem-setup-1.png)
3. 將快速啟動jar更名為`aem-author-p4502.jar` ，並將其放在`/author`目錄下。 在`/author`目錄下添加`license.properties`檔案。
   ![AEM Author Instance](assets/implementation/aem-setup-author.png)
4. 製作快速啟動jar的副本，將其更名為`aem-publish-p4503.jar` ，並將其放在`/publish`目錄下。 在`/publish`目錄下添加`license.properties`檔案的副本。
   ![AEM Publish Instance](assets/implementation/aem-setup-publish.png)
5. 連按兩下`aem-author-p4502.jar`檔案以安裝Author執行個體。 這將啟動作者實例，在本地電腦的埠4502上運行。
6. 使用下列認證登入，成功登入後，您會被導向至首頁AEM畫面。
用戶名：**admin**
密碼：**admin**
   ![AEM Publish Instance](assets/implementation/aem-author-home-page.png)
7. 連按兩下`aem-publish-p4503.jar`檔案以安裝發佈例項。 您可以注意到，瀏覽器中針對您的發佈例項開啟了一個新標籤，執行於連接埠4503並顯示WeRetail首頁。 我們將在本教學課程中使用WKND參考網站，讓我們在作者實例上安裝這些套件。
8. 在您的網頁瀏覽器中導覽至`http://localhost:4502`的「AEM作者」。 在「開AEM始」螢幕中，導航至&#x200B;*[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下載並上傳AEM的套件(列於&#x200B;*[必要條件> AEM](#aem)*&#x200B;下)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安裝套件後，請在AEM Package Manager中選取每個已上傳的套件，然後選取「**更多>複製**」，以確保套件已部署至AEM Publish。
11. 目前，您已成功安裝WKND參考網站和本教學課程所需的所有其他套件。

[下一章](./using-launch-adobe-io.md):在下一章中，您將整合Launch與AEM。
