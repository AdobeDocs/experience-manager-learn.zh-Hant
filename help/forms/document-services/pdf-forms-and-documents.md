---
title: 瞭解不同型別的PDF forms和檔案
description: PDF實際上是一系列檔案格式，本文會說明對表單開發人員而言重要和相關的PDF型別。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
jira: KT-7071
topic: Development
last-substantial-update: 2020-07-07T00:00:00Z
duration: 336
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# PDF

可攜式檔案格式(PDF)實際上是一系列檔案格式，本文會詳細說明與表單開發人員最相關的格式。 各種PDF型別的許多技術細節和標準都在不斷演化和變更。 這些格式和規格中有些是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產。

本文會說明如何建立各種型別的PDF。 它有助於您瞭解如何使用及為何要使用各種。 所有這些型別在用於檢視和使用PDF的Premier Client工具(Adobe Acrobat DC)中運作得最好。

以下是Acrobat DC中PDF/A檔案的範例。

![Pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可以是 [已從此處下載](assets/pdf-file-types.zip)

## XML Forms架構PDF(XFAPDF)

Adobe使用術語XFAPDF表單，來指稱您使用AEM Forms Designer建立的互動式和動態Forms。 您使用Designer建立的Forms和檔案是以Adobe的XML Forms架構(XFA)為基礎。 在許多方面，XFAPDF檔案格式比HTML檔案更接近傳統PDF檔案。 例如，下列程式碼會向您展示XFAPDF檔案中簡單文字物件的外觀。

![文字欄位](assets/text-field.JPG)

XFA Forms以XML為基礎。 這種良好的結構和彈性的格式可讓AEM Forms伺服器將您的設計工具檔案轉換為不同的格式，包括傳統的PDF、PDF/A和HTML。 您可以選取「版面配置編輯器」的「XML來源」標籤，在Designer中檢視Forms的完整XML結構。 您可以在AEM Forms Designer中建立靜態和動態XFA Forms。

## 靜態PDF

靜態XFAPDF forms配置在執行階段從不變更，但可為使用者提供互動式。 以下是靜態XFAPDF forms的一些優點：

* 靜態XFAPDF forms配置在執行階段從不變更，但可為使用者提供互動式。
* 靜態Forms支援Acrobat的評論和標籤工具。
* 靜態Forms可讓您匯入和匯出Acrobat註解。
* 靜態Forms支援字型子設定，此技術可在AEM Forms伺服器上完成。
* 靜態Forms可使用隨新式瀏覽器提供的內建PDF檢視器呈現。

>[!NOTE]
>
> 您可以使用AEM Forms Designer建立靜態PDF，方法是將XDP儲存為Adobe靜態PDF表單



### 動態Forms

動態XFAPDF可在執行階段變更其配置，因此不支援註釋和標籤功能。 不過，動態XFAPDF可提供下列優點：

* 動態表單支援可變更表單版面配置和分頁的使用者端指令碼。 例如，如果您將Purchase Order.xdp儲存為動態表單，它會展開並分頁，以因應無數的資料量
* 動態表單在執行階段支援表單的所有屬性，而靜態表單僅支援子集

* [請參閱本檔案以瞭解靜態與動態pdf表單之間的差異](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/pdf-forms-and-documents.html#:~:text=Dynamic%20forms%20support%20all%20the,forms%20support%20only%20a%20subset)

>[!NOTE]
>
> 您可以使用AEM Forms Designer建立動態PDF，方法是將XDP儲存為Adobe動態XML表單

>[!NOTE]
>
> 動態表單無法使用現代瀏覽器的內建pdf檢視器呈現。

### PDF檔案(傳統PDF)

認證檔案可為PDF檔案和Forms收件者提供其真實性和完整性的額外保證。

最受歡迎和普及的PDF格式是傳統的PDF檔案。 建立傳統PDF檔案有許多方式，包括使用Acrobat和許多協力廠商工具。 Acrobat提供下列所有方法來建立傳統PDF檔案。 如果您未安裝Acrobat，則電腦上可能看不到這些選項。

* 擷取案頭應用程式的列印資料流：選擇製作應用程式的「列印」指令，並選取Adobe PDF印表機圖示。 您將建立檔案的PDF檔案，而不是列印的檔案復本
* 搭配Microsoft Office應用程式使用Acrobat PDFMaker外掛程式：安裝Acrobat時，它會將Adobe PDF功能表新增至Microsoft Office應用程式，並將圖示新增至Office功能區。 您可以使用這些新增的功能，直接在Microsoft Office中建立PDF檔案
* 使用Acrobat Distiller將Postscript和封裝的Postscript (EPS)檔案轉換為PDF：Distiller通常用於列印發佈和其他需要從Postscript格式轉換為PDF格式的工作流程
* 根本上，傳統PDF與XFAPDF非常不同。 它沒有相同的XML結構，而且由於它是透過擷取檔案的列印資料流所建立，所以傳統的PDF是靜態且唯讀的檔案。

認證檔案可為PDF檔案及表單收件者提供其真實性和完整性的額外保證。

### Acroforms

Acroform是Adobe的舊版互動式表單技術，可追溯至Acrobat第3版。 Adobe提供 [Acrobat Forms API參考](assets/FormsAPIReference.pdf)日期為2003年5月，目的是提供此技術的技術細節。 Acroform是下列專案的組合：

* 定義表單靜態版面和圖形的傳統PDF。
* 使用Adobe Acrobat程式的表單工具錨定在頂端的互動式表單欄位。 這些表單工具是AEM Forms Designer中可用功能的一小部分。

### PDF/A (封存的PDF)

PDF/A (封存PDF)以傳統PDF的檔案儲存優勢為基礎，並包含許多可強化長期封存的特定細節。 傳統的PDF檔案格式為長期檔案儲存提供了許多好處。 PDF的精簡特性有助於輕鬆傳輸並節省空間，而其結構良好的特性可啟用強大的索引和搜尋功能。 傳統PDF廣泛支援中繼資料，而PDF在支援不同電腦環境方面也有悠久的歷史。

就像PDF一樣，PDF/A是ISO標準規格。 它由包括AIIM （資訊與影像管理協會）、NPES （國家印刷裝置協會）和美國法院行政辦公室在內的工作組所開發。 由於PDF/A規格的目標是提供長期的封存格式，因此會省略許多PDF功能，讓檔案可以自包含。 以下是有關規格的幾個要點，這些要點可增強PDF/A檔案的長期可重複性：

* 所有內容都必須包含在檔案中，而且外部來源（例如超連結、字型或軟體程式）可能不具任何相依性。
* 所有字型都必須內嵌，而且必須是擁有電子檔案無限制使用許可的字型。
* 不允許JavaScript
* 不允許透明度
* 不允許加密
* 不允許音訊和視訊內容
* 必須以與裝置無關的方式定義色域
* 所有中繼資料都必須遵循特定標準

### 檢視PDF/A檔案

範例檔案中的兩個檔案是從相同的Microsoft Word檔案建立的。 其中一個是以傳統PDF建立，另一個則以PDF/A檔案建立。 在Acrobat Professional中開啟這兩個檔案：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

雖然檔案看起來相同，但PDF/A檔案會以藍色列跨過頂端開啟，表示您正在以PDF/A模式檢視此檔案。 這個藍色列是Acrobat的檔案訊息列，當您開啟特定型別的PDF檔案時，就會看到此訊息列。

![Pdf-img](assets/pdfa-message.png)

檔案訊息列包含指示，以及可能有助於您完成工作的按鈕。 它標示有色彩，當您開啟特殊型別的PDF(如此PDF/A檔案)以及經認證和數位簽署的PDF時，您會看到藍色。 當您參與PDF稽核時，PDF forms的橫條會變更為紫色，而黃色會變成。

>[!NOTE]
>
> 如果按一下「啟用編輯」，您會將此檔案從PDF/A規範中移除。
