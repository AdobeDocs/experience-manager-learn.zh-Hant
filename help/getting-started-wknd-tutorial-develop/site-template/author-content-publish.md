---
title: 製作和發佈簡介 | AEM快速網站建立
description: 使用AEMAdobe Experience Manager中的頁面編輯器來更新網站的內容。 了解如何使用元件來加速編寫。 了解AEM製作和發佈環境之間的差異，並了解如何將變更發佈至即時網站。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 2%

---

# 製作和發佈簡介 {#author-content-publish}

>[!CAUTION]
>
> 快速網站建立工具目前是技術預覽。 除非經Adobe支援同意，否則可供測試及評估之用，且非供生產使用。

請務必了解使用者如何更新網站的內容。 在本章中，我們將採用 **內容作者** 並對上一章中產生的網站進行一些編輯更新。 在章節結尾，我們將發佈變更，以了解即時網站的更新方式。

## 必備條件 {#prerequisites}

此為多部分教學課程，假設要執行 [建立網站](./create-site.md) 章節已完成。

## 目標 {#objective}

1. 了解 **頁面** 和 **元件** 在AEM Sites。
1. 了解如何更新網站的內容。
1. 了解如何將變更發佈至上線網站。

## 建立新頁面 {#create-page}

網站通常會分割成頁面，以形成多頁體驗。 AEM以相同方式建構內容。 接下來，為網站建立新頁面。

1. 登入AEM **作者** 上一章中使用的服務。
1. 在AEM Start畫面中按一下 **網站** > **WKND站點** > **英文** > **文章**
1. 在右上角按一下 **建立** > **頁面**.

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這會顯示 **建立頁面** 嚮導。

1. 選擇 **文章頁面** 範本，按一下 **下一個**.

   AEM中的頁面是根據頁面範本建立。 如需深入探索頁面範本，請參閱 [頁面範本](page-templates.md) 章節。

