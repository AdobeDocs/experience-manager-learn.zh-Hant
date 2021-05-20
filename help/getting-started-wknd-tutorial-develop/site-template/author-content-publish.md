---
title: 製作內容和發佈變更
seo-title: AEM Sites快速入門 — 製作內容與發佈變更
description: 使用AEMAdobe Experience Manager中的頁面編輯器來更新網站的內容。 了解如何使用元件來加速編寫。 了解AEM製作和發佈環境之間的差異，並了解如何將變更發佈至即時網站。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件、頁面編輯器
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 2%

---


# 製作內容和發佈變更{#author-content-publish}

>[!CAUTION]
>
> 此處的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽。

請務必了解使用者如何更新網站的內容。 在本章中，我們將採用&#x200B;**內容作者**&#x200B;的角色，並對上一章中產生的網站進行一些編輯更新。 在章節結尾，我們將發佈變更，以了解即時網站的更新方式。

## 必備條件 {#prerequisites}

本教學課程分為多部分，假設[建立網站](./create-site.md)章節中概述的步驟已完成。

## 目標 {#objective}

1. 了解AEM Sites中&#x200B;**Pages**&#x200B;和&#x200B;**Components**&#x200B;的概念。
1. 了解如何更新網站的內容。
1. 了解如何將變更發佈至上線網站。

## 建立新頁面{#create-page}

網站通常會分割成頁面，以形成多頁體驗。 AEM以相同方式建構內容。 接下來，為網站建立新頁面。

1. 登入前一章所使用的AEM **Author**&#x200B;服務。
1. 在AEM「開始」螢幕中，按一下「**Sites** > **WKND Site** > **English** > **Article**」
1. 在右上角按一下「**Create** > **Page**」。

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這會顯示&#x200B;**建立頁面**&#x200B;精靈。

1. 選擇&#x200B;**文章頁面**&#x200B;範本，然後按一下&#x200B;**下一步**。

   AEM中的頁面是根據頁面範本建立。 將在[頁面範本](page-templates.md)章節中更詳細地探索頁面範本。

