---
title: 瞭解不同類型的PDF forms和檔案
description: PDF實際上是一系列檔案格式，本文說明對表單開發人員而言重要且相關的PDF類型。
solution: Experience Manager Forms
product: aem
type: 文件
role: 開發人員
level: 初學者，中級
version: 6.3,6.4,6.5
feature: 文件服務
kt: 7071
topic: 開發
translation-type: tm+mt
source-git-commit: d9799acb28dfc3c9767374798828754d5a50831f
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---


# PDF

可攜式檔案格式(PDF)實際上是一系列的檔案格式，本文詳細說明與表單開發人員最相關的檔案格式。 不同PDF類型的許多技術細節和標準都在不斷變化。 其中一些格式和規格是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產權。

本文說明如何建立各種類型的PDF。 它可協助您瞭解如何及為何使用每一種工具。 所有這些類型在主要用戶端工具(Adobe AcrobatDC)中最適合檢視和使用PDF。

以下是AcrobatDC中的PDF/A檔案範例。

![Pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可從此處下載[](assets/pdf-file-types.zip)

## XMLForms架構PDF

Adobe使用PDF表單一詞來指您使用AEM Forms設計人員建立的互動式動態Forms。 您使用設計人員建立的Forms和檔案以Adobe的XMLForms架構(XFA)為基礎。 在許多方面，XFA PDF檔案格式更接近HTML檔案，而非傳統PDF檔案。 例如，下列程式碼顯示XFA PDF檔案中的簡單文字物件外觀。

![文字欄位](assets/text-field.JPG)

XFAForms是以XML為基礎。 這種結構精良且有彈性的格式可讓AEM Forms伺服器將您的設計人員檔案轉換為不同格式，包括傳統PDF、PDF/A和HTML。 在「設計人員」中，您可以通過選擇「佈局編輯器」的「XML源」頁籤，查看Forms的「完整XML」結構。 您可以在AEM Forms設計師中建立靜態和動態XFAForms。

## 靜態PDF

靜態XFAPDF forms版面在執行時期不會變更，但可讓使用者互動。 以下是靜態XFAPDF forms的一些優點：

* 靜態XFAPDF forms版面在執行時期不會變更，但可讓使用者互動。
* 靜態Forms支援Acrobat的注釋與標籤工具。
* 靜態Forms可讓您匯入和匯出Acrobat注釋。
* 靜態Forms支援字型子設定，這是一種在AEM Forms伺服器上可完成的技術。
* 靜態Forms可使用隨附於現代瀏覽器的內建PDF檢視器來轉換。

>[!NOTE]
>
> 您可以使用AEM Forms設計人員將XDP儲存為Adobe靜態PDF表單，以建立靜態PDF

## PDF格式

可攜式檔案格式(PDF)實際上是一系列的檔案格式，本文詳細說明與表單開發人員最相關的檔案格式。 不同PDF類型的許多技術細節和標準都在不斷變化。 其中一些格式和規格是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產權。

本文說明如何建立各種類型的PDF。 它可協助您瞭解如何及為何使用每一種工具。 所有這些類型在主要用戶端工具(Adobe AcrobatDC)中最適合檢視和使用PDF。

這是AcrobatDC中PDF/A檔案的範例。

![pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可從此處下載[](assets/pdf-file-types.zip)

### XFA PDF

Adobe使用PDF表單一詞來指您使用AEM Forms設計人員建立的互動式動態表單。 請務必注意，有另一種PDF表格，稱為Acroform，與您在AEM Forms設計人員中建立的PDF forms不同。 您使用設計人員建立的表單和檔案以Adobe的XMLForms架構(XFA)為基礎。 在許多方面，XFA PDF檔案格式更接近HTML檔案，而非傳統PDF檔案。 例如，下列程式碼會顯示XFA PDF檔案中的簡單文字物件外觀。

![文本欄位](assets/text-field.JPG)

如您所見，XFA表單是以XML為基礎。 這種結構精良且有彈性的格式可讓AEM Forms伺服器將您的設計人員檔案轉換為不同的格式，包括傳統的PDF、PDF/A和HTML。 在Designer中，您可以選擇「版面編輯器」的「XML來源」標籤，以檢視表單的完整XML結構。 您可以在AEM Forms設計人員中建立靜態和動態XFA表單。

### 靜態PDF

靜態XFAPDF forms在執行時期不會變更其版面，但可讓使用者互動。 以下是靜態XFAPDF forms的一些優點：

* 靜態XFAPDF forms在執行時期不會變更其版面，但可讓使用者互動。
* 靜態表單支援Acrobat的注釋與標籤工具。
* 靜態表單可讓您匯入和匯出Acrobat注釋。
* 靜態表單支援字型子設定，此技術可在AEM Forms伺服器上完成。
* 靜態表格可使用隨附於現代瀏覽器的內建pdf檢視器來轉換。

>[!NOTE]
> 您可以使用AEM Forms設計人員將XDP儲存為Adobe靜態PDF表單，以建立靜態PDF

### 動態Forms

動態XFA PDF可在執行時期變更版面，因此不支援注釋和標籤功能。 不過，動態XFA PDF提供下列優點：

* 動態表單支援用戶端指令碼，可變更表單的版面配置和分頁。 例如，如果您將Purchase Order.xdp儲存為動態表單，Purchase Order.xdp將會擴充並分頁，以容納無數的資料
* 動態表單在執行時期支援表單的所有屬性，而靜態表單只支援子集

>[!NOTE]
>
> 您可以使用AEM Forms設計人員將XDP儲存為Adobe動態XML表單，以建立動態PDF

>[!NOTE]
>
> 無法使用現代瀏覽器的內建pdf檢視器來轉換動態表格。

### PDF檔案（傳統PDF）

「認證檔案」為PDF檔案及Forms收件者提供其真實性與完整性的額外保證。

最常用、最普及的PDF格式是傳統的PDF檔案。 建立傳統PDF檔案的方式有很多，包括使用Acrobat和許多協力廠商工具。 Acrobat提供下列所有方式來建立傳統PDF檔案。 如果您未安裝Acrobat，您可能在電腦上看不到這些選項。

* 擷取案頭應用程式的列印串流：選擇編寫應用程式的「打印」命令，然後選擇Adobe PDF打印機表徵圖。 您將會建立檔案的PDF檔案，而非檔案的印刷本
* 將AcrobatPDFMaker插件與Microsoft Office應用程式一起使用：當您安裝Acrobat時，它會在Microsoft Office應用程式中新增Adobe PDF功能表，並在Office功能區中新增圖示。 您可以使用這些新增功能，直接在Microsoft Office中建立PDF檔案
* 使用Acrobat·Distiller將Postscript和封裝的Postscript(EPS)檔案轉換為PDF:Distiller通常用於印刷出版和其他需要從Postscript格式轉換為PDF格式的工作流程
* 在外觀之下，傳統PDF與XFA PDF非常不同。 它沒有相同的XML結構，而且由於它是透過擷取檔案的列印串流來建立的，所以傳統的PDF是靜態的唯讀檔案。

「認證檔案」為PDF檔案和表單收件者提供其真實性和完整性的額外保證。

### Acroforms

Acroforms是Adobe較舊的互動式表單技術；它們的歷史可以追溯到Acrobat3版。 Adobe提供日期為2003年5月的[AcrobatFormsAPI參考](assets/FormsAPIReference.pdf)，以提供此技術的技術細節。 Acroforms是
下列項目：

* 定義表單靜態版面和圖形的傳統PDF。
* 使用Adobe Acrobat程式的表單工具將互動式表單欄位固定在上方。 這些表單工具是AEM Forms設計人員可用工具的一小部分。

### PDF/A(PDF for Archive)

PDF/A(PDF for Archives)以傳統PDF的檔案儲存優點為基礎，提供許多可增強長期封存的特定詳細資訊。 傳統PDF檔案格式為長期檔案儲存提供許多優點。 PDF的精簡效能有助於輕鬆傳輸並節省空間，而且其結構精良的特性，可提供強大的索引和搜尋功能。 傳統PDF對中繼資料有廣泛的支援，而PDF在支援不同電腦環境方面有悠久的歷史。

和PDF一樣，PDF/A是ISO標準規格。 它由一個工作隊開發，其中包括資訊和影像管理協會(AIIM)、國家印刷設備協會(NPES)和美國法院行政辦公室。 由於PDF/A規格的目標是提供長期封存格式，因此會略過許多PDF功能，讓檔案可自行包含。 以下是提高PDF/A檔案長期可重複性的規格要點：

* 所有內容都必須包含在檔案中，而且不會依賴外部來源，例如超連結、字型或軟體程式。
* 所有字型都必須內嵌，而且必須是擁有電子檔案無限使用授權的字型。
* 不允許JavaScript
* 不允許透明度
* 不允許加密
* 不允許音訊和視訊內容
* 色域必須以不受裝置影響的方式定義
* 所有中繼資料都必須遵循特定標準

### 檢視PDF/A檔案

範例檔案中的兩個檔案是從相同的Microsoft Word檔案建立。 其中一個是以傳統PDF建立，而另一個則以PDF/A檔案建立。 在「Acrobat專業版」中開啟下列兩個檔案：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

雖然檔案的外觀相同，但PDF/A檔案會在上方開啟，並在上方加上藍色列，表示您是以PDF/A模式檢視本檔案。 此藍色列是Acrobat的檔案訊息列，當您開啟特定類型的PDF檔案時，就會看到它。

![Pdf-img](assets/pdfa-message.png)

檔案訊息列包含說明，可能還包括按鈕，可協助您完成工作。 它採用色彩編碼，當您開啟特殊類型的PDF（如此PDF/A檔案）以及經過認證和數位簽章的PDF時，就會看到藍色。 當您參與PDF審核時，橫條會變更為PDF forms的紫色和黃色。

>[!NOTE]
>
> 如果您按一下「啟用編輯」，您就不需要遵循PDF/A規範。




