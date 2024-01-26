---
title: 為列印管道檔案建立兩個欄配置
description: 為列印管道檔案建立2欄版面配置
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# Print Channel檔案中的兩欄配置

這篇簡短文章將重點介紹在列印管道中建立2欄版面配置所需的步驟。 使用案例是產生2頁檔案，其中第1頁具有2欄配置，而第2頁具有標準1欄配置。

以下是使用AEM Forms Designer建立2欄配置的相關高階步驟。

* 在頁面1主版頁面中建立2個內容區域
* 將2個內容區域命名為「leftcolumn」和「rightcolumn」
* 建立一個具有一個內容區域的第二個主版頁面（此為預設值）
* 選取分頁標籤（未命名的子表單） （第1頁）和（未命名的子表單） （第2頁），並設定屬性，如下列熒幕擷取畫面所示。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

一旦設定分頁屬性，我們就可以在（無標題的子表單） （第1頁）底下新增子表單或目標區域。

然後，我們可以將檔案片段新增到這些子表單或目標區域。 當左欄已滿時，內容將會流向右欄。

若要在本機伺服器上測試此專案，請下載與本文相關的資產。 向下捲動至此頁面底部

* [使用封裝管理員下載並安裝範例Print Channel Document](assets/print-channel-with-two-column-layout.zip)
* [預覽Print Channel檔案](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
