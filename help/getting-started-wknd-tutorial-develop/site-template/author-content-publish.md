---
title: 製作和發佈簡介 | AEM快速網站建立
description: 使用Adobe Experience Manager AEM中的頁面編輯器來更新網站內容。 瞭解如何使用元件促進撰寫。 瞭解AEM作者與發佈環境之間的差異，並瞭解如何發佈對已上線網站的變更。
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 2%

---

# 製作和發佈簡介 {#author-content-publish}

瞭解使用者將如何更新網站內容很重要。 在本章中，我們將採用 **內容作者** 並對上一章產生的網站進行編輯更新。 本章結束時，我們將發佈變更，以瞭解如何更新即時網站。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [建立網站](./create-site.md) 章節已完成。

## 目標 {#objective}

1. 瞭解以下概念 **頁面** 和 **元件** 在AEM Sites中。
1. 瞭解如何更新網站內容。
1. 瞭解如何將變更發佈至已上線的網站。

## 建立新頁面 {#create-page}

網站通常會分成多個頁面，以形成多頁體驗。 AEM會以相同的方式建構內容。 接著，建立網站的新頁面。

1. 登入AEM **作者** 上一章中使用的服務。
1. 從AEM開始畫面按一下 **網站** > **WKND網站** > **英文** > **文章**
1. 在右上角按一下 **建立** > **頁面**.

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這會顯示 **建立頁面** 精靈。

1. 選擇 **文章頁面** 範本並按一下 **下一個**.

   AEM中的頁面是根據頁面範本建立的。 有關頁面範本的詳細資訊，請參閱 [頁面範本](page-templates.md) 章節。

