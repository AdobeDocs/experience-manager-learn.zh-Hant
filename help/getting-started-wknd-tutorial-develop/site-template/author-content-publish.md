---
title: 製作和發佈簡介 | AEM快速網站建立
description: 使用Adobe Experience Manager、AEM中的頁面編輯器來更新網站內容。 瞭解如何使用元件促進撰寫。 瞭解AEM作者與發佈環境之間的差異，並瞭解如何發佈對已上線網站的變更。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 0%

---

# 製作和發佈簡介 {#author-content-publish}

瞭解使用者將如何更新網站內容很重要。 在本章中，我們將採用&#x200B;**內容作者**&#x200B;的角色，並對上一章產生的網站進行一些編輯更新。 本章結束時，我們將發佈變更，以瞭解如何更新即時網站。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設已完成[建立網站](./create-site.md)章節中概述的步驟。

## 目標 {#objective}

1. 瞭解AEM Sites中&#x200B;**頁面**&#x200B;和&#x200B;**元件**&#x200B;的概念。
1. 瞭解如何更新網站內容。
1. 瞭解如何將變更發佈至已上線的網站。

## 建立新頁面 {#create-page}

網站通常會分成多個頁面，以形成多頁體驗。 AEM會以相同方式建構內容。 接著，建立網站的新頁面。

1. 登入上一章使用的AEM **作者**&#x200B;服務。
1. 從AEM開始畫面按一下&#x200B;**網站** > **WKND網站** > **英文** > **文章**
1. 按一下右上角的&#x200B;**建立** > **頁面**。

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這會顯示&#x200B;**建立頁面**&#x200B;精靈。

1. 選擇&#x200B;**文章頁面**&#x200B;範本並按一下&#x200B;**下一步**。

   AEM中的頁面是根據頁面範本建立的。 在[頁面範本](page-templates.md)章節中詳細探索頁面範本。

