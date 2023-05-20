---
title: 為打印渠道文檔建立兩列佈局
seo-title: Creating two column layouts for print channel documents
description: 為打印渠道文檔建立2列佈局
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

# 打印渠道文檔中的兩列佈局

此簡短文章將重點介紹在打印通道中建立2列佈局所需的步驟。 用例是生成2頁文檔，頁1具有2列佈局，頁2具有標準1列佈局。

以下是使用AEM Forms設計器建立2列佈局時涉及的高級步驟。

* 在第1頁母版頁中建立2個內容區域
* 將2個內容區域命名為「leftcolumn」和「rightcolumn」
* 使用一個內容區域建立第二個母版頁（這是預設值）
* 選擇「分頁」頁籤（無標題子表單）（第1頁）和（無標題子表單）（第2頁），並設定如下螢幕抓圖所示的屬性。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

設定分頁屬性後，我們可以在（無標題子表單）（第1頁）下添加子表單或目標區域。

然後，我們可以將這些子窗體或目標區域添加文檔片段。 當左欄已滿時，內容將流向右欄。

要在本地伺服器上test此項，請下載與本文相關的資產。 向下滾動到此頁的底部

* [使用包管理器下載並安裝示例打印通道文檔](assets/print-channel-with-two-column-layout.zip)
* [預覽打印渠道文檔](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
