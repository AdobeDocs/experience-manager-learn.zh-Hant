---
title: 頁面範本
description: 瞭解如何建立和修改頁面範本。 瞭解頁面範本和頁面之間的關係。 瞭解如何設定頁面範本的原則，為內容提供精細的控管和品牌一致性。  根據Adobe XD的模型建立結構良好的雜誌文章範本。
version: Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1593
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# 頁面範本 {#page-templates}

在本章中，我們將探索頁面範本與頁面之間的關係。 我們將根據中的某些模型，建置無樣式的雜誌文章範本 [AdobeXD](https://www.adobe.com/products/xd.html). 在建置範本的過程中，將涵蓋核心元件和進階原則設定。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [製作內容與發佈變更](./author-content-publish.md) 章節已完成。

## 目標

1. 瞭解頁面範本的詳細資料，以及如何使用原則來強制精細控制頁面內容。
1. 瞭解範本和頁面的連結方式。
1. 建立新範本並編寫頁面。

## 您將建置的內容 {#what-you-will-build}

在教學課程的這個部分，您將建立一個新的Magazine文章頁面範本，可用來建立新的雜誌文章，並與通用結構保持一致。 範本是根據設計和在AdobeXD中產生的UI套件。 本章只著重於建置範本的結構或骨架。 未實作任何樣式，但範本和頁面運作正常。

## 建立Magazine文章頁面範本

建立頁面時，您必須選取範本，作為建立新頁面的基礎。 範本會定義結果頁面的結構、初始內容和允許的元件。

有3個主要區域 [頁面範本](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)：

1. **結構**  — 定義屬於範本一部分的元件。 內容作者無法編輯這些內容。
1. **初始內容**  — 定義範本開始使用的元件，內容作者可以編輯和/或刪除這些元件
1. **原則**  — 定義元件行為方式的設定，以及作者可用的選項。

接下來，在AEM中建立符合模型結構的新範本。 這會在AEM的本機執行個體中發生。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

您可以使用以下縮圖來識別您的範本（或上傳您自己的範本！）

![文章頁面範本縮圖](./assets/page-templates/article-page-template-thumbnail.png)


### 解決方案套件

已完成 [Magazine範本的解決方案](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) 可透過「封裝管理員」下載及安裝。

## 使用體驗片段更新頁首和頁尾 {#experience-fragments}

建立全域內容（例如頁首或頁尾）的常見作法是使用 [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). 體驗片段，可讓使用者結合多個元件，以建立單一可參考的元件。 體驗片段的優點在於支援多網站管理和 [本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

網站範本產生頁首和頁尾。 接下來，更新體驗片段以符合模型。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

以下影片的高層級步驟：

1. 下載範例內容套件 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. 使用封裝管理員上傳及安裝內容封裝。
1. 更新頁首和頁尾體驗片段以使用WKND標誌

## 建立Magazine文章頁面

接下來，使用Magazine文章頁面範本建立新頁面。 編寫頁面內容以符合網站模型。 請依照下列影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

使用 [提供的文字](./assets/page-templates/la-skateparks-copy.txt) 以填入文章內文。

## 恭喜！ {#congratulations}

恭喜，您剛才已使用Adobe Experience Manager Sites建立新範本和頁面。

### 後續步驟 {#next-steps}

此時，雜誌文章頁面和網站不符合WKND的品牌樣式。 請遵循 [佈景主題](theming.md) 此教學課程將學習更新CSS和Javascript前端程式碼的最佳作法，這些程式碼用於將全域樣式套用至網站。

### 解決方案套件

本章的解決方案套件可供下載： [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
