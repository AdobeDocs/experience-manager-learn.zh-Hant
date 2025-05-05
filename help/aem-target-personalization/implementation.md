---
title: 將AEM Sites與Adobe Target整合
description: 本文會說明如何針對不同案例使用Adobe Target設定Adobe Experience Manager。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# 將AEM Sites與Adobe Target整合

在本節中，我們將討論如何針對不同案例使用Adobe Target設定Adobe Experience Manager Sites。 根據您的情境和組織需求。

* **新增Adobe Target JavaScript資料庫（所有案例均需要）**
對於在AEM上託管的網站，您可以使用Adobe Experience Platform[&#128279;](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=zh-Hant)中的標籤將Target資料庫新增至您的網站。 標籤提供一種簡單的方式來部署及管理所有必要的標籤，以便支援相關客戶體驗。
* **新增Adobe TargetCloud Service（體驗片段案例所需）**
如果是AEM客戶，想使用體驗片段選件在Adobe Target中建立活動，您需要使用舊版Cloud Service整合Adobe Target與AEM。 從AEM推送體驗片段做為HTML/JSON選件至Target，以及保持選件與AEM同步時，需要這項整合。 *實作案例1需要此整合。*

## 先決條件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 （建議使用&#x200B;*最新Service Pack*）
   * 下載AEM WKND reference-site套件
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心元件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數位資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建下列解決方案的Experience Cloud
      * [資料彙集](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O主控台](https://console.adobe.io)

* **環境**
   * Java 1.8或Java 11 (僅限AEM 6.5+)
   * Apache Maven （3.3.9或更新版本）
   * Chrome

>[!NOTE]
>
> 客戶需要布建來自[Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html)的資料收集和Adobe I/O，或聯絡您的系統管理員

### 設定AEM{#set-up-aem}

您必須有AEM作者和發佈例項，才能完成本教學課程。 作者執行個體在`http://localhost:4502`上執行，而發佈執行個體在`http://localhost:4503`上執行。 如需詳細資訊，請參閱： [設定本機AEM開發環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 設定AEM Author和Publish例項

1. 取得[AEM Quickstart Jar的復本和授權。](https://helpx.adobe.com/tw/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在電腦上建立檔案夾結構，如下所示：
   ![資料夾結構](assets/implementation/aem-setup-1.png)
3. 將Quickstart jar重新命名為`aem-author-p4502.jar`，並將其放在`/author`目錄下。 在`/author`目錄下新增`license.properties`檔案。
   ![AEM作者執行個體](assets/implementation/aem-setup-author.png)
4. 複製Quickstart jar，將其重新命名為`aem-publish-p4503.jar`，並放置在`/publish`目錄下。 在`/publish`目錄下新增`license.properties`檔案的復本。
   ![AEM Publish執行個體](assets/implementation/aem-setup-publish.png)
5. 按兩下`aem-author-p4502.jar`檔案以安裝作者執行個體。 這會啟動編寫執行個體，在本機電腦的連線埠4502上執行。
6. 使用下列憑證登入，系統會在您成功登入後，將您導向至AEM首頁畫面。
使用者名稱：**管理員**
密碼： **管理員**
   ![AEM Publish執行個體](assets/implementation/aem-author-home-page.png)
7. 按兩下`aem-publish-p4503.jar`檔案以安裝發佈執行個體。 您會發現瀏覽器中針對您的發佈執行個體開啟了一個新索引標籤，此索引標籤會在連線埠4503上執行並顯示WeRetail首頁。 在本教學課程中，我們使用WKND參考網站，並將套件安裝在作者執行個體上。
8. 在網頁瀏覽器的`http://localhost:4502`瀏覽至AEM作者。 在AEM開始畫面上，瀏覽至&#x200B;*[工具>部署>套件](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下載並上傳AEM的套件(列於上述&#x200B;*[先決條件> AEM](#aem)*&#x200B;下)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安裝套件後，請在AEM Package Manager中選取每個已上傳的套件，然後選取「**更多>復寫**」以確保套件已部署至AEM Publish。
11. 此時，您已成功安裝WKND參考網站及本教學課程所需的所有其他套件。

[下一章](./using-launch-adobe-io.md)：在下一章中，您會將標籤與AEM整合。
