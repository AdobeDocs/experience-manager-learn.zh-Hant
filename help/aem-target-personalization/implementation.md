---
title: Adobe Experience Manager與Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: 一篇文章，介紹如何與Adobe Target建立Adobe Experience Manager，以應對不同的情景。
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

# Adobe Experience Manager與Adobe Target

在本節中，我們將討論如何針對不同情形與Adobe Target建立Adobe Experience Manager。 根據您的方案和組織要求。

* **添加Adobe TargetJavaScript庫（所有方案都需要）**
對於上承載的站AEM點，可以使用 [啟動](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)。 Launch提供了部署和管理所有必要標籤的簡單方法，以便為相關客戶體驗提供動力。
* **添加Adobe TargetCloud Services（「體驗片段」方案需要）**
對於AEM希望使用體驗片段產品在Adobe Target內建立活動的客戶，您需要將Adobe Target與使用舊式Cloud ServicesAEM進行整合。 此整合是將體驗片段作為HTML/AEMJSON提供的推送到目標，並與之保持同步所需的AEM整合。 
*實施方案1需要此整合。*

## 必備條件

* **Adobe Experience ManagerAEM({#aem}**
   * AEM 6.5(*建議使用最新的Service Pack*)
   * 下載AEMWKND參考站點包
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心元件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數字資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

* **環境**
   * Java 1.8或Java 11(AEM僅限6.5+)
   * Apache Maven (3.3.9 或更新版本)
   * 鉻

>[!NOTE]
>
> 客戶需要提供Experience Platform Launch和Adobe I/O [Adobe支援](https://helpx.adobe.com/tw/contact/enterprise-support.ec.html) 或聯繫您的系統管理員

### 設定AEM{#set-up-aem}

完成AEM本教程需要編寫和發佈實例。 作者實例正在運行 `http://localhost:4502` 發佈運行的實例 `http://localhost:4503`。 有關詳細資訊，請參閱： [設定本地開發AEM環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 設定AEM作者和發佈實例

1. 獲取副本 [快AEM速啟動Jar和許可證。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在電腦上建立資料夾結構，如下所示：
   ![資料夾結構](assets/implementation/aem-setup-1.png)
3. 將快速啟動jar更名為 `aem-author-p4502.jar` 放在下面 `/author` 的子菜單。 添加 `license.properties` 檔案下方 `/author` 的子菜單。
   ![AEM作者實例](assets/implementation/aem-setup-author.png)
4. 製作快速啟動jar的副本，將其更名為 `aem-publish-p4503.jar` 放在下面 `/publish` 的子菜單。 添加副本 `license.properties` 檔案下方 `/publish` 的子菜單。
   ![AEM發佈實例](assets/implementation/aem-setup-publish.png)
5. 按兩下 `aem-author-p4502.jar` 檔案以安裝作者實例。 這將啟動在本地電腦上的埠4502上運行的作者實例。
6. 使用下面的憑據登錄，成功登錄後，您將被定向到「主AEM頁」螢幕。
用戶名： **管理員**
密碼： **管理員**
   ![AEM發佈實例](assets/implementation/aem-author-home-page.png)
7. 按兩下 `aem-publish-p4503.jar` 檔案以安裝發佈實例。 您可以注意到瀏覽器中為發佈實例開啟的一個新頁籤，該頁籤在埠4503上運行並顯示WeRetail首頁。 本教程使用的是WKND參考站點，讓我們在作者實例上安裝軟體包。
8. 在Web瀏覽器中導航到AEM作者，網址為： `http://localhost:4502`。 在「開始AEM」螢幕上，導航至 *[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下載和上載包AEM(上面列於 *[先決條件> AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安裝包後，在包管理器中選擇每個上載AEM的包，然後選擇 **更多>複製** 以確保將包部署到AEM發佈。
11. 此時，您已成功安裝了WKND參考站點和本教程所需的所有其他軟體包。

[下一章](./using-launch-adobe-io.md):在下一章中，您將與Launch整合AEM。
