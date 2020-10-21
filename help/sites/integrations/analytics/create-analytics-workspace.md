---
title: 使用分析工作區分析資料
description: 瞭解如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度。 瞭解如何使用Adobe Analytics的「分析工作區」功能建立詳細的報告控制面板。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 55beee99b91c44f96cd37d161bb3b4ffe38d2687
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# 使用分析工作區分析資料

瞭解如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度。 瞭解如何使用Adobe Analytics的「分析工作區」功能建立詳細的報告控制面板。

## 您將建立的

WKND行銷團隊想瞭解哪個「行動呼籲(CTA)」按鈕在首頁上表現最佳。 在本教學課程中，我們將在分析工作區中建立新專案，以視覺化方式呈現不同CTA按鈕的效能，並瞭解使用者在網站上的行為。 當使用者按一下WKND首頁上的「動作呼叫(CTA)」按鈕時，會使用Adobe Analytics擷取下列資訊。

**Analytics變數**

以下是目前追蹤的Analytics變數：

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA按一下Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目標 {#objective}

1. 建立新的報表套裝或使用現有的報表套裝。
1. 在報 [表套裝中設定轉](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) 換變數(eVar) [](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html) 和成功事件（事件）。
1. 建立分 [析工作區專案](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html) ，以利用工具分析資料，讓您快速建立、分析和分享見解。
1. 與其他團隊成員共用分析工作區專案。

## 必備條件

本教學課程是Adobe Analytics中 [Track點按元件的延續](./track-clicked-component.md) ，並假設您已：

* 啟 **用Adobe Analytics擴充功能** 的 [Launch屬性](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)
* **Adobe Analytics** test/dev報表套裝ID和追蹤伺服器。 請參閱下列檔案以 [建立新報表套裝](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。
* [Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) browser extension configured with your Launch property loaded on [https://wknd.site/us/en.html](https://wknd.site/us/en.html) or an AEM site with the Adobe Data Layer.

## 轉換變數(eVar)和成功事件（事件）

自訂分析轉換變數（或eVar）會放置在您網站所選網頁的Adobe程式碼中。 其主要用途是在自訂行銷報告中劃分轉換成功度量。 eVar可以是瀏覽型的，其功能與Cookie類似。 傳入eVar變數的值會跟隨使用者一段預定期間。

當eVar設為訪客的值時，Adobe會自動記住該值，直到其過期。 訪客在eVar值作用中時遇到的任何成功事件都會計入eVar值。

eVar最適合用來測量因果，例如：

* 哪些內部促銷活動影響收入
* 哪些橫幅廣告最終導致註冊
* 下訂單前使用內部搜尋的次數

成功事件是可追蹤的動作。 您決定什麼是成功事件。 例如，如果訪客點按CTA按鈕，點按事件可視為成功事件。

### 設定eVar

1. 從Adobe Experience Cloud首頁，選取您的組織並啟動Adobe Analytics。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. 在Analytics工具列中，按一下「管 **理** >報 **表套裝** 」並尋找您的報表套裝。

   ![Analytics報表套裝](assets/create-analytics-workspace/select-report-suite.png)

1. 選取報表套裝>編 **輯設定** > **轉換** >轉 **換變數**

   ![Analytics轉換變數](assets/create-analytics-workspace/conversion-variables.png)

1. 使用「 **新增** 」選項，我們建立轉換變數以映射結構，如下所示：

   * `eVar5` -  `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![新增eVar](assets/create-analytics-workspace/add-new-evars.png)

1. 為每個eVar提供適當的名稱和說明，並 **儲存您** 的變更。 我們將使用這些eVar在下一節中建立分析工作區專案。 因此，使用者易記的名稱可讓變數更容易被發現。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 設定成功事件

接下來，我們建立追蹤「CTA按鈕」點按的「偶數」。

1. 從「報 **表套裝管理器** 」視窗中，選取「報 **表套裝ID** 」並按一 **下「編輯設定**」。
1. 按一下 **轉換** >成 **功事件**
1. 使用「 **新增** 」選項，建立新的自訂成功事件以追蹤「CTA按鈕」按一下，然後 **** 儲存變更。
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## 在分析工作區中建立新專案 {#workspace-project}

分析工作區是彈性的瀏覽器工具，可讓您快速建立分析並分享見解。 使用拖放介面，您可以建立分析、新增視覺化，讓資料更生動、組織資料集、與組織中的任何人共用及排程專案。

接著，建立新 [專案](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html) ，以建立控制面板，分析整個網站的CTA按鈕效能。

1. 從Analytics工具列中，選取「工 **作區** 」，然後按 **一下「建立新專案」**。

   ![工作區](assets/create-analytics-workspace/create-workspace.png)

1. 選擇從空白的專 **案開始** ，或選取其中一個預先建立的範本（由Adobe提供或由您的組織建立的自訂範本）。 有數個範本可用，視您所考慮的分析或使用案例而定。 [進一步瞭解](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) 「可用的不同範本」選項。

   在您的工作區專案中，面板、表格、視覺化和元件可從左側導軌存取。 這些是您的專案構建區塊。

   * **[元件](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** -元件是維度、量度、區段或日期範圍，所有這些都可結合在自由表格中，以開始回答您的業務問題。 請務必先熟悉每種元件類型，然後再深入分析。 掌握元件術語後，您就可以開始拖放，在自由表格中建立分析。
   * **[視覺化](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** -視覺化（例如長條圖或折線圖）會加入資料之上，以視覺化方式呈現。 在最左側的邊欄上，選取中間的「視覺化」圖示，以檢視可用視覺化的完整清單。
   * **[面板](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)** -面板是表格和視覺化的集合。 您可以從工作區的左上角圖示存取面板。 當您想要根據時段、報表套裝或分析使用案例來組織專案時，面板會很有幫助。 「分析工作區」中提供下列面板類型：

   ![範本選擇](assets/create-analytics-workspace/workspace-tools.png)

### 使用分析工作區新增資料視覺化

接著，建立表格，以建立使用者如何與WKND網站首頁上的「動作呼叫」(CTA)按鈕互動的視覺表示。 若要建立此類表示法，讓我們使用Adobe Analytics在「追蹤」點按 [元件中收集到的資料](./track-clicked-component.md)。 以下是使用者與WKND網站的「動作呼叫」按鈕互動所追蹤的資料快速摘要。

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. 將「頁面」維 **度元件拖放** 至「自由表格」。 您現在應該可以檢視顯示表格中顯示頁面名稱(eVar9)和對應的頁面檢視（發生次數）的視覺化。

   ![頁面維度](assets/create-analytics-workspace/evar9-dimension.png)

1. 將 **CTA Click** (event8)量度拖放至發生次數量度並加以取代。 您現在可以檢視顯示頁面名稱(eVar9)的視覺化，以及頁面上的CTA點按事件對應計數。

   ![頁面量度- CTA點按](assets/create-analytics-workspace/evar8-cta-click.png)

1. 讓我們依其範本類型來劃分依頁面。 從元件中選取頁面範本量度，並將「頁面範本」量度拖放至「頁面名稱」維度。 您現在可以檢視依範本類型劃分的頁面名稱。

   * **變更前**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **變更後**

      ![eVar5量度](assets/create-analytics-workspace/evar5-metrics.png)

1. 若要瞭解使用者在WKND網站頁面上與CTA按鈕互動的方式，我們需要新增按鈕ID(eVar8)量度，進一步劃分頁面範本量度。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 在下方，您可看到WKND網站的視覺呈現方式，依其頁面範本細分，並進一步依使用者與WKND網站點按動作(CTA)按鈕的互動來細分。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. 您可以使用「Adobe Analytics分類」，將「按鈕ID」值取代為更易用的名稱。 您可在此處閱讀有關如何建立特定量度之分類的更多 [資訊](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html)。 在這種情況下，我們會針對按鈕ID `Button Section (Button ID)` 對應至 `eVar8` 使用者易記名稱的分類量度設定。

   ![按鈕區段](assets/create-analytics-workspace/button-section.png)

## 新增分類至分析變數

### 轉換分類

Analytics分類是將Analytics變數資料分類，然後在您產生報表時以不同方式顯示資料的方式。 為改善按鈕ID在「Analytics工作區」報表中的顯示方式，讓我們為按鈕ID(eVar8)建立分類變數。 分類時，您會在變數和與該變數相關的中繼資料之間建立關係。

接下來，我們建立Analytics變數的分類。

1. 從「管理」工 **具列** ，選取「報 **表套裝」**
1. 從「報表套 **裝管理員** 」視窗中選取報表套裝Id **，然後按一下「** 編輯設定 **>轉換** >轉換 ******分類」**

   ![轉換分類](assets/create-analytics-workspace/conversion-classification.png)

1. 從「選 **取分類類型** 」下拉式清單中，選取變數(eVar8-Button ID)以新增分類。
1. 按一下「分類」區段下方所列「分類」變數旁的箭頭，以新增「分類」。

   ![轉換分類類型](assets/create-analytics-workspace/select-classification-variable.png)

1. 在「編 **輯分類」對話方塊中** ，為「文字分類」提供適當的名稱。 建立具有「文本分類」名稱的維元件。

   ![轉換分類類型](assets/create-analytics-workspace/new-classification.png)

1. **儲存您的變更** 。

### 分類匯入工具

使用匯入工具將分類上傳至Adobe Analytics。 您也可以匯出資料，以便在匯入前進行更新。 您使用匯入工具匯入的資料必須是特定格式。 Adobe提供您下載資料範本的選項，其中包含Tab分隔資料檔案中所有正確的標題詳細資訊。 您可以將新資料新增至此範本，然後使用FTP在瀏覽器中匯入資料檔案。

#### 分類範本

在將分類匯入行銷報告之前，您可以下載範本，幫助您建立分類資料檔案。 資料檔案會將您想要的分類用作欄標題，然後將報告資料集組織在適當的分類標題下。

接下來，讓我們下載按鈕ID(eVar8)變數的分類範本

1. 導覽至「管 **理員** >分類匯 **入工具」**
1. 讓我們從「下載範本」標籤下載轉換變數的 **分類範本** 。
   ![轉換分類類型](assets/create-analytics-workspace/classification-importer.png)

1. 在「下載範本」標籤上，指定資料範本設定。
   * **選擇報表套裝** :選取要在範本中使用的報表套裝。 報表套裝和資料集必須相符。
   * **要分類的資料集** :選擇資料檔案的資料類型。 功能表包含您報表套裝中所有已設定分類的報表。
   * **編碼** :選擇資料檔案的字元編碼。 預設編碼格式為UTF-8。

1. 按一 **下「下載** 」，並將範本檔案儲存至本機系統。 範本檔案是以Tab分隔的資料檔案（.tab副檔名），大部分的試算表應用程式都支援。
1. 使用您選擇的編輯器開啟以Tab分隔的資料檔案。
1. 在區段的步驟9中，為每個eVar9值新增按鈕ID(eVar9)和對應的按鈕名稱至以Tab分隔的檔案。

   ![關鍵值](assets/create-analytics-workspace/key-value.png)

1. **儲存** Tab分隔的檔案。
1. 導覽至「匯 **入檔案** 」標籤。
1. 設定檔案匯入的目標。
   * **選擇報表套裝** :WKND網站AEM（報表套裝）
   * **要分類的資料集** :按鈕Id（轉換變數eVar8）
1. 按一下「選 **擇檔案** 」選項，從系統上傳以Tab分隔的檔案，然後按一下「匯 **入檔案」**

   ![檔案匯入工具](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 成功的匯入會立即顯示匯出中的適當變更。 不過，使用瀏覽器匯入時，報表中的資料變更需要4小時，使用FTP匯入時則需要24小時。

#### 以分類變數取代轉換變數

1. 從Analytics工具列中，選取「工 **作區** 」，並開啟本教學課程「在分析工作區 [](#workspace-project) 中建立新專案」區段中建立的工作區。

   ![工作區按鈕ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 接著，將工作區 **中顯示「動作呼叫(CTA)」按鈕ID的「按鈕Id** 」量度取代為上一步驟中建立的分類名稱。

1. 從元件搜尋器中，搜尋 **WKND CTA按鈕** ，並將 **** WKND CTA按鈕（按鈕Id）維度拖放至「按鈕Id」量度上並加以取代。

   * **變更前**

      ![工作區按鈕之前](assets/create-analytics-workspace/wknd-button-before.png)
   * **變更後**

      ![工作區按鈕之後](assets/create-analytics-workspace/wknd-button-after.png)

1. 您可以注意到「按鈕ID」量度包含「動作呼叫(CTA)」按鈕的按鈕ID，現在會以「分類範本」中提供的對應名稱取代。
1. 讓我們比較Analytics工作區表格與WKND首頁，並瞭解CTA按鈕點按計數及其分析。 根據工作區自由表格資料，顯然使用者點按 **SKI NOW** （現在滑雪）按鈕的次數是22次，而在西澳洲的WKND Home Page Camping（WKND首頁露營）按鈕的次數是4 **次** 。

   ![CTA報表](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. 請務必儲存您的Adobe Analytics工作區專案，並提供正確的名稱和說明。 （可選）您可以將標籤添加到工作區項目。

   ![儲存專案](assets/create-analytics-workspace/save-project.png)

1. 在成功儲存專案後，您可以使用「共用」選項與其他同事或隊友共用您的工作區專案。

   ![共用專案](assets/create-analytics-workspace/share.png)

## 恭喜！

您剛學過如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度、對量度執行分類，並使用Adobe Analytics的分析工作區功能建立詳細的報表控制面板。

