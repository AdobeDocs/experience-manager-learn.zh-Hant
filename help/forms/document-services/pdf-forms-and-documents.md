---
title: 了解不同類型的PDF forms和檔案
description: PDF實際上是一系列檔案格式，本文說明對表單開發人員來說重要且相關的PDF類型。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF 產生器
kt: 7071
topic: 開發
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 0%

---


# PDF

可攜式檔案格式(PDF)其實是一系列檔案格式，本文詳細說明與表單開發人員最相關的格式。 不同PDF類型的許多技術細節和標準都在不斷變化。 其中一些格式和規格是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產權。

本文說明如何建立各種類型的PDF。 這可協助您了解使用每個工具的方式及原因。 在檢視及使用PDF的主要用戶端工具(Adobe Acrobat DC)中，所有這些類型都最能運作。

以下是Acrobat DC中的PDF/A檔案範例。

![Pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可從此處[下載](assets/pdf-file-types.zip)

## Xml Forms Architecture PDF

Adobe使用PDF表單一詞來指代您使用AEM Forms Designer建立的互動式動態Forms。 您使用Designer建立的Forms和檔案是以Adobe的XML Forms架構(XFA)為基礎。 在許多方面，XFA PDF檔案格式更接近HTML檔案，而非傳統PDF檔案。 例如，下列程式碼會顯示簡單文字物件在XFA PDF檔案中的外觀。

![文字欄位](assets/text-field.JPG)

XFA Forms是以XML為基礎。 這種結構精良且有彈性的格式可讓AEM Forms伺服器將您的Designer檔案轉換為不同格式，包括傳統PDF、PDF/A和HTML。 通過選擇「佈局編輯器」的「XML源」頁簽，您可以在設計器中查看Forms的「完整XML結構」。 您可以在AEM Forms Designer中建立靜態和動態XFA Forms。

## 靜態PDF

靜態XFAPDF forms版面配置在執行階段不會變更，但可供使用者互動。 以下是靜態XFAPDF forms的幾項優點：

* 靜態XFAPDF forms版面配置在執行階段不會變更，但可供使用者互動。
* 靜態Forms支援Acrobat的註解和標籤工具。
* 靜態Forms可讓您匯入和匯出Acrobat註解。
* 靜態Forms支援字型子設定，此技術可在AEM Forms伺服器上完成。
* 可使用現代瀏覽器隨附的內建PDF檢視器來轉譯靜態Forms。

>[!NOTE]
>
> 您可以使用AEM Forms Designer將XDP儲存為Adobe靜態PDF表單，以建立靜態PDF

## PDF格式

可攜式檔案格式(PDF)其實是一系列檔案格式，本文詳細說明與表單開發人員最相關的格式。 不同PDF類型的許多技術細節和標準都在不斷變化。 其中一些格式和規格是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產權。

本文說明如何建立各種類型的PDF。 這可協助您了解使用每個工具的方式和原因。 在檢視及使用PDF的主要用戶端工具(Adobe Acrobat DC)中，所有這些類型都最能運作。

這是Acrobat DC中的PDF/A檔案範例。

![pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可從此處[下載](assets/pdf-file-types.zip)

### XFA PDF

Adobe使用PDF表單一詞來指您使用AEM Forms Designer建立的互動式和動態表單。 請務必注意，有另一種PDF表單，稱為Acroform，與您在AEM Forms Designer中建立的PDF forms不同。 您使用Designer建立的表單和檔案以Adobe的XML Forms架構(XFA)為基礎。 在許多方面，XFA PDF檔案格式更接近HTML檔案，而非傳統PDF檔案。 例如，下列程式碼會顯示簡單文字物件在XFA PDF檔案中的外觀。

![文本欄位](assets/text-field.JPG)

如您所見，XFA表單是以XML為基礎。 這種結構精良且有彈性的格式可讓AEM Forms伺服器將您的Designer檔案轉換為不同格式，包括傳統PDF、PDF/A和HTML。 通過選擇「佈局編輯器」的「XML源」頁簽，您可以在設計器中查看表單的完整XML結構。 您可以在AEM Forms Designer中建立靜態和動態XFA表單。

### 靜態PDF

靜態XFAPDF forms在執行階段不會變更其版面，但可供使用者互動。 以下是靜態XFAPDF forms的幾項優點：

* 靜態XFAPDF forms在執行階段不會變更其版面，但可供使用者互動。
* 靜態表單支援Acrobat的註解和標籤工具。
* 靜態表單可讓您匯入和匯出Acrobat註解。
* 靜態表單支援字型子設定，此技術可在AEM Forms伺服器上完成。
* 您可以使用隨附現代瀏覽器的內建pdf檢視器來轉譯靜態表單。

>[!NOTE]
> 您可以使用AEM Forms Designer將XDP儲存為Adobe靜態PDF表單，以建立靜態PDF

### 動態Forms

動態XFA PDF可在執行階段變更版面，因此不支援註解和標籤功能。 不過，動態XFA PDF具備下列優點：

* 動態表單支援用戶端指令碼，可變更表單的版面配置和分頁。 例如，如果您將Purchase Order.xdp儲存為動態表單，它將擴展和分頁，以容納無數的資料量
* 動態表單在執行階段支援表單的所有屬性，而靜態表單僅支援子集

>[!NOTE]
>
> 您可以使用AEM Forms Designer建立動態PDF，方法是將XDP儲存為Adobe動態XML表單

>[!NOTE]
>
> 無法使用現代瀏覽器內建的pdf檢視器來轉譯動態表單。

### PDF檔案（傳統PDF）

認證檔案可為PDF檔案和Forms收件者提供其真實性和完整性的額外保證。

最常用且最普遍的PDF格式是傳統PDF檔案。 建立傳統PDF檔案的方式有很多，包括使用Acrobat和許多協力廠商工具。 Acrobat提供以下所有建立傳統PDF檔案的方式。 如果您未安裝Acrobat，您可能在電腦上看不到這些選項。

* 通過捕獲案頭應用程式的打印流：選擇創作應用程式的「打印」命令，然後選擇Adobe PDF打印機表徵圖。 您將建立文檔的PDF檔案，而不是打印的文檔副本
* 將Acrobat PDFMaker外掛程式與Microsoft Office應用程式搭配使用：安裝Acrobat時，它會向Microsoft Office應用程式添加一個Adobe PDF菜單，並向Office功能區添加一個表徵圖。 您可以直接在Microsoft Office中使用這些新增功能建立PDF檔案
* 使用Acrobat Distiller將Postscript和封裝的Postscript(EPS)檔案轉換為PDF:Distiller通常用於列印發佈和其他需要從Postscript格式轉換為PDF格式的工作流程
* 在外套下，傳統PDF與XFA PDF非常不同。 它沒有相同的XML結構，而且由於它是通過捕獲檔案的打印流而建立的，因此傳統的PDF是靜態的只讀檔案。

認證檔案為PDF檔案和收件者提供其真實性和完整性的額外保證。

### Acroforms

Acroforms是Adobe較舊的互動式表單技術；其日期可回溯至Acrobat 3版。 Adobe提供日期為2003年5月的[Acrobat Forms API參考](assets/FormsAPIReference.pdf)，以提供此技術的技術詳細資訊。 Acroforms是
下列項目：

* 定義表單靜態版面和圖形的傳統PDF。
* 使用Adobe Acrobat程式的表單工具將互動式表單欄位固定在上方。 這些表單工具只是AEM Forms Designer中可用功能的一小部分。

### PDF/A(PDF for Archive)

PDF/A(PDF for Archives)以傳統PDF的檔案儲存優點為基礎，提供許多可增強長期歸檔的具體細節。 傳統的PDF檔案格式為長期檔案儲存提供許多優點。 PDF的緊湊性有助於輕鬆傳輸和節省空間，而且其結構完善，可提供強大的索引和搜索功能。 傳統PDF對中繼資料有廣泛的支援，而PDF在支援不同電腦環境方面有著悠久的歷史。

與PDF一樣，PDF/A也是ISO標準規範。 它由一個工作組開發，其中包括資訊和影像管理協會、國家印刷設備協會和美國法院行政辦公室。 由於PDF/A規範的目標是提供長期的封存格式，因此會忽略許多PDF功能，以便檔案可自行包含。 以下是提高PDF/A檔案長期可重複性的規範的一些關鍵點：

* 檔案中必須包含所有內容，並且不能依賴外部源，如超連結、字型或軟體程式。
* 所有字型都必須嵌入，並且它們必須是具有電子文檔無限使用許可的字型。
* 不允許JavaScript
* 不允許透明
* 不允許加密
* 不允許音頻和視頻內容
* 必須以獨立於設備的方式定義顏色空間
* 所有中繼資料都必須遵循特定標準

### 檢視PDF/A檔案

範例檔案中的兩個檔案是從同一個Microsoft Word檔案建立的。 一個是以傳統PDF建立，另一個是PDF/A檔案。 在Acrobat Professional中開啟以下兩個檔案：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

儘管文檔「看起來相同」，但PDF/A檔案會在頂部以藍色條開啟，表示您正以PDF/A模式查看此文檔。 此藍色列是Acrobat的檔案訊息列，您在開啟特定類型的PDF檔案時會看到此訊息列。

![Pdf-img](assets/pdfa-message.png)

文檔消息欄包括幫助您完成任務的說明，可能還包括按鈕。 它以色碼編碼，當您開啟特殊類型的PDF（如此PDF/A檔案）以及經認證和數位簽署的PDF時，會看到藍色。 當您參與PDF審核時，條狀會變為紫色(代表PDF forms)和黃色（代表參加PDF審核）。

>[!NOTE]
>
> 如果按一下「啟用編輯」，您會將此檔案從PDF/A法規遵循性中移除。




