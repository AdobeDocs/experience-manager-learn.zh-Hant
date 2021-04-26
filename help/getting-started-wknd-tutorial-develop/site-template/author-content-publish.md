---
title: 製作內容並發佈變更
seo-title: 開始使用AEM Sites-製作內容並發佈變更
description: 使用Adobe Experience Manager的「頁面」編AEM輯器更新網站內容。 瞭解如何使用元件來協助製作。 瞭解AEM Author和Publish環境之間的差異，並瞭解如何將變更發佈至即時網站。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件，頁面編輯器
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 2%

---


# 製作內容並發佈變更{#author-content-publish}

>[!CAUTION]
>
> 此處展示的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽使用。

請務必瞭解使用者將如何更新網站的內容。 在本章中，我們將採用&#x200B;**內容作者**&#x200B;的角色，並對上一章中產生的網站進行一些編輯更新。 在本章結束時，我們將發佈變更，以瞭解即時網站的更新方式。

## 必備條件 {#prerequisites}

本教學課程包含多部分，假設[建立網站](./create-site.md)章節中概述的步驟已完成。

## 目標 {#objective}

1. 瞭解AEM Sites的&#x200B;**Pages**&#x200B;和&#x200B;**Components**&#x200B;概念。
1. 瞭解如何更新網站內容。
1. 瞭解如何將變更發佈至即時網站。

## 建立新頁面{#create-page}

網站通常會分成多個頁面，以形成多頁體驗。 以相AEM同的方式建構內容。 接著，為網站建立新頁面。

1. 登入前AEM一章中使用的&#x200B;**Author**&#x200B;服務。
1. 從「AEM開始」畫面按一下「**網站** > **WKND網站** > **英文** > **文章**」
1. 在右上角按一下「建立&#x200B;**>** > **頁面**」。

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這將啟動&#x200B;**建立頁面**&#x200B;精靈。

1. 選擇「**文章頁面**」範本，然後按一下「**Next**」。

   中的頁AEM面是根據頁面範本建立的。 將在[頁面範本](page-templates.md)章節中更詳細地探討頁面範本。

