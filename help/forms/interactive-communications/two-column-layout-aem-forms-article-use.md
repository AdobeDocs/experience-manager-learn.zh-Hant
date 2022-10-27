---
title: 為打印渠道文檔建立兩列佈局
seo-title: Creating two column layouts for print channel documents
description: 為打印通道文檔建立2列佈局
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 打印通道文檔中的兩列佈局

本簡短文章將反白顯示在列通道中建立2列佈局所需的步驟。 使用案例是生成2頁文檔，其中第1頁具有2列佈局，第2頁具有標準的1列佈局。

以下是使用AEM Forms Designer建立2欄配置時涉及的高階步驟。

* 在第1頁母版頁中建立2個內容區域
* 將2個內容區域命名為「leftcolumn」和「rightcolumn」
* 使用一個內容區域建立第二個主版頁面（此為預設）
* 選取分頁標籤（無標題子表單）（第1頁）和（無標題子表單）（第2頁），然後設定屬性，如下方螢幕擷取畫面所示。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

設定分頁屬性後，我們就可以在下方新增子表單或目標區域（無標題子表單）（第1頁）。

然後，我們可以將檔案片段新增至這些子表單或目標區域。 當左欄已滿時，內容會流向右欄。

若要在本機伺服器上測試，請下載與本文相關的資產。 向下捲動到此頁底部

* [使用封裝管理程式下載及安裝範例列印管道檔案](assets/print-channel-with-two-column-layout.zip)
* [預覽打印通道文檔](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