1. 在 **屬性** 輸入 **標題** 《你好世界》
1. 設定 **名稱** 為 `hello-world` 按一下 **建立**.

   ![初始頁面屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話方塊快顯視窗中按一下 **開啟** 開啟新建立的頁面。

## 製作元件 {#author-component}

AEM元件可視為網頁的小型模組化建置組塊。 將UI分割為邏輯區塊或元件，可讓管理更輕鬆。 要重新使用元件，元件必須是可配置的。 這可透過製作對話方塊完成。

AEM提供 [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant) 生產就緒即可使用。 此 **核心元件** 範圍包括基本元素，如 [文字](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 和 [影像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 更複雜的UI元素，例如 [輪播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

接下來，使用AEM頁面編輯器編寫一些元件。

1. 導覽至 **《你好世界》** 頁面。
1. 確保您位於 **編輯** 模式，然後在左側邊欄中按一下 **元件** 表徵圖。

   ![頁面編輯器側邊欄](assets/author-content-publish/page-editor-siderail.png)

   這會開啟元件程式庫，並列出可在頁面上使用的可用元件。

1. 向下捲動並 **拖放** a **文字(v2)** 元件（位於頁面的主要可編輯區域）。

   ![拖曳+拖放文字元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下 **文字** 要突出顯示的元件，然後按一下 **扳手** 圖示 ![扳手圖示](assets/author-content-publish/wrench-icon.png) 以開啟元件的對話方塊。 輸入一些文字並將更改保存到對話框。

   ![RTF元件](assets/author-content-publish/rich-text-populated-component.png)

   此 **文字** 元件現在應會在頁面上顯示rtf。

1. 重複上述步驟，除了拖曳 **影像(v2)** 元件。 開啟 **影像** 元件的對話框。

1. 在左側邊欄中，切換至 **資產尋找器** 按一下 **資產** 圖示 ![資產圖示](assets/author-content-publish/asset-icon.png).
1. **拖放** 將影像放入元件的對話方塊中，然後按一下 **完成** 以儲存變更。

   ![將資產新增至對話方塊](assets/author-content-publish/add-asset-dialog.png)

1. 請注意，頁面上有元件，例如 **標題**, **導覽**, **搜尋** 已修正。 這些區域是「頁面範本」的一部分，無法在個別頁面上修改。 下一章將詳細探討此內容。

您可以嘗試其他部分元件。 關於每個 [您可以在此處找到核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 關於 [您可以在此處找到頁面編寫](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 發佈更新 {#publish-updates}

AEM環境會分割為 **作者服務** 和 **發佈服務**. 在本章中，我們已對上的網站進行數項修改 **作者服務**. 為了讓網站訪客檢視將變更發佈至 **發佈服務**.

![高階圖](assets/author-content-publish/author-publish-high-level-flow.png)

*從製作到發佈的高階內容流程*

**1.** 內容作者會更新網站內容。 可預覽、審核和核准更新，以便上線推播。

**2.** 內容已發佈。 可依需要執行發佈，或排程日期。

**3.** 網站訪客會看到發佈服務上反映的變更。

### 發佈變更

接下來，我們發佈變更。

1. 從AEM開始畫面導覽至 **網站** ，然後選取 **WKND站點**.
1. 按一下 **管理出版物** 的下限。

   ![管理發佈](assets/author-content-publish/click-manage-publiciation.png)

   由於這是一個全新的網站，因此我們要發佈所有頁面，並且可以使用「管理出版物」精靈來確切定義需要發佈的內容。

1. 在 **選項** 將預設設定保留為 **發佈** 並排程 **現在**. 按一下&#x200B;**下一步**。
1. 在 **範圍**，請選取 **WKND站點** 按一下 **包括子項設定**. 在對話方塊中，核取 **包括子項**. 取消勾選其餘的方塊，以確保發佈整個網站。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下 **發佈的參考** 按鈕。 在對話方塊中，確認已勾選所有項目。 這包括 **標準網站範本** 和「網站範本」產生的數個設定。 按一下 **完成** 以更新。

   ![發佈參考](assets/author-content-publish/publish-references.png)

1. 最後，勾選旁邊的方塊 **WKND站點** 按一下 **下一個** 在右上角。
1. 在 **工作流程** 步驟，輸入 **工作流程標題**. 這可以是任何文字，日後在稽核軌跡中會很實用。 輸入「初始發佈」，然後按一下 **發佈**.

![工作流程步驟初始發佈](assets/author-content-publish/workflow-step-publish.png)

## 檢視已發佈的內容 {#publish}

接著，導覽至發佈服務以檢視變更。

1. 若要取得發佈服務的URL，可以復製作者URL並取代 `author` word `publish`. 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 新增 `/content/wknd.html` 至發佈URL，使最終URL看起來類似： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 變更 `wknd.html` 若要比對網站名稱，在 [網站建立](create-site.md).

1. 導覽至「發佈URL」後，您應該會看到網站，而沒有任何AEM編寫功能。

   ![已發佈網站](assets/author-content-publish/publish-url-update.png)

1. 使用 **導覽** 功能表 **文章** > **《你好世界》** 導覽至先前建立的「您好世界」頁面。
1. 返回 **AEM Author Service** 並在頁面編輯器中進行其他內容變更。
1. 按一下 **頁面屬性** 圖示> **發佈頁面**

   ![發佈直接](assets/author-content-publish/page-editor-publish.png)

1. 返回 **AEM Publish Service** 來檢視變更。 很可能 **not** 立即查看更新。 這是因為 **AEM Publish Service** 包括 [透過Apache Web伺服器和CDN進行快取](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). 依預設，快取HTML內容約5分鐘。

1. 若要略過快取以用於測試/除錯用途，只需新增查詢參數，如 `?nocache=true`. URL看起來會像 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 有關可用快取策略和配置的詳細資訊 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. 您也可以在Cloud Manager中找到發佈服務的URL。 導覽至 **Cloud Manager計畫** > **環境** > **環境**.

   ![檢視發佈服務](assets/author-content-publish/view-environment-segments.png)

   在 **環境區段** 您可以找到連結 **作者** 和 **發佈** 服務。

## 恭喜！ {#congratulations}

恭喜，您剛剛撰寫並發佈了AEM網站的變更！

### 後續步驟 {#next-steps}

在實際實作中，規劃網站時通常會先建立模型和UI設計， 了解如何使用Adobe XD UI Kit來設計和加速Adobe Experience Manager Sites實作，於 [使用Adobe XD規劃UI](./ui-planning-adobe-xd.md).

想繼續探索AEM Sites功能嗎？ 隨時跳進 [頁面範本](./page-templates.md) 了解頁面範本和頁面之間的關係。


