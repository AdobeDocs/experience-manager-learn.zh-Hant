---
title: 使用Analysis Workspace分析資料
description: 瞭解如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度。 瞭解如何使用Adobe Analytics的Analysis Workspace功能建置詳細的報告控制面板。
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="整合" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 443
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 0%

---

# 使用Analysis Workspace分析資料

瞭解如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度。 瞭解如何使用Adobe Analytics的Analysis Workspace功能建置詳細的報告控制面板。

## 您即將建置的內容 {#what-build}

WKND行銷團隊想要瞭解哪些`Call to Action (CTA)`按鈕在首頁上表現最佳。 在本教學課程中，請在&#x200B;**Analysis Workspace**&#x200B;中建立專案，以視覺化不同CTA按鈕的效能，並瞭解網站上的使用者行為。 以下資訊是使用Adobe Analytics擷取，當使用者按一下WKND首頁上的行動號召(CTA)按鈕時。

**分析變數**

目前追蹤的Analytics變數如下：

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA按一下Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目標 {#objective}

1. 建立報表套裝或使用現有報表套裝。
1. 在報表套裝中設定[轉換變數(eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)和[成功事件（事件）](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)。
1. 建立[Analysis Workspace專案](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)，藉助可讓您快速建置、分析和分享見解的工具，分析資料。
1. 與其他團隊成員共用Analysis Workspace專案。

## 先決條件

本教學課程是[使用Adobe Analytics](./track-clicked-component.md)追蹤已點按元件的延續，並假設您有：

* 已啟用[Adobe Analytics擴充功能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html)的&#x200B;**標籤屬性**
* **Adobe Analytics**&#x200B;測試/開發報表套裝ID與追蹤伺服器。 請參閱下列有關[建立報表套裝](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html)的檔案。
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)瀏覽器擴充功能已設定在[WKND網站](https://wknd.site/us/en.html)或啟用Adobe資料層的AEM網站上載入標籤屬性。

## 轉換變數(eVar)和成功事件（事件）

Custom Insight轉換變數(或eVar)會放置在網站所選網頁的Adobe程式碼中。 其主要用途是在自訂行銷報告中區隔轉換成功量度。 eVar可以是以造訪為基礎，其功能與Cookie類似。 傳入eVar變數的值會跟隨使用者預定的一段時間。

當eVar設為訪客的值時，Adobe會自動記住該值，直到它過期為止。 eVar值作用中時，訪客遇到的任何成功事件都會計入eVar值。

eVar最適合用來測量原因和結果，例如：

* 哪些內部行銷活動影響了收入
* 最終導致註冊的橫幅廣告
* 訂單前使用內部搜尋的次數

成功事件是可追蹤的動作。 成功事件的條件由您決定。 例如，如果訪客按一下CTA按鈕，該點選事件可被視為成功事件。

### 設定eVar

1. 從Adobe Experience Cloud首頁，選取您的組織，然後啟動Adobe Analytics。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. 在Analytics工具列中按一下&#x200B;**管理員** > **報表套裝**，然後尋找您的報表套裝。

   ![Analytics報表套裝](assets/create-analytics-workspace/select-report-suite.png)

1. 選取報表套裝> **編輯設定** > **轉換** > **轉換變數**

   ![Analytics轉換變數](assets/create-analytics-workspace/conversion-variables.png)

1. 使用&#x200B;**新增**&#x200B;選項，讓我們建立轉換變數來對應結構描述，如下所示：

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![新增eVar](assets/create-analytics-workspace/add-new-evars.png)

1. 為每個eVar提供適當的名稱和說明，然後&#x200B;**儲存**&#x200B;您的變更。 在Analysis Workspace專案中，會使用適當名稱的eVar，因此好記的名稱可讓您輕鬆找到變數。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 設定成功事件

接下來，讓我們建立事件以追蹤CTA按鈕點選。

1. 從&#x200B;**報表套裝管理員**&#x200B;視窗中，選取&#x200B;**報表套裝ID**，然後按一下&#x200B;**編輯設定**。
1. 按一下&#x200B;**轉換** > **成功事件**
1. 使用&#x200B;**新增**&#x200B;選項，建立自訂成功事件以追蹤CTA按鈕點選，然後&#x200B;**儲存**&#x200B;您的變更。
   * `Event` ： `event8`
   * `Name`：`CTA Click`
   * `Type`：`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## 在Analysis Workspace中建立專案 {#workspace-project}

Analysis Workspace是彈性的瀏覽器工具，可讓您快速建立分析和分享見解。 您可以使用拖放式操作介面進行分析、新增視覺效果以生動呈現資料、組織資料集、與組織中的任何人共用及排程專案。

接下來，建立[專案](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace)以建置儀表板，分析整個網站中CTA按鈕的效能。

1. 從Analytics工具列中選取&#x200B;**Workspace**，然後按一下&#x200B;**建立新專案**。

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. 選擇從&#x200B;**空白專案**&#x200B;開始，或選取其中一個預先建立的範本(由Adobe提供，或由您的組織建立的自訂範本)。 您可使用數個範本，視您思考的分析或使用案例而定。 [深入瞭解](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html)可用的不同範本選項。

   在Workspace專案中，可從左側欄存取面板、表格、視覺效果和元件。 它們構成了專案的建置組塊。

   * **[元件](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** — 元件包括維度、量度、區段或日期範圍，您可以在自由表格中結合這些元件，開始回應您的業務問題。 請務必熟悉各種元件型別，再開始建立分析。 熟悉元件術語後，即可開始在自由表格中以拖放方式建立分析。
   * **[視覺效果](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** — 接著在資料上新增視覺效果（例如長條圖或折線圖），以視覺化方式生動呈現資料。 在左側邊欄中，選取中間的「視覺效果」圖示，即可檢視完整的可用視覺效果清單。
   * **[面板](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)** — 面板是表格與視覺效果的集合。 您可以從Workspace左上角圖示存取面板。 當您想要根據時段、報表套裝或分析使用案例來組織專案時，面板會很有幫助。 Analysis Workspace中有以下面板型別：

   ![範本選取專案](assets/create-analytics-workspace/workspace-tools.png)

### 使用Analysis Workspace新增資料視覺效果

接下來，建立表格以建立使用者如何與WKND網站首頁上的`Call to Action (CTA)`按鈕互動的視覺化表示法。 若要建置這類表示法，請使用Adobe Analytics](./track-clicked-component.md)在[追蹤已點按元件中收集的資料。 以下是使用WKND網站的呼叫動作按鈕針對使用者互動所追蹤的資料快速摘要。

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. 將&#x200B;**頁面**&#x200B;維度元件拖放至自由表格。 您現在應該能夠檢視視覺效果，以顯示表格內顯示的「頁面名稱」(eVar9)和「頁面檢視次數」（發生次數）。

   ![頁面Dimension](assets/create-analytics-workspace/evar9-dimension.png)

1. 將&#x200B;**CTA Click** (event8)量度拖放至「發生次數」量度上，並加以取代。 您現在可以檢視在頁面上顯示「頁面名稱」(eVar9)和CTA點選事件對應計數的視覺效果。

   ![頁面量度 — CTA點按](assets/create-analytics-workspace/evar8-cta-click.png)

1. 讓我們依照其範本型別來劃分頁面。 從元件選取頁面範本量度，並將「頁面範本」量度拖放至「頁面名稱」維度。 您現在可以檢視依範本型別劃分的頁面名稱。

   * **在**之前
     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **After**
     ![eVar5量度](assets/create-analytics-workspace/evar5-metrics.png)

1. 若要瞭解使用者在WKND網站頁面上與CTA按鈕互動的方式，需要新增按鈕ID (eVar8)量度來進一步劃分。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 在下方，您可以看到WKND網站的視覺化呈現方式，依其頁面範本劃分，並進一步依使用者與WKND網站點按以動作(CTA)按鈕的互動劃分。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. 您可以使用Adobe Analytics分類，以更好記的名稱取代「按鈕ID」值。 如需深入瞭解如何建立特定量度[的分類，請參閱此處](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html)。 在此案例中，我們為`eVar8`設定了分類量度`Button Section (Button ID)`，將按鈕ID對應至好記的名稱。

   ![按鈕區段](assets/create-analytics-workspace/button-section.png)

## 新增分類至分析變數

### 轉換分類

Analytics分類是將Analytics變數資料分類，然後在您產生報表時以不同方式顯示資料的方式。 若要改善按鈕ID在Analytics Workspace報表中的顯示方式，讓我們為按鈕ID (eVar8)建立分類變數。 分類時，您會建立變數與該變數相關的中繼資料之間的關係。

接下來，建立Analytics變數的分類。

1. 從&#x200B;**管理員**&#x200B;工具列功能表，選取&#x200B;**報表套裝**
1. 從&#x200B;**報表套裝管理員**&#x200B;視窗中選取&#x200B;**報表套裝Id**，然後按一下&#x200B;**編輯設定** > **轉換** > **轉換分類**

   ![轉換分類](assets/create-analytics-workspace/conversion-classification.png)

1. 從&#x200B;**選取分類型別**&#x200B;下拉式清單中，選取變數(eVar8 — 按鈕ID)以新增分類。
1. 按一下「分類」區段所列分類變數旁的箭頭，以新增分類。

   ![轉換分類型別](assets/create-analytics-workspace/select-classification-variable.png)

1. 在&#x200B;**編輯分類**&#x200B;對話方塊中，為文字分類提供適當的名稱。 會建立具有文字分類名稱的維度元件。

   ![轉換分類型別](assets/create-analytics-workspace/new-classification.png)

1. **儲存**&#x200B;您的變更。

### 分類匯入工具

使用匯入工具將分類上傳至Adobe Analytics。 您也可以在匯入之前，匯出資料以進行更新。 您使用匯入工具匯入的資料必須是特定格式。 Adobe提供您下載資料範本的選項，該範本包含以Tab分隔的資料檔案中所有適當的標題詳細資訊。 您可以將新資料新增至此範本，然後使用FTP在瀏覽器中匯入資料檔案。

#### 分類範本

將分類匯入行銷報表之前，您可以下載範本來協助您建立分類資料檔案。 資料檔案會使用您所需的分類當做欄標題，然後將報告資料集組織在適當的分類標題下。

接下來，讓我們下載Button Id (eVar8)變數的分類範本

1. 瀏覽至&#x200B;**管理員** > **分類匯入工具**
1. 讓我們從&#x200B;**下載範本**索引標籤下載轉換變數的分類範本。
   ![轉換分類型別](assets/create-analytics-workspace/classification-importer.png)

1. 在「下載範本」索引標籤中指定資料範本設定。
   * **選取報表套裝** ：選取要在範本中使用的報表套裝。 報表套裝和資料集必須相符。
   * **要分類的資料集** ：選取資料檔案的資料型別。 功能表含有報表套裝中所有已針對分類設定的報告。
   * **編碼** ：選取資料檔案的字元編碼。 預設編碼格式為UTF-8。

1. 按一下&#x200B;**下載**&#x200B;並將範本檔案儲存到您的本機系統。 範本檔案是以定位字元分隔的資料檔案（副檔名為.tab），大多數的試算表應用程式均能支援。
1. 使用您選擇的編輯器開啟以Tab分隔的資料檔案。
1. 針對區段中步驟9的每個eVar9值，將按鈕ID (eVar9)和對應的按鈕名稱新增至定位點分隔的檔案。

   ![索引鍵值](assets/create-analytics-workspace/key-value.png)

1. **儲存**&#x200B;以Tab分隔的檔案。
1. 瀏覽至&#x200B;**匯入檔案**&#x200B;標籤。
1. 設定檔案匯入的目的地。
   * **選取報表套裝** ： WKND網站AEM （報表套裝）
   * **要分類的資料集** ：按鈕Id (轉換變數eVar8)
1. 按一下&#x200B;**選擇檔案**&#x200B;選項，從您的系統上傳Tab分隔的檔案，然後按一下&#x200B;**匯入檔案**

   ![檔案匯入工具](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 成功的匯入會立即在匯出中顯示適當的變更。 不過，使用瀏覽器匯入時，報表中的資料變更需時長達四小時，使用FTP匯入時，則需時長達二十四小時。

#### 以分類變數取代轉換變數

1. 從Analytics工具列中選取&#x200B;**Workspace**，然後開啟在本教學課程的[在Analysis Workspace中建立專案](#create-a-project-in-analysis-workspace)區段中建立的工作區。

   ![Workspace按鈕ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 接下來，將工作區中顯示「行動號召」 (CTA)按鈕識別碼的&#x200B;**按鈕ID**&#x200B;量度，取代為先前步驟中建立的分類名稱。

1. 在元件尋找器中，搜尋&#x200B;**WKND CTA按鈕**&#x200B;並將&#x200B;**WKND CTA按鈕（按鈕ID）**&#x200B;維度拖放到Button ID量度上並加以取代。

   * **在**之前
     ![在](assets/create-analytics-workspace/wknd-button-before.png)之前的Workspace按鈕
   * **After**
     ](assets/create-analytics-workspace/wknd-button-after.png)之後的![Workspace按鈕

1. 您可以注意到，包含行動號召(CTA)按鈕的按鈕ID量度，現在已取代為分類範本中提供的對應名稱。
1. 讓我們比較Analytics Workspace表格與WKND首頁，瞭解CTA按鈕點選計數及其分析。 根據工作區自由表格資料，使用者已按下&#x200B;**SKI NOW**&#x200B;按鈕22次，西澳洲的WKND首頁露營&#x200B;**瞭解詳情**&#x200B;按鈕已按下4次。

   ![CTA報告](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. 請務必儲存Adobe Analytics Workspace專案，並提供適當的名稱和說明。 或者，您也可以將標籤新增至Workspace專案。

   ![儲存專案](assets/create-analytics-workspace/save-project.png)

1. 成功儲存專案後，您可以使用「共用」選項與其他同事或團隊成員共用您的工作區專案。

   ![共用專案](assets/create-analytics-workspace/share.png)

## 恭喜！

您剛剛瞭解如何將從Adobe Experience Manager網站擷取的資料對應至Adobe Analytics報表套裝中的量度和維度。 此外，已針對量度執行分類，並使用Adobe Analytics的Analysis Workspace功能來建置詳細的報表控制面板。