1. 在「**屬性**」下，輸入「Hello World」的&#x200B;**Title**。
1. 將&#x200B;**Name**&#x200B;設定為`hello-world`，然後按一下&#x200B;**Create**。

   ![初始頁面屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話方塊快顯視窗中，按一下&#x200B;**開啟**&#x200B;以開啟新建立的頁面。

## 編寫元件{#author-component}

AEM元件可視為網頁的小型模組化建置組塊。 將UI分割為邏輯區塊或元件，可讓管理更輕鬆。 要重新使用元件，元件必須是可配置的。 這可透過製作對話方塊完成。

AEM提供一組可供生產使用的[核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。 **核心元件**&#x200B;從[Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)和[Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)等基本元素，到更複雜的UI元素，如[Carousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)。

接下來，我們使用AEM頁面編輯器來編寫一些元件。

1. 導覽至上次練習中建立的&#x200B;**Hello World**&#x200B;頁面。
1. 請確定您處於&#x200B;**Edit**&#x200B;模式，並在左側邊欄中按一下&#x200B;**Components**&#x200B;圖示。

   ![頁面編輯器側邊欄](assets/author-content-publish/page-editor-siderail.png)

   這會開啟元件程式庫，並列出可在頁面上使用的可用元件。

1. 向下捲動並&#x200B;**拖放**&#x200B;上的&#x200B;**文字(v2)**&#x200B;元件至頁面的主要可編輯區域。

   ![拖曳+拖放文字元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下&#x200B;**Text**&#x200B;元件以醒目提示，然後按一下&#x200B;**扳手**&#x200B;圖示![扳手圖示](assets/author-content-publish/wrench-icon.png)以開啟元件的對話方塊。 輸入一些文字並將更改保存到對話框。

   ![RTF元件](assets/author-content-publish/rich-text-populated-component.png)

   **Text**&#x200B;元件現在應會在頁面上顯示RTF。

1. 重複上述步驟，除了將&#x200B;**Image(v2)**&#x200B;元件的例項拖曳到頁面上。 開啟&#x200B;**Image**&#x200B;元件的對話框。

1. 在左側邊欄中，按一下&#x200B;**Assets**&#x200B;圖示![資產圖示](assets/author-content-publish/asset-icon.png)，切換至&#x200B;**資產尋找器**。
1. **將+拖** 放影像拖曳至元件的對話方塊，然後按一 **** 下「完成」以儲存變更。

   ![將資產新增至對話方塊](assets/author-content-publish/add-asset-dialog.png)

1. 請注意，頁面上有固定的元件，例如&#x200B;**Title**、**Navigation**、**Search**。 這些區域是「頁面範本」的一部分，無法在個別頁面上修改。 下一章將詳細探討此內容。

您可以嘗試其他部分元件。 若需每個[核心元件的相關檔案，請前往此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。 若需[頁面製作的詳細影片系列，請前往此處](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)。

## 發佈更新{#publish-updates}

AEM環境會分割為&#x200B;**Author Service**&#x200B;和&#x200B;**Publish Service**。 在本章中，我們已對&#x200B;**作者服務**&#x200B;上的網站進行數項修改。 為了讓網站訪客檢視所需變更，我們需將變更發佈至&#x200B;**發佈服務**。

![高階圖](assets/author-content-publish/author-publish-high-level-flow.png)

*從製作到發佈的高階內容流程*

**1.** 內容作者會更新網站內容。可預覽、審核和核准更新，以便上線推播。

**2.** 內容已發佈。可依需要執行發佈，或排程日期。

**3.** 網站訪客會看到發佈服務上反映的變更。

### 發佈變更

接下來，我們發佈變更。

1. 從「AEM開始」螢幕導覽至&#x200B;**Sites**&#x200B;並選取&#x200B;**WKND Site**。
1. 按一下菜單欄中的&#x200B;**管理出版物**。

   ![管理發佈](assets/author-content-publish/click-manage-publiciation.png)

   由於這是一個全新的網站，因此我們要發佈所有頁面，並且可以使用「管理出版物」精靈來確切定義需要發佈的內容。

1. 在&#x200B;**Options**&#x200B;下，將預設設定保留為&#x200B;**Publish**，並為&#x200B;**Now**&#x200B;排程。 按一下&#x200B;**下一步**。
1. 在&#x200B;**範圍**&#x200B;下，選擇&#x200B;**WKND站點**，然後按一下&#x200B;**包含子項**。 在對話方塊中，取消勾選所有方塊。 我們要發佈完整網站。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下&#x200B;**Published References**&#x200B;按鈕。 在對話方塊中，確認已勾選所有項目。 這將包括&#x200B;**基本AEM網站範本**&#x200B;以及網站範本產生的數個設定。 按一下&#x200B;**Done**&#x200B;以更新。

   ![發佈參考](assets/author-content-publish/publish-references.png)

1. 最後，按一下右上角的&#x200B;**發佈**&#x200B;以發佈內容。

## 檢視已發佈的內容 {#publish}

接著，導覽至發佈服務以檢視變更。

1. 若要取得發佈服務的URL，可以復製作者URL，並將`author`字詞取代為`publish`。 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL**  -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 將`/content/wknd.html`新增至發佈URL，使最終URL看起來類似：`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 如果您在[網站建立](create-site.md)期間提供了唯一名稱，請變更`wknd.html`以符合網站名稱。

1. 導覽至「發佈URL」後，您應該會看到網站，而沒有任何AEM編寫功能。

   ![已發佈網站](assets/author-content-publish/publish-url-update.png)

1. 使用&#x200B;**Navigation**&#x200B;菜單，按一下&#x200B;**Article** > **Hello World**&#x200B;以導航到先前建立的Hello World頁。
1. 返回&#x200B;**AEM Author Service**，並在頁面編輯器中變更一些其他內容。
1. 按一下&#x200B;**頁面屬性**&#x200B;圖示> **發佈頁面**，直接從頁面編輯器發佈這些變更

   ![發佈直接](assets/author-content-publish/page-editor-publish.png)

1. 返回&#x200B;**AEM Publish Service**&#x200B;以檢視變更。 很可能您會立即看到&#x200B;**not**&#x200B;更新。 這是因為&#x200B;**AEM Publish Service**&#x200B;包含透過Apache Web伺服器和CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)的[快取。 預設會快取約5分鐘的HTML內容。

1. 若要略過快取以用於測試/偵錯用途，只需新增查詢參數，如`?nocache=true`即可。 URL看起來會像`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。 有關[可用快取策略和配置的詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您也可以在Cloud Manager中找到發佈服務的URL。 導覽至「**Cloud Manager程式** > **環境** > **環境**」。

   ![檢視發佈服務](assets/author-content-publish/view-environment-segments.png)

   在&#x200B;**環境區段**&#x200B;下，您可以找到&#x200B;**Author**&#x200B;和&#x200B;**Publish**&#x200B;服務的連結。

## 恭喜！ {#congratulations}

恭喜，您剛剛撰寫並發佈了AEM網站的變更！

### 後續步驟{#next-steps}

了解如何建立和修改[頁面範本](./page-templates.md)。 了解頁面範本和頁面之間的關係。 了解如何設定頁面範本的原則，以提供精細的控管和內容的品牌一致性。  系統會根據Adobe XD的模型，建立結構良好的雜誌文章範本。
