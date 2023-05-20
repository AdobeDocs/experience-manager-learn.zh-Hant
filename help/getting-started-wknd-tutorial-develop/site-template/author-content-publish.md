---
title: 創作和發佈簡介 |快AEM速站點建立
description: 使用Adobe Experience Manager的頁面編AEM輯器更新網站的內容。 瞭解如何使用元件來方便創作。 瞭解AEM作者與發佈環境之間的區別，並瞭解如何將更改發佈到即時網站。
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 2%

---

# 創作和發佈簡介 {#author-content-publish}

瞭解用戶將如何更新網站的內容非常重要。 在本章中，我們將採用 **內容作者** 對上一章中生成的站點進行編輯更新。 在本章的末尾，我們將發佈這些更改以瞭解即時網站的更新方式。

## 必備條件 {#prerequisites}

這是一個多部分教程，並假定在 [建立站點](./create-site.md) 一章已經完成。

## 目標 {#objective}

1. 瞭解 **頁面** 和 **元件** 在AEM Sites。
1. 瞭解如何更新網站的內容。
1. 瞭解如何將更改發佈到即時網站。

## 建立新頁面 {#create-page}

通常，網站會被分成多個頁面，形成多頁體驗。 結AEM構內容相同。 接下來，為站點建立新頁。

1. 登錄到AEM **作者** 上一章中使用的服務。
1. 在「Start(開AEM始)」螢幕中按一下 **站點** > **WKND站點** > **英語** > **文章**
1. 在右上角按一下 **建立** > **頁面**。

   ![建立頁面](assets/author-content-publish/create-page-button.png)

   這將提出 **建立頁** 的子菜單。

1. 選擇 **文章頁** 按一下 **下一個**。

   中的頁AEM面基於頁面模板建立。 在中詳細探討了頁面模板 [頁面模板](page-templates.md) 一章。

1. 下 **屬性** 輸入 **標題** &quot;你好世界&quot;
1. 設定 **名稱** 要 `hello-world` 按一下 **建立**。

   ![初始頁屬性](assets/author-content-publish/initial-page-properties.png)

1. 在對話框彈出式窗口中按一下 **開啟** 開啟新建立的頁面。

## 建立元件 {#author-component}

組AEM件可以視為網頁的小模組構建塊。 通過將UI分為邏輯塊或元件，可使管理更輕鬆。 為了重新使用元件，元件必須是可配置的。 這是通過作者對話框完成的。