1. 在 **屬性** 輸入 **標題** 「Hello World」的縮寫。
1. 設定 **名稱** 成為 `hello-world` 並按一下 **建立**.

   ![初始頁面屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話方塊快顯視窗中，按一下 **開啟** 以開啟新建立的頁面。

## 編寫元件 {#author-component}

AEM元件可視為網頁的小型模組建置區塊。 將UI分成邏輯區塊或元件，可讓管理更容易。 若要重複使用元件，必須可設定元件。 這是透過作者對話方塊完成。

AEM提供一組 [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 已準備好使用的生產環境。 此 **核心元件** 從基本元素範圍，例如 [文字](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 和 [影像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 至更複雜的UI元素，例如 [輪播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

接下來，使用AEM頁面編輯器編寫一些元件。

1. 導覽至 **Hello World** 在上一個練習中建立的頁面。
1. 確保您位於 **編輯** 模式，並在左側邊欄中按一下 **元件** 圖示。

   ![頁面編輯器側欄](assets/author-content-publish/page-editor-siderail.png)

   這樣會開啟元件程式庫，並列出可用於頁面上的可用元件。

1. 向下捲動並 **拖放** a **文字(v2)** 元件移至頁面的主要可編輯區域。

   ![拖放文字元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下 **文字** 反白的元件，然後按一下 **扳手** 圖示 ![扳手圖示](assets/author-content-publish/wrench-icon.png) 以開啟元件的對話方塊。 輸入一些文字並將變更儲存到對話方塊。

   ![RTF元件](assets/author-content-publish/rich-text-populated-component.png)

   此 **文字** 元件現在應在頁面上顯示RTF文字。

1. 重複上述步驟，但拖曳 **影像(v2)** 元件移至頁面上。 開啟 **影像** 元件的對話方塊。

1. 在左側邊欄中，切換至 **資產尋找器** 按一下 **資產** 圖示 ![資產圖示](assets/author-content-publish/asset-icon.png).
1. **拖放** 將影像放入元件的對話方塊中，然後按一下 **完成** 以儲存變更。

   ![將資產新增至對話方塊](assets/author-content-publish/add-asset-dialog.png)

1. 請注意，頁面上有一些元件，例如 **標題**， **導覽**， **搜尋** 已修正的問題。 這些區域已設定為頁面範本的一部分，無法在個別頁面上修改。 這會在下一章中進一步探討。

您可以隨意嘗試其他元件。 關於每個專案的檔案 [您可以在此處找到核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 詳細的影片系列介紹 [頁面製作可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 發佈更新 {#publish-updates}

AEM環境分為兩種 **作者服務** 和 **發佈服務**. 在本章中，我們已在以下網址對網站進行數項修改： **作者服務**. 為了讓網站訪客檢視變更，我們需要將它們發佈到 **發佈服務**.

![高階圖表](assets/author-content-publish/author-publish-high-level-flow.png)

*從製作到發佈的高層內容流程*

**1.** 內容作者會更新網站內容。 您可以預覽、稽核及核准更新，以即時推送。

**2.** 內容已發佈。 可隨選執行或排程在未來日期發佈。

**3.** 網站訪客將看到變更反映在發佈服務上。

### 發佈變更

接下來，讓我們發佈變更。

1. 從AEM開始畫面導覽至 **網站** 並選取 **WKND網站**.
1. 按一下 **管理發布** 功能表列中的。

   ![管理出版物](assets/author-content-publish/click-manage-publiciation.png)

   由於這是全新的網站，因此我們想發佈所有頁面，而且可以使用「管理出版物」精靈來定義需要發佈的確切內容。

1. 在 **選項** 將預設設定保留為 **發佈** 並排程 **現在**. 按一下「**下一步**」。
1. 在 **範圍**，選取 **WKND網站** 並按一下 **包含子設定**. 在對話方塊中，核取 **包含子項**. 取消勾選其餘方塊，以確保發佈整個網站。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下 **已發佈引用** 按鈕。 在對話方塊中，確認已核取所有專案。 這將包括 **標準網站範本** 以及網站範本產生的數個設定。 按一下 **完成** 以更新。

   ![發佈參考](assets/author-content-publish/publish-references.png)

1. 最後，勾選旁的方塊 **WKND網站** 並按一下 **下一個** 位於右上角。
1. 在 **工作流程** 步驟，輸入 **工作流程標題**. 這可以是任何文字，並可用於後續的稽核軌跡中。 輸入「初始發佈」並按一下 **發佈**.

![工作流程步驟初始發佈](assets/author-content-publish/workflow-step-publish.png)

## 檢視發佈的內容 {#publish}

接下來，導覽至「發佈」服務以檢視變更。

1. 取得Publish服務URL的簡單方法是復製作者URL並取代 `author` 文字與 `publish`. 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 新增 `/content/wknd.html` 至發佈URL，使最終URL看起來像這樣： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 變更 `wknd.html` 比對網站名稱，前提是您在 [網站建立](create-site.md).

1. 瀏覽至發佈URL，您應該會看到網站，沒有任何AEM編寫功能。

   ![已發佈的網站](assets/author-content-publish/publish-url-update.png)

1. 使用 **導覽** 功能表點按 **文章** > **Hello World** 以導覽至先前建立的Hello World頁面。
1. 返回 **AEM作者服務** 並在頁面編輯器中進行一些額外的內容變更。
1. 按一下「 」，直接從頁面編輯器發佈這些變更 **頁面屬性** 圖示> **發佈頁面**

   ![直接發佈](assets/author-content-publish/page-editor-publish.png)

1. 返回 **AEM發佈服務** 以檢視變更。 您很可能會 **非** 立即檢視更新。 這是因為 **AEM發佈服務** 包含 [透過Apache網頁伺服器和CDN快取](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). 根據預設，快取HTML內容約5分鐘。

1. 若要略過快取以進行測試/偵錯，只需新增查詢引數，例如 `?nocache=true`. URL看起來像這樣 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 有關可用快取策略和設定的更多詳細資料 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. 您也可以在Cloud Manager中找到發佈服務的URL。 導覽至 **Cloud Manager計畫** > **環境** > **環境**.

   ![檢視發佈服務](assets/author-content-publish/view-environment-segments.png)

   在 **環境區段** 您可以找到 **作者** 和 **發佈** 服務。

## 恭喜！ {#congratulations}

恭喜，您剛才已撰寫並發佈變更至您的AEM網站！

### 後續步驟 {#next-steps}

在真實世界的實作中，使用模型和UI設計規劃網站通常會在建立網站之前進行。 瞭解如何使用Adobe XD UI Kit來設計和加速您在中的Adobe Experience Manager Sites實施 [使用Adobe XD進行UI規劃](./ui-planning-adobe-xd.md).

想要繼續探索AEM Sites功能嗎？ 您可以直接跳至上的章節 [頁面範本](./page-templates.md) 以瞭解頁面範本和頁面之間的關係。


