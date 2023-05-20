---
title: 瞭解不同類型的PDF forms和文檔
description: PDF實際上是一系列檔案格式，本文描述了對表單開發者來說重要且相關的PDF類型。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

可移植文檔格式(PDF)實際上是一系列檔案格式，本文詳細介紹了最適合表單開發者的格式。 許多不同PDF類型的技術細節和標準都在不斷演變和變化。 這些格式和規格中有些是國際標準化組織(ISO)標準，有些是Adobe擁有的特定智慧財產權。

本文介紹如何建立各種類型的PDF。 它有助於你理解如何和為什麼使用每一個。 所有這些類型在用於查看和使用PDF的Premier客戶端工具(Adobe AcrobatDC)中最有效。

以下是Acrobat DC的PDF/A檔案示例。

![普德法](assets/pdfa-file-in-acrobat.png)

示例檔案可以是 [從此處下載](assets/pdf-file-types.zip)

## XMLForms體系結構PDF(XFAPDF)

Adobe使用術語XFAPDF表單來指您使用AEM Forms設計器建立的互動式和動態Forms。 使用設計器建立的Forms和檔案基於Adobe的XMLForms體系結構(XFA)。 在很多方面， XFAPDF檔案格式比傳統HTML檔案更接近PDF檔案。 例如，以下代碼顯示了XFAPDF檔案中簡單文本對象的外觀。

![文本欄位](assets/text-field.JPG)

XFAForms是基於XML的。 此結構良好且靈活的格式使AEM Forms伺服器能夠將您的設計器檔案轉換為不同的格式，包括傳統PDF、PDF/A和HTML。 通過選擇佈局編輯器的「XML源」頁籤，可以在設計器中查看Forms的「完整XML」結構。 可以在AEM Forms設計器中建立靜態和動態XFAForms。

## 靜態PDF

靜態XFAPDF forms佈局在運行時不會更改，但對用戶來說，它們可以是互動式的。 以下是靜態XFAPDF forms的幾個優點：

* 靜態XFAPDF forms佈局在運行時不會更改，但對用戶來說，它們可以是互動式的。
* 靜態Forms支援Acrobat的注釋和標籤工具。
* 靜態Forms使您能夠導入和導出Acrobat注釋。
* 靜態Forms支援字型子設定，該技術可在AEM Forms伺服器上完成。
* 使用現代瀏覽器附帶的內置PDF查看器可以呈現靜態Forms。

>[!NOTE]
>
> 通過將XDP另存為PDF靜態PDF窗體，可以使用AEM Forms設計器建立靜態Adobe



### 動態Forms

動態XFAPDF可以在運行時更改其佈局，因此不支援注釋和標籤功能。 但是，動態XFAPDF確實提供了以下優勢：

* 動態表單支援更改表單佈局和分頁的客戶端指令碼。 例如，如果將Purchase Order.xdp另存為動態表單，它將擴展並分頁以容納無數資料
* 動態表單在運行時支援表單的所有屬性，而靜態表單只支援子集

>[!NOTE]
>
> 通過將XDP另存為Adobe動態XML表單，可以使用AEM Forms設計器建立動態PDF

>[!NOTE]
>
> 無法使用現代瀏覽器的內置pdf查看器來呈現動態表單。

### PDF檔案(傳統PDF)

認證文檔為PDF文檔和Forms收件人提供了對其真實性和完整性的額外保證。

最常見、最普遍的PDF格式是傳統的PDF檔案。 建立傳統PDF檔案有多種方法，包括使用Acrobat和許多第三方工具。 Acrobat提供了以下所有建立傳統PDF檔案的方法。 如果您沒有安裝Acrobat，則您可能在電腦上看不到這些選項。

* 通過捕獲案頭應用程式的打印流：選擇創作應用程式的「打印」命令，然後選擇Adobe PDF打印機表徵圖。 您將建立文檔的PDF檔案，而不是打印文檔的副本
* 通過將AcrobatPDFMaker插件與MicrosoftOffice應用程式配合使用：安裝Acrobat時，它會向MicrosoftOffice應用程式添加一個Adobe PDF菜單，向Office功能區添加一個表徵圖。 您可以使用這些添加的功能直接在Microsoft辦公室建立PDF檔案
* 使用Acrobat·Distiller將Postscript和封裝的Postscript(EPS)檔案轉換為PDF:Distiller通常用於打印發佈和其他需要從Postscript格式轉換為PDF格式的工作流
* 在引擎蓋下，傳統PDF與XFAPDF截然不同。 它沒有相同的XML結構，而且由於它是通過捕獲檔案的打印流建立的，因此傳統的PDF是靜態的只讀檔案。

認證文檔向PDF文檔和表單收件人提供對其真實性和完整性的附加保證。

### 頂體

頂體是Adobe較老的互動式形式技術；它們的歷史可以追溯到Acrobat版3。 Adobe提供 [AcrobatFormsAPI參考](assets/FormsAPIReference.pdf)日期為2003年5月，提供此技術的技術細節。 頂框是以下項的組合：

* 定義表單靜態佈局和圖形的傳統PDF。
* 使用Adobe Acrobat程式的表單工具將交互表單域固定在頂部。 這些表單工具是AEM Forms設計器中可用工具的一小部分。

### PDF/A(存檔PDF)

PDF/A(存檔PDF)以傳統PDF的文檔儲存優勢為基礎，具有許多增強長期存檔的具體細節。 傳統的PDF檔案格式為長期文檔儲存提供了許多好處。 PDF的緊湊效能方便了傳輸並節約了空間，而其結構良好的特性則支援強大的索引和搜索功能。 傳統PDF對元資料有廣泛的支援，而PDF在支援不同的電腦環境方面有著悠久的歷史。

與PDF一樣，PDF/A是ISO標準規範。 它由一個包括資訊和影像管理協會(AIIM)、國家印刷設備協會(NPES)和美國法院行政辦公室在內的工作隊開發。 由於PDF/A規範的目標是提供長期存檔格式，因此會省略許多PDF功能，以便可以自包含檔案。 以下是提高PDF/A檔案長期再現性的規範的關鍵點：

* 所有內容都必須包含在檔案中，並且不能依賴於外部源，如超連結、字型或軟體程式。
* 所有字型都必須嵌入，並且它們必須是具有電子文檔無限使用許可證的字型。
* 不允許使用JavaScript
* 不允許透明
* 不允許加密
* 不允許使用音頻和視頻內容
* 必須以與設備無關的方式定義顏色空間
* 所有元資料必須遵循某些標準

### 查看PDF/A檔案

示例檔案中的兩個檔案是從同一個MicrosoftWord檔案建立的。 一個建立為傳統PDF，另一個建立為PDF/A檔案。 在Acrobat專業人員中開啟以下兩個檔案：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

儘管文檔外觀相同，但PDF/A檔案在頂部以藍色條形開啟，表明您正在PDF/A模式下查看此文檔。 此藍色條是Acrobat的文檔消息欄，當您開啟某些類型的PDF檔案時，會看到此欄。

![PDF-img](assets/pdfa-message.png)

文檔消息欄包含幫助您完成任務的說明，可能還包含按鈕。 它是彩色編碼的，當您開啟特殊類型的PDF(如此PDF/A檔案)以及經認證和數字簽名的PDF時，您會看到藍色。 在您參與PDF forms評審時，條變為紫色，變為黃色。

>[!NOTE]
>
> 如果按一下「啟用編輯」，則將此文檔從PDF/A符合性中刪除。