1. 在&#x200B;**Properties**&#x200B;下輸入&#x200B;**Title** of &quot;Hello World&quot;。
1. 將&#x200B;**名稱**&#x200B;設為`hello-world`，然後按一下「建立&#x200B;**」。**

   ![初始頁面屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話方塊快顯視窗中，按一下「開啟&#x200B;****」以開啟新建立的頁面。

## 編寫元件{#author-component}

元AEM件可視為網頁的小模組化建置區塊。 將UI分割為邏輯區塊或元件，讓管理更輕鬆。 若要重新使用元件，元件必須是可設定的。 這是透過作者對話方塊完成的。

提AEM供生產就緒的[核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)集。 **核心元件**&#x200B;從諸如[Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)和[Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)等基本元素到更複雜的UI元素，如[Carousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)。

接下來，讓我們使用頁面編輯器來製作AEM一些元件。

1. 導覽至上一練習中建立的&#x200B;**Hello World**&#x200B;頁面。
1. 請確定您處於&#x200B;**編輯**&#x200B;模式，並在左側導軌中按一下&#x200B;**元件**&#x200B;表徵圖。

   ![頁面編輯器側邊欄](assets/author-content-publish/page-editor-siderail.png)

   這將開啟「元件」庫，並列出可在頁面上使用的可用元件。

1. 向下捲動並&#x200B;**Drag+Drop** a **Text(v2)**&#x200B;元件至頁面的主要可編輯區域。

   ![拖放文字元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下&#x200B;**Text**&#x200B;元件以反白標示，然後按一下&#x200B;**wrench**&#x200B;圖示![Wrench圖示](assets/author-content-publish/wrench-icon.png)以開啟「元件」的對話方塊。 輸入一些文本並保存對話框的更改。

   ![Rich Text元件](assets/author-content-publish/rich-text-populated-component.png)

   **Text**&#x200B;元件現在應會在頁面上顯示Rich Text。

1. 重複上述步驟，但將&#x200B;**Image(v2)**&#x200B;元件實例拖動到頁面中除外。 開啟&#x200B;**Image**&#x200B;元件的對話框。

1. 在左側導軌中，按一下「資產」圖示![資產圖示](assets/author-content-publish/asset-icon.png)，以切換至「資產搜尋器」。********
1. **將+** Dropan影像拖動到「元件」對話框中，然後按一下「 **** 完成」保存更改。

   ![新增資產至對話方塊](assets/author-content-publish/add-asset-dialog.png)

1. 請注意，頁面上有固定的元件，例如&#x200B;**Title**、**Navigation**、**Search**。 這些區域是設定為「頁面範本」的一部分，無法在個別頁面上修改。 下一章將詳細探討此問題。

您可以自由嘗試其他元件。 有關每個[核心元件的文檔可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。 有關[頁面編寫的詳細影片系列，請參閱](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)。

## 發佈更新{#publish-updates}

環境AEM分割為&#x200B;**作者服務**&#x200B;和&#x200B;**發佈服務**。 在本章中，我們對&#x200B;**作者服務**&#x200B;上的網站做了幾項修改。 為了讓網站訪客檢視所需的變更，我們必須將其發佈至&#x200B;**Publish Service**。

![高級圖](assets/author-content-publish/author-publish-high-level-flow.png)

*從「作者」到「發佈」的高階內容流程*

**1.** 內容作者會更新網站內容。您可預覽、審閱和核准更新，以便即時推播。

**2.** 內容會發佈。出版品可隨選執行或排程在未來日期。

**3.** 網站訪客將會看到Publish服務所反映的變更。

### 發佈變更

接下來，讓我們發佈變更。

1. 從「開AEM始」螢幕導航至&#x200B;**Sites**&#x200B;並選擇&#x200B;**WKND Site**。
1. 按一下菜單欄中的&#x200B;**管理出版物**。

   ![管理發佈](assets/author-content-publish/click-manage-publiciation.png)

   由於這是全新的網站，因此我們想要發佈所有頁面，並可使用「管理出版物」精靈來精確定義需要發佈的內容。

1. 在&#x200B;**Options**&#x200B;下，將預設設定保留為&#x200B;**Publish**，並排程&#x200B;**Now**。 按一下&#x200B;**下一步**。
1. 在&#x200B;**Scope**&#x200B;下，選擇&#x200B;**WKND站點** ，然後按一下&#x200B;**Include Children**。 在對話方塊中，取消勾選所有方塊。 我們想要發佈完整網站。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下&#x200B;**發佈引用**&#x200B;按鈕。 在對話方塊中，確認所有項目皆已勾選。 這將包括&#x200B;**基本站AEM點模板**&#x200B;和由站點模板生成的幾個配置。 按一下&#x200B;**Done**&#x200B;進行更新。

   ![發佈參考](assets/author-content-publish/publish-references.png)

1. 最後，按一下右上角的&#x200B;**Publish**&#x200B;以發佈內容。

## 檢視已發佈的內容{#publish}

接著，導覽至「發佈」服務以檢視變更。

1. 要取得Publish Service的URL，最簡單的方式是複製Author url，並將`author`字詞取代為`publish`。 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL** -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 將`/content/wknd.html`新增至「發佈URL」，讓最終URL看起來像：`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 如果您在[網站建立](create-site.md)期間提供唯一名稱，請變更`wknd.html`以符合您的網站名稱。

1. 導覽至您應該看到網站的「發佈URL」，而不需具備任何制AEM作功能。

   ![發佈網站](assets/author-content-publish/publish-url-update.png)

1. 使用&#x200B;**Navigation**&#x200B;功能表，按一下「文章&#x200B;**>** Hello World **」以導覽至先前建立的「Hello World」頁面。**
1. 返回&#x200B;**AEM Author Service**，並在頁面編輯器中進行其他內容變更。
1. 按一下「頁面屬性&#x200B;**」圖示>**「發佈頁面&#x200B;**」，直接在頁面編輯器中發佈這些變更**

   ![發佈直接](assets/author-content-publish/page-editor-publish.png)

1. 返回&#x200B;**AEM Publish Service**&#x200B;以檢視變更。 您很可能會立即看到&#x200B;**not**&#x200B;的更新。 這是因為&#x200B;**AEM Publish Service**&#x200B;包含透過Apache網頁伺服器和CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)進行的[快取。 依預設，HTML內容會快取約5分鐘。

1. 若要略過快取以進行測試／除錯，只需新增查詢參數，例如`?nocache=true`。 URL看起來會像`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。 有關[可用快取策略和配置的詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您也可以在Cloud Manager中找到發佈服務的URL。 導覽至「**Cloud Manager Program** > **環境** > **環境**」。

   ![檢視發佈服務](assets/author-content-publish/view-environment-segments.png)

   在&#x200B;**環境區段**&#x200B;下，您可以找到指向&#x200B;**作者**&#x200B;和&#x200B;**發佈**&#x200B;服務的連結。

## 恭喜！{#congratulations}

恭喜您，您剛撰寫並發佈網站的變AEM更！

### 後續步驟{#next-steps}

瞭解如何建立和修改[頁面範本](./page-templates.md)。 瞭解頁面範本與頁面之間的關係。 瞭解如何設定頁面範本的原則，為內容提供精細的治理和品牌一致性。  將會根據Adobe XD的模型，建立結構精良的雜誌文章範本。