1. 在「**屬性**」下輸入「Hello World」的&#x200B;**標題**。
1. 將&#x200B;**名稱**&#x200B;設為`hello-world`並按一下&#x200B;**建立**。

   ![初始頁面屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話方塊快顯視窗中，按一下&#x200B;**開啟**&#x200B;以開啟新建立的頁面。

## 編寫元件 {#author-component}

AEM元件可視為網頁的小型模組建置區塊。 將UI分成邏輯區塊或元件，可讓管理更容易。 若要重複使用元件，必須可設定元件。 這是透過作者對話方塊完成。

AEM提供一組[已生產就緒可供使用的「核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hant)」。 **核心元件**&#x200B;的範圍包括基本元素（例如[Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)和[Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)）到更複雜的UI元素（例如[輪播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)）。

接下來，使用AEM頁面編輯器編寫一些元件。

1. 導覽至上一個練習建立的&#x200B;**Hello World**&#x200B;頁面。
1. 確定您處於&#x200B;**編輯**&#x200B;模式，並在左側邊欄按一下&#x200B;**元件**&#x200B;圖示。

   ![頁面編輯器側欄](assets/author-content-publish/page-editor-siderail.png)

   這樣會開啟元件程式庫，並列出可用於頁面上的可用元件。

1. 向下捲動並將&#x200B;**拖放** **文字(v2)**&#x200B;元件至頁面的主要可編輯區域。

   ![拖放文字元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下&#x200B;**文字**&#x200B;元件以反白顯示，然後按一下&#x200B;**扳手**&#x200B;圖示![扳手圖示](assets/author-content-publish/wrench-icon.png)以開啟元件的對話方塊。 輸入一些文字並將變更儲存到對話方塊。

   ![RTF元件](assets/author-content-publish/rich-text-populated-component.png)

   **Text**&#x200B;元件現在應該在頁面上顯示RTF文字。

1. 重複上述步驟，但將&#x200B;**Image(v2)**&#x200B;元件的執行個體拖曳到頁面上除外。 開啟&#x200B;**影像**&#x200B;元件的對話方塊。

1. 在左側邊欄中，按一下&#x200B;**Assets**&#x200B;圖示![資產圖示](assets/author-content-publish/asset-icon.png)以切換至&#x200B;**資產尋找器**。
1. **拖放**&#x200B;影像到元件的對話方塊中，然後按一下&#x200B;**完成**&#x200B;以儲存變更。

   ![將資產新增至對話方塊](assets/author-content-publish/add-asset-dialog.png)

1. 請注意，頁面上有已修正的元件，例如&#x200B;**Title**、**Navigation**、**Search**。 這些區域已設定為頁面範本的一部分，無法在個別頁面上修改。 這會在下一章中進一步探討。

您可以隨意嘗試其他元件。 如需各個[核心元件的相關檔案，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hant)。 有關[頁面編寫的詳細影片系列可在此找到](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)。

## 發佈更新 {#publish-updates}

AEM環境在&#x200B;**作者服務**&#x200B;和&#x200B;**發佈服務**&#x200B;之間分割。 在本章中，我們已對&#x200B;**作者服務**&#x200B;上的網站進行數次修改。 為了讓網站訪客檢視變更，我們需要將它們發佈到&#x200B;**發佈服務**。

![高階圖表](assets/author-content-publish/author-publish-high-level-flow.png)

*從作者到發佈的高層級內容流程*

**1。**&#x200B;內容作者更新網站內容。 您可以預覽、稽核及核准更新，以即時推送。

**2。**&#x200B;內容已發佈。 可隨選執行或排程在未來日期發佈。

**3。**&#x200B;個網站訪客將看到變更反映在發佈服務上。

### 發佈變更

接下來，讓我們發佈變更。

1. 從AEM開始畫面導覽至&#x200B;**網站**&#x200B;並選取&#x200B;**WKND網站**。
1. 按一下功能表列中的&#x200B;**管理出版物**。

   ![管理出版物](assets/author-content-publish/click-manage-publiciation.png)

   由於這是全新的網站，因此我們想發佈所有頁面，而且可以使用「管理出版物」精靈來定義需要發佈的確切內容。

1. 在「**選項**」下，將預設設定保留為「**發佈**」，並排程為「**立即**」。 按一下「**下一步**」。
1. 在&#x200B;**領域**&#x200B;下，選取&#x200B;**WKND網站**，然後按一下&#x200B;**包含子系設定**。 在對話方塊中，核取&#x200B;**包含子項**。 取消勾選其餘方塊，以確保發佈整個網站。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下&#x200B;**已發佈的參考**&#x200B;按鈕。 在對話方塊中，確認已核取所有專案。 這將包括&#x200B;**標準網站範本**&#x200B;和網站範本產生的數個設定。 按一下&#x200B;**完成**&#x200B;以進行更新。

   ![發佈參考](assets/author-content-publish/publish-references.png)

1. 最後，核取&#x200B;**WKND網站**&#x200B;旁的方塊，然後按一下右上角的&#x200B;**下一步**。
1. 在&#x200B;**工作流程**&#x200B;步驟中，輸入&#x200B;**工作流程標題**。 這可以是任何文字，並可用於後續的稽核軌跡中。 輸入「初始發佈」並按一下&#x200B;**發佈**。

![工作流程步驟初始發佈](assets/author-content-publish/workflow-step-publish.png)

## 檢視發佈的內容 {#publish}

接下來，導覽至「發佈」服務以檢視變更。

1. 取得Publish服務URL的簡單方法是復製作者URL並將`author`字取代為`publish`。 例如：

   * **作者URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 將`/content/wknd.html`新增至發佈URL，使最終URL看起來像是： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 變更`wknd.html`以符合您的網站名稱（如果您在[網站建立](create-site.md)期間提供唯一名稱）。

1. 導覽至發佈URL時，您應該會看到網站，沒有任何AEM撰寫功能。

   ![已發佈的網站](assets/author-content-publish/publish-url-update.png)

1. 使用&#x200B;**導覽**&#x200B;功能表按一下&#x200B;**文章** > **Hello World**，以導覽至先前建立的Hello World頁面。
1. 返回&#x200B;**AEM作者服務**，並在頁面編輯器中進行一些額外的內容變更。
1. 按一下&#x200B;**頁面屬性**&#x200B;圖示> **發佈頁面**，直接從頁面編輯器發佈這些變更

   ![直接發佈](assets/author-content-publish/page-editor-publish.png)

1. 返回&#x200B;**AEM發佈服務**&#x200B;以檢視變更。 您很可能不會&#x200B;**立即**&#x200B;看到更新。 這是因為&#x200B;**AEM發佈服務**&#x200B;包含透過Apache Web伺服器和CDN[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)的快取。 依預設，快取HTML內容約5分鐘。

1. 若要略過快取以進行測試/偵錯，只需新增查詢引數，例如`?nocache=true`。 URL看起來會像`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。 有關可用快取策略與設定的詳細資訊[可在此找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您也可以在Cloud Manager中找到發佈服務的URL。 瀏覽至&#x200B;**Cloud Manager方案** > **環境** > **環境**。

   ![檢視發佈服務](assets/author-content-publish/view-environment-segments.png)

   在&#x200B;**環境區段**&#x200B;下方，您可以找到&#x200B;**作者**&#x200B;和&#x200B;**發佈**&#x200B;服務的連結。

## 恭喜！ {#congratulations}

恭喜，您剛才已撰寫並發佈變更至您的AEM網站！

### 後續步驟 {#next-steps}

在真實世界的實作中，使用模型和UI設計規劃網站通常會在建立網站之前進行。 瞭解Adobe XD UI Kit如何透過Adobe XD[&#128279;](./ui-planning-adobe-xd.md)在UI規劃中設計和加速Adobe Experience Manager Sites實施。

想要繼續探索AEM Sites功能嗎？ 您可以直接跳到[頁面範本](./page-templates.md)上的章節，瞭解頁面範本與頁面之間的關係。