AEM提供 [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 生產準備使用。 的 **核心元件** 範圍，如 [文本](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 和 [影像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 到更複雜的UI元素，如 [旋轉木馬](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)。

接下來，使用「頁面編輯器」編寫AEM一些元件。

1. 導航到 **《你好世界》** 的子菜單。
1. 確保您在 **編輯** 模式，在左側滑軌中按一下 **元件** 表徵圖

   ![頁面編輯器側欄](assets/author-content-publish/page-editor-siderail.png)

   這將開啟「元件」庫，並列出可在頁面上使用的可用元件。

1. 向下滾動並 **拖放** a **文本(v2)** 上的元件。

   ![拖放文本元件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 按一下 **文本** 要加亮的元件，然後按一下 **扳** 表徵圖 ![扳手錶徵圖](assets/author-content-publish/wrench-icon.png) 開啟「元件」對話框。 輸入一些文本並將更改保存到對話框。

   ![RTF元件](assets/author-content-publish/rich-text-populated-component.png)

   的 **文本** 元件現在應在頁面上顯示富格文本。

1. 重複上述步驟，但拖動 **影像(v2)** 上的元件。 開啟 **影像** 對話框。

1. 在左滑軌中，切換到 **資產查找器** 按一下 **資產** 表徵圖 ![資產表徵圖](assets/author-content-publish/asset-icon.png)。
1. **拖放** 將影像放入「元件」對話框，然後按一下 **完成** 的子菜單。

   ![將資產添加到對話框](assets/author-content-publish/add-asset-dialog.png)

1. 請注意頁面上有元件，如 **標題**。 **導航**。 **搜索** 已修復。 這些區域配置為頁面模板的一部分，無法在單個頁面上進行修改。 在下一章中將對此進行更多探討。

您可以自由地嘗試其它部分。 關於每個 [可在此處找到核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。 詳細視頻系列關於 [可在此處找到頁面創作](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)。

## 發佈更新 {#publish-updates}

環AEM境在 **作者服務** 和 **發佈服務**。 在本章中，我們對上面的站點進行了幾次修改 **作者服務**。 為了讓站點訪問者查看所做更改，我們需要將更改發佈到 **發佈服務**。

![高級圖](assets/author-content-publish/author-publish-high-level-flow.png)

*從作者到發佈的高級內容流*

**1.** 內容作者對網站內容進行更新。 可以預覽、查看和批准更新以即時推送。

**2.** 內容已發佈。 可以按需執行發佈，也可以計畫將來的日期。

**3.** 站點訪問者將看到「發佈」服務上反映的更改。

### 發佈更改

接下來，我們發佈更改。

1. 從「開始AEM」螢幕導航到 **站點** 的 **WKND站點**。
1. 按一下 **管理發布** 的子菜單。

   ![管理出版物](assets/author-content-publish/click-manage-publiciation.png)

   由於此網站是全新的網站，因此我們希望發佈所有頁面，並可以使用「管理發布」嚮導精確定義需要發佈的內容。

1. 下 **選項** 將預設設定保留為 **發佈** 並安排 **現在**。 按一下&#x200B;**下一步**。
1. 下 **範圍**，選擇 **WKND站點** 按一下 **包括子項設定**。 在對話框中，選中 **包括子項**。 取消選中其餘框以確保發佈整個站點。

   ![更新發佈範圍](assets/author-content-publish/update-scope-publish.png)

1. 按一下 **已發佈引用** 按鈕 在對話框中，驗證是否檢查了所有內容。 這包括 **標準站點模板** 以及由站點模板生成的幾個配置。 按一下 **完成** 的子菜單。

   ![發佈引用](assets/author-content-publish/publish-references.png)

1. 最後，選中旁邊的框 **WKND站點** 按一下 **下一個** 在右上角。
1. 在 **工作流** 步驟，輸入 **工作流標題**。 這可以是任何文本，並且以後可以作為審計線索的一部分。 輸入「初始發佈」，然後按一下 **發佈**。

![工作流步驟初始發佈](assets/author-content-publish/workflow-step-publish.png)

## 查看已發佈的內容 {#publish}

接下來，導航到「發佈」服務以查看更改。

1. 獲取發佈服務URL的一種簡單方法是復製作者URL並替換 `author` 單詞 `publish`。 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **發佈URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 添加 `/content/wknd.html` 到「發佈」URL，以便最終URL如下所示： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 更改 `wknd.html` 匹配站點名稱(如果在 [站點建立](create-site.md)。

1. 導航到「發佈URL」，您應該看到該網站，而不需要任何創AEM作功能。

   ![已發佈站點](assets/author-content-publish/publish-url-update.png)

1. 使用 **導航** 菜單 **文章** > **《你好世界》** 導航到先前建立的「Hello World」頁。
1. 返回到 **AEM作者服務** 並在頁面編輯器中進行一些其他內容更改。
1. 通過按一下 **頁面屬性** 表徵圖 **發佈頁面**

   ![發佈直接](assets/author-content-publish/page-editor-publish.png)

1. 返回到 **AEM發佈服務** 的子菜單。 你很可能 **不** 立即查看更新。 這是因為 **AEM發佈服務** 包括 [通過Apache Web伺服器和CDN進行快取](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)。 預設情況下，HTML內容將快取約5分鐘。

1. 要繞過快取以進行測試/調試，只需添加查詢參數，如 `?nocache=true`。 該URL看起來 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。 有關快取策略和可用配置的詳細資訊 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您還可以在雲管理器中找到發佈服務的URL。 導航到 **雲管理器程式** > **環境** > **環境**。

   ![查看發佈服務](assets/author-content-publish/view-environment-segments.png)

   下 **環境段** 可以找到指向 **作者** 和 **發佈** 服務。

## 恭喜！ {#congratulations}

祝賀您，您剛剛撰寫並發佈了對網站的AEM更改！

### 後續步驟 {#next-steps}

在現實實施規劃中，通常在建立站點之前先使用帶有無形模組和UI設計的站點。 瞭解如何使用Adobe XDUI套件來設計和加速您的Adobe Experience Manager Sites實施 [UI規劃與Adobe XD](./ui-planning-adobe-xd.md)。

想繼續探索AEM Sites的能力嗎？ 你可以直接跳到 [頁面模板](./page-templates.md) 瞭解頁面模板和頁面之間的關係。


