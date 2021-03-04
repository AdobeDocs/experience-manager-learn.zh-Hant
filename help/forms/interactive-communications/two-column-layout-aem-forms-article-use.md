---
title: 為列印渠道檔案建立兩欄版面
seo-title: 為列印渠道檔案建立兩欄版面
description: 為列印頻道檔案建立2欄版面
seo-description: 為列印頻道檔案建立2欄版面
feature: 互動式通訊
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 2%

---


# 列印頻道檔案中的兩欄版面

此簡短文章將反白顯示在列印頻道中建立2欄版面所需的步驟。 使用案例是產生2頁檔案，其中第1頁具有2欄版面，第2頁具有標準1欄版面。

以下是使用AEM Forms設計人員建立2欄版面時的高階步驟。

* 在第1頁主版頁面中建立2個內容區域
* 將2個內容區域命名為「leftcolumn」和「rightcolumn」
* 使用一個內容區域建立第二個主版頁面（這是預設）
* 選取「分頁」標籤（無標題子表單）（第1頁）和（無標題子表單）（第2頁），並設定下列螢幕擷取畫面中顯示的屬性。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

設定分頁屬性後，我們就可以在（未命名的子表單）（第1頁）下新增子表單或目標區域。

然後，我們可以將檔案片段新增至這些子表單或目標區域。 當左欄已滿時，內容會流向右欄。

若要在您的本機伺服器上測試此項，請下載與本文相關的資產。 向下捲動至此頁面底部

* [使用套件管理器下載並安裝範例列印頻道檔案](assets/print-channel-with-two-column-layout.zip)
* [預覽列印頻道檔案](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
